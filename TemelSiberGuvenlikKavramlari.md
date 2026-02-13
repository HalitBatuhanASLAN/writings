# Siber Güvenliğin Alfabesi: Temel Terimler, Kriptoloji ve CIA Üçgeni

**Siber güvenlik dünyasına adım attığınızda karşınıza çıkan ilk kavramlar genellikle CIA üçgeni, şifreleme türleri ve saldırı vektörleridir. Staj sürecimde aldığım teorik eğitimler ve pratik gözlemlerim (özellikle "şifre" ve "parola" arasındaki fark gibi) ışığında, bu temel yapı taşlarını sizler için derledim.**

________________________________________

## Bilgi Güvenliğinin Temeli: CIA Üçgeni

Bir bilgi sisteminin güvenlik hedeflerini belirlemede **CIA Triad** kullanılır. Bu üç yapıdan herhangi birindeki hasar, zafiyet oluşmasına sebep olur:

* **Confidentiality (Gizlilik):** Bilgilere yalnızca yetkili kişilerin erişebilmesini temsil eder. Veri yaşam döngüsü boyunca korunmalıdır. İki adımlı doğrulama (2FA) ve güçlü şifreleme bu ilkenin en önemli koruyucularıdır.
* **Integrity (Bütünlük):** Bilginin yetkisiz kişilerce değiştirilmediğini, silinmediğini garanti eder. Verinin doğruluğu ve tamlığı esastır.
* **Availability (Erişilebilirlik):** Bilginin ihtiyaç duyulduğu anda yetkili kişilerce erişilebilir olmasıdır. DDoS saldırıları genellikle bu ilkeyi hedef alır.

________________________________________

## Kriptoloji Dünyası: Encoding, Hashing ve Encryption

Bu terimler sıklıkla birbirine karıştırılsa da stajda aralarındaki farkları net bir şekilde öğrendim:

### Encoding (Kodlama)
Verinin şifrelenmesi değil, farklı bir formatta tutulmasıdır. Örneğin **Base64** veya **URL Encoding**. Bir anahtara ihtiyaç duyulmaz, algoritmayı bilen herkes geri döndürebilir (Decoding). Jul Sezar zamanında kullanılan her harfi 3 karakter öteleme yöntemi (Sezar Şifrelemesi) aslında tarihsel bir encoding/basit şifreleme örneğidir.

### Encryption (Şifreleme)
Verinin bir anahtar kullanılarak gizlenmesidir.
* **Simetrik Şifreleme:** Şifrelerken ve çözerken aynı anahtar kullanılır. Hızlıdır ancak anahtar paylaşımı risklidir.
* **Asimetrik Şifreleme:** Açık (Public) ve Özel (Private) olmak üzere iki anahtar vardır. Herkesin bildiği Public Key ile şifrelenen veri, sadece sahibinin bildiği Private Key ile çözülebilir (Örn: RSA).

### Hashing (Özetleme)
Veriyi sabit uzunlukta ve benzersiz bir değere dönüştürme işlemidir. Tek yönlüdür, geri döndürülemez. Parolaların veritabanında saklanması için kullanılır (MD5, SHA-256).
* **Collision (Çakışma):** Farklı iki verinin aynı hash değerini üretmesi durumudur.
* **Salting (Tuzlama):** Hash saldırılarına (Rainbow Table) karşı, parolaya rastgele veriler ekleyerek hash'in karmaşıklaştırılması işlemidir.

________________________________________

## Kavram Karmaşası: Şifre (Cipher) vs Parola (Password)

Stajda öğrendiğim en ilginç ayrımlardan biri buydu. Günlük hayatta *"Şifremi unuttum"* deriz ama aslında teknik olarak yanlış kullanıyoruz:

* **Şifre (Cipher):** Bir metni gizlemek, karmaşıklaştırmak için kullanılan **algoritma veya anahtardır**.
* **Parola (Password):** Kimlik doğrulaması (Authentication) için kullanılan gizli kelimedir. Askeriyeden verilen örnekle; nöbetçinin *"Parola?"* sorusuna karşılık gelen *"Şafak"* cevabı bir paroladır.

________________________________________

## Siber Tehdit İstihbaratı (CTI) ve İstihbarat Türleri

Savunmaların daha etkili yapılabilmesi için tehdit aktörleri hakkında bilgi toplama sürecidir.

* **OSINT (Open Source Intelligence):** Açık kaynaklardan (sosyal medya, forumlar, Google Dorking) bilgi toplama sürecidir.
* **HUMINT (Human Intelligence):** Teknoloji yerine insan iletişimi ve sosyal mühendislik kullanılarak doğrudan bilgi toplanmasıdır.
* **OPSEC (Operational Security):** Operasyonel güvenliğin sağlanması, kritik bilgilerin korunması ve risklerin analiz edilmesidir.

________________________________________

## Sosyal Mühendislik Saldırıları

İnsan psikolojisini kullanarak bilgi çalmayı hedefleyen yöntemlerdir:

* **Phishing (Oltalama):** Toplu e-posta veya SMS ile yapılan saldırılar.
* **Spear Phishing:** Belirli bir kişiyi veya kurumu hedef alan, özelleştirilmiş oltalama saldırısı.
* **Vishing:** Sesli arama yoluyla (telefonda bankacı gibi davranarak) yapılan dolandırıcılık.
