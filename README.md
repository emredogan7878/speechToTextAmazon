# speechToTextAmazon
Speech to text usage report with Amazon Transcribe and Python

![githubamazon1](https://github.com/iremibis/SpeechToText/assets/112003747/1f572876-9864-4445-b028-33f37e795837)
* First open an aws account.
* Create S3 Bucket. In this Bucket put the audio file we want to translate.
* I have uploaded a 50-second speech file of South Park character Craig Tucker.
* As you can see above, you can upload the desired audio file to S3 Bucket by clicking the upload button.
* Use S3 bucket in global region.

![githubamazon2](https://github.com/iremibis/SpeechToText/assets/112003747/9520ef97-00ab-49de-bfb6-d4f49d3724fc)
* Create a Transcription Job here by going to Amazon Transcribe > Create Transcription Job.
* All of the options that come up when creating will be in default settings.
* Transcription jobs appear here as you can see in the screenshot above. The first time you create it, it will appear empty.
* Every time we run the Python code, it will generate a new example job and appear here.
* Be careful in which region you use Transcription Job. (For example, if you choose eu-central-1 (Frankfurt), you must specify this in the code section.)

![githubamazon3](https://github.com/iremibis/SpeechToText/assets/112003747/5a408b02-f2a2-4adc-b4a2-64820c2fb337)
* Now create an aws IAM account.
* From the IAM Dashboard, navigate to the Users section and click the Add Users button.
* Select the options indicated by the red boxes. This will give us access.
* Click the Next button at the bottom right.
* Use in global region.

![githubamazon4](https://github.com/iremibis/SpeechToText/assets/112003747/a35766b7-04d3-4dee-aa65-2629ca30b825)
* On this page, click the Create User Button shown in the red box.

![githubamazon5](https://github.com/iremibis/SpeechToText/assets/112003747/3b8490bb-8c36-493e-a295-7b367254ae7c)
* Go to the Security Credentials tab with the red box.

![githubamazon6](https://github.com/iremibis/SpeechToText/assets/112003747/cb82bbff-2a47-4418-9666-490e3e5c9823)
* Click on the Create Access Key button, indicated by the red box.

![githubamazon7](https://github.com/iremibis/SpeechToText/assets/112003747/198d497b-ed13-49a8-a36e-ae525f2faa22)
* Select the CLI option to allow and provide command line interface access to our AWS account.
* Tick the box to declare that we accept alternative suggestions.
* Press the Next button

![githubamazon8](https://github.com/iremibis/SpeechToText/assets/112003747/25853644-bfa7-4e36-8ca7-4dce19a92177)
* It wants us to set Description Tag on this page. The description of this access key is attached to the user as a label and is displayed next to the access key.
* This is an optional option, you can set it if you want. Be careful to make the explanation properly. A good explanation will help you safely return this access key later.
* Click the Create Access Key button.

![githubamazon9](https://github.com/iremibis/SpeechToText/assets/112003747/551d1d7e-e16b-4410-8594-81ee34e431ac)
* Access key and Secret access key were created on this page.
* If you lose or forget Access key and Secret access key, you cannot get it back. Instead, create a new access key and disable the old key.
* You can download the keys to your computer by clicking the Download .csv file button shown in the red box, so you do not lose them.
* Write the keys in the python terminal and connect them with the code.

# Importing keys.
Open your terminal and run the following command:
* export AWS_ACCESS_KEY_ID=ACCESS KEYİNİZ
* export AWS_SECRET_ACCESS_KEY=SECRET ACCESS KEYİNİZ

# PYTHON CODE BLOCK :

```python
import time
import boto3
import requests

def get_transcript_text(json_url):
    response = requests.get(json_url)
    if response.status_code == 200:
        json_data = response.json()
        transcript_text = json_data['results']['transcripts'][0]['transcript']
        return transcript_text
    else:
        print("Failed to fetch transcript text.")
        return None

def transcribe_file(job_name, file_uri, transcribe_client):
    transcribe_client.start_transcription_job(
        TranscriptionJobName=job_name,
        Media={
            'MediaFileUri': file_uri
        },
        MediaFormat='mp3',
        LanguageCode='en-US'
    )

    max_tries = 60
    while max_tries > 0:
        max_tries -= 1
        job = transcribe_client.get_transcription_job(TranscriptionJobName=job_name)
        job_status = job['TranscriptionJob']['TranscriptionJobStatus']
        if job_status in ['COMPLETED', 'FAILED']:
            print(f"Job {job_name} is {job_status}.")
            if job_status == 'COMPLETED':
                transcript_file_uri = job['TranscriptionJob']['Transcript']['TranscriptFileUri']
                transcript_text = get_transcript_text(transcript_file_uri)
                if transcript_text:
                    print("Transcript Text:")
                    print(transcript_text)
            break
        else:
            print(f"Waiting for {job_name}. Current status is {job_status}.")
        time.sleep(10)

def main():
    transcribe_client = boto3.client('transcribe', region_name='eu-central-1')
    file_uri = 's3://transcribebucket78/speech.mp3'
    transcribe_file("Example-job1", file_uri, transcribe_client)

if __name__ == '__main__':
    main()
```
* As you can see the type of audio file (mp3) in the code block
* file uri = 's3://transcribebucket78/speech.mp3'
* The address of the transcribe client = boto3.client('transcribe', region_name='eu-central-1') is specified.

![githubamazonoutput1real](https://github.com/iremibis/SpeechToText/assets/112003747/e7456139-252e-4b91-a94f-dd887163db0a)
* With this code block, the output looks like this. We can see the translated version of the audio file.

![githubamazon11](https://github.com/iremibis/SpeechToText/assets/112003747/d620d0bd-1e10-4467-93d9-bc4647949791)
* After the code runs successfully, in Amazon Transcribe > Transcription jobs, we can see Example-job1.

![githubamazon12](https://github.com/iremibis/SpeechToText/assets/112003747/2abaf348-132f-41be-8c2e-4cfb0247c7a8)
* When we open Example-job1, it shows us aws transcribe in paragraph form. When you click on each word, the second interval of the word in the audio file and the word confidence rate appear.

![githubamazon10](https://github.com/iremibis/SpeechToText/assets/112003747/41233e05-118f-4665-823b-16f843cc65bb)
* The code in the red box,

```
print(
                    f"Download the transcript from\n"
                    f"\t{job['TranscriptionJob']['Transcript']['TranscriptFileUri']}.")
```
* If you replace it with this code, you will see the json file download link with the transcribe of the audio file in the output section.

![githubamazonoutput2](https://github.com/iremibis/SpeechToText/assets/112003747/3f3330e3-0a3c-44fc-8c80-f8fec43df167)
* Your output will be like this and when you click on the link, you will download a json file containing the transcribe text of your audio file and information about start_time, end_time, confidence, content.
