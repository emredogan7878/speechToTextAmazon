# speechToTextAmazon
Amazon Transcribe ve Python ile speech to text kullanımı raporu

![githubamazon1](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/502e8a68-f341-492b-88f6-e0a7d5d159d0)
* Öncelikle bir aws hesabı açın.
* Daha sonra S3 Bucket oluşturun. Bu Bucket'in içine çevireceğimiz ses dosyasını koyun.
* Yukarıda gördüğünüz gibi istediğiniz ses dosyasını S3 Buckete yükleyebilirsiniz.

![githubamazon2](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/e0c8bb64-1712-498a-86a8-b311427d4759)
* Burada Amazon Transcribe > Create Transcription Job kısmına gelerek bir Transcription Job oluşturun.
* Oluştururken çıkan seçeneklerde hepsi default ayarlarda olacak.
* Yukarıdaki ekran resminde gördüğünüz gibi Transcription jobs burada görünüyor. İlk oluşturduğunuzda burası boş görünecek.
* Python kodunu her çalıştırdığımızda yeni bir example job üretecek ve burada görünecek.

![githubamazon3](https://github.com/emredogan7878/speechToTextAmazon/assets/112003747/79b79366-b489-4da0-830d-3ae8d74ee280)
* Daha sonra bir aws IAM accountu oluşturun.
* IAM Dashboard'dan Users kısmına gelin ve Add Users butonuna tıklayın.
* Kırmızı kutucuklarla gösterilen seçenekleri seçin. Bu bizim erişimimizi sağlayacak.
* En alt sağda Next butonuna tıklayın.

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
