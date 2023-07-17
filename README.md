# speechToTextAmazon
Amazon Transcribe ve Python ile speech to text kullanımı raporu

![githubamazon1](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/502e8a68-f341-492b-88f6-e0a7d5d159d0)
* Öncelikle bir aws hesabı açın.
* Daha sonra S3 Bucket oluşturun. Bu Bucket'in içine çevireceğimiz ses dosyasını koyun.
* Yukarıda gördüğünüz gibi istediğiniz ses dosyasını S3 Buckete yükleyebilirsiniz.
* S3 bucket'i global bölgede kullanın.

![githubamazon2](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/e0c8bb64-1712-498a-86a8-b311427d4759)
* Burada Amazon Transcribe > Create Transcription Job kısmına gelerek bir Transcription Job oluşturun.
* Oluştururken çıkan seçeneklerde hepsi default ayarlarda olacak.
* Yukarıdaki ekran resminde gördüğünüz gibi Transcription jobs burada görünüyor. İlk oluşturduğunuzda burası boş görünecek.
* Python kodunu her çalıştırdığımızda yeni bir example job üretecek ve burada görünecek.
* Transcription Job'u hangi bölgede kullandığınıza dikkat edin. (Örneğin eu-central-1 i seçerseniz kod kısmında bunu belirtmeniz gerekir.)

![githubamazon3](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/79b79366-b489-4da0-830d-3ae8d74ee280)
* Daha sonra bir aws IAM accountu oluşturun.
* IAM Dashboard'dan Users kısmına gelin ve Add Users butonuna tıklayın.
* Kırmızı kutucuklarla gösterilen seçenekleri seçin. Bu bizim erişimimizi sağlayacak.
* En alt sağda Next butonuna tıklayın.
* Global bölgede kullanın.

![githubamazon4](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/5741c142-cb98-4d1a-8fc1-f6b1c6ef3826)
* Bu sayfada kırmızı kutucukta gösterilen Create User Butonuna tıklayın.

![githubamazon5](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/0748e43d-d720-44c0-91a5-a8d8cf3cbfdf)
* Kırmızı kutucukla gösterilen Security Credentials sekmesine gelin.

![githubamazon6](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/9b6870f3-d92f-492e-8827-b9bff9ec9e82)
* Kırmızı kutucukla gösterilen Create Access Key butonuna tıklayın.

![githubamazon7](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/a2434294-4151-416a-a60a-e4a4b659f99e)
* AWS hesabımıza komut satırı arayüzünün erişimine izin vermek ve sağlamak için CLI seçeneğini seçin.
* Alternatif önerileri kabul ettiğimizi beyan etmek için tik işaretini işaretleyin.
* Next butonuna basın

![githubamazon8](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/f01a786b-42f3-4347-b527-939cdad404b0)
* Bu sayfada Description Tag ayarlamamızı istiyor. Bu erişim anahtarının açıklaması, kullanıcıya bir etiket olarak eklenir ve erişim anahtarının yanında gösterilir.
* Bu opsiyonel bir seçenek isterseniz ayarlayabilirsiniz. Açıklamayı düzgün yapmaya dikkat edin. İyi bir açıklama, bu erişim anahtarını daha sonra güvenle döndürmenize yardımcı olacaktır.
* Create Access Key butonuna tıklayın.

![githubamazon9](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/49c2255f-0157-4078-b784-f4d75c581720)
* Bu sayfada access key ve Secret access key oluşturuldu.
* Access key ve Secret access key'i kaybederseniz veya unutursanız onu geri alamazsınız. Bunun yerine, yeni bir erişim anahtarı oluşturun ve eski anahtarı devre dışı bırakın.
* Kırmızı kutucukta gösterilen Download .csv file butonuna tıklayarak keyleri bilgisayarınıza indirebilirsiniz böylece kaybetmezsiniz. 
* Keyleri python terminaline yazıp kod ile birbirine bağlayın.

# Keyleri import etmek.
Terminalinizi açın ve aşağıdaki komutu çalıştırın:
* export AWS_ACCESS_KEY_ID=ACCESS KEYİNİZ
* export AWS_SECRET_ACCESS_KEY=SECRET ACCESS KEYİNİZ

# PYTHON KOD BLOĞU :

Yukarıdaki örnekte, Python kodunuz ````python` ve ```` ` arasına yerleştirilmiştir. Bu, README dosyasında kod bloğunun Python dilinde olduğunu belirtir.

Kod bloğunuz aşağıdaki gibi görünecektir:

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
* Gördüğünüz gibi kod bloğunda ses dosyasının türü (mp3)
* file urisi = 's3://transcribebucket78/speech.mp3'
* transcribe clien'in adresi = boto3.client('transcribe', region_name='eu-central-1') belirtilmiştir.

![githubamazonoutput1real](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/07063f8d-316c-4c03-977a-c7aa73dce442)
* Bu kod bloğuyla output bu şekilde görünmektedir.

![githubamazon11](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/d586e43a-6b72-48d0-8642-949cb81e6fe5)
* Kod başarıyla çalıştıktan sonra Amazon Transcribe > Transcription jobs kısmında oluşturduğumuz Example-job1 gözüküyor.

![githubamazon12](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/c6541fb0-7d9b-46dd-8185-78c340cfb681)
* Example-job1'i açtığımızda bize bu şekilde aws transcribe'i gösteriyor. Her bir kelimenin üstüne tıkladığınızda ses dosyasının kaçıncı saniyeleri arasında söylendiği ve word confidence oranı gözükür.

![githubamazon10](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/63322e35-30ec-4e65-81ee-73302fcb94f5)
* Kırmızı kutucuk içerisindeki kodu,

```
print(
                    f"Download the transcript from\n"
                    f"\t{job['TranscriptionJob']['Transcript']['TranscriptFileUri']}.")
```
* Kodu ile değiştirirseniz, çıktı kısmında ses dosyasının transcribe'ini içeren json dosyası indirme linki görürsünüz.

![githubamazonoutput2](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/8dac7109-56b8-4c31-918a-fe0cf97bb150)
* Çıktınız bu şekilde olur ve linke tıkladığınız zaman ses dosyaınızla ilgili transcribe metnini ve start_time, end_time, confidence, content hakkında bilgiler içeren bir json dosyası indirirsiniz.
