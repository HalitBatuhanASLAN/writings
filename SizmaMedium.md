# Staj Günlüğümden: Web Zafiyetleri ve Mobil Sızma Testi Laboratuvarı Kurulumu

**Siber güvenlik staj sürecimde hem teorik hem de pratik olarak web ve mobil uygulama güvenliği üzerine yoğunlaştığım günlerden aldığım notları bu yazıda derledim. XSS, SQL Injection gibi temel web zafiyetlerinden; Android sızma testleri için gerekli olan Genymotion, Burp Suite ve Frida kurulumuna kadar edindiğim tecrübeleri aşağıda bulabilirsiniz.**

________________________________________

## Web Zafiyetlerine İlk Adım

Web zafiyetlerinin nasıl yapıldığı ve neler olduğunu hem teorik hem de pratik bir şekilde şirket çalışanlarından birisinin kendi oluşturduğu laboratuvar ortamı üzerinden ilerleyerek öğrendik.

Burada **XAMPP** uygulamasını indirdik ve ardından GitHub üzerindeki zafiyetli laboratuvar dosyasını indirip XAMPP uygulaması üzerinde çalıştırdık (Burada XAMPP uygulaması bizlerin bir web sunucusu ortamını hızlı ve güvenli bir şekilde kurmamıza olanak tanır).

Ardından web üzerinden laboratuvara erişim sağladık. Öncelikli olarak XSS saldırılarından başladık.

### XSS (Cross-Site Scripting) Saldırıları
Kullanıcı girdilerinin güvenli bir şekilde işlenmemesi sonucu zararlı kullanıcıların genellikle JavaScript kodlarını enjekte etmesi ve bunun diğer kullanıcıların tarayıcılarında çalışması sonucu oluşur. 3 temel türü vardır:

* **Stored XSS:** Veri girişi alanına zararlı kod yerleştirilmesi ile oluşur.
* **Reflected XSS:** Zararlı kod içeren bir URL veya forma tıklanmaya itecek şekilde tasarlanır.
* **DOM-Based XSS:** Kullanıcı girdilerinin müdahalesi ile DOM (Document Object Model) üzerinde değişiklik yapılarak olur.

Öncelikli olarak seviye 1 üzerinde zafiyet bulmaya çalıştık, burada PHP kodunu da inceleyerek gittik. Girdi alma kısmında bir koşul olmadığını gördük ve biz kendimiz isim girme kısmında `<i>isim</i>` yazarak italik bir isim yazısı elde ettik. Yani burada bir **HTML Injection** olduğunu anladık. Böyle bir durumda XSS zafiyeti olduğunu görmüş olduk.

________________________________________

## Diğer Kritik Web Zafiyetleri

**Hash Collision Zafiyeti**
Hash, parolaların belli bir standart uzunlukta değiştirilmiş bir şekilde tutulmasını sağlayan yapı olarak adlandırılabilir en basit anlatımıyla. Bizler farklı parola değerleri için aynı hash algoritmasını kullandığımızda aynı sonucu elde edebiliriz (Collision).

PHP kodu yazarken parola doğrulamasını eğer `==` eşitliği ile kontrol edersek hash sonuçlarının karşılaştırılması yapılır; fakat `===` kontrolü ile sağlarsak içerideki değerlerin ve türlerin karşılaştırılması yapılır.

**Command Execution Zafiyeti**
Sistemdeki zafiyet neticesinde saldırganın sistem üzerinde uzaktan kod çalıştırabilmesine olanak sağlaması durumudur. Biz lab üzerinde `whoami` komutunu çalıştırdığımızda kullanıcı adını alabiliyoruz veya `type` ile bir dosya içeriğini okuyabildik.

**Path Traversal Saldırısı**
Kullanıcı girdileri aracılığı ile dosya yollarına erişimin kısıtlanmaması sonucu kullanıcının girdiler eşliğinde dosyalar arasında gezinme, okuma işlemlerini yapabilmesine sebep olan bir zafiyet türüdür. Örneğin `../` ile bir üst dizine çıkılabilir.

**IDOR (Insecure Direct Object Reference)**
Kullanıcıların bir kimlik doğrulama süreci olmadan başkalarının verisine erişebilmesine ve bu sayede başkasının hesabında işlem yapabilmesi durumudur. Lab üzerinde ID numaralarını 0'dan itibaren tek tek girerek 8 ID'si ile giriş direkt olarak gerçekleşti.

**SQL Injection**
Bir kullanıcı girdisinin kontrol edilmeden SQL sorgusuna girmesi sonucunda meydana gelir. Burada saldırgan girdi sırasında hassas verilere erişebilecek, veritabanını manipüle edebilecek şekilde girdiler verebilir. Lab'daki sistem için `sqlmap` aracını kullanarak `sqlmap -u "URL" --dbs --dump-all` kullanımı ile tüm veritabanını sisteme çekebileceğimizi de görmüş olduk.

________________________________________

## Mobil Sızma Testleri ve Laboratuvar Kurulumu

Mobil sızma testlerinin gerçekleştirilmesinin nasıl yapıldığı ile başladık.

**1. Genymotion Kurulumu**
Öncelikli olarak Genymotion adlı uygulamayı sistemlerimize indirip uygulama içinde sanal bir cihaz (emülatör) ayağa kaldırdık. Genymotion kullanma sebebimiz test süreçlerinde emülatör kullanımının daha işlevsel olmasıdır.

**2. Burp Suite Entegrasyonu**
Mobil uygulama sızma testleri belli bir adımdan sonra web sızma testlerine benzerlik gösterdiği, API'ler ile olan istekleri görüntüleyebilmek ve Proxy özelliğini kullanarak HTTPS trafiği üzerinde manipüle edebilmek gibi ihtiyaçlarımız olduğu için Burp Suite kullandık.

**3. Frida Kurulumu**
Dinamik analiz için Frida'yı terminal üzerinden kurmamız gerekti. Frida'yı kurduktan sonra GitHub üzerinden bizim indirdiğimiz Frida versiyonunu bulup, emülatör cihazımızın işletim sistemine uygun olan `frida-server` dosyasını indirdik.

### Sertifika ve Proxy Ayarları
Burp Suite üzerinde bir sertifika oluşturup `.cer` uzantısı ile export ettik. Terminal üzerinden `adb` (Android Debug Bridge) komutu ile sertifikayı cihaza ilettik (`adb push`). Android cihaz içerisindeki **Security -> Encryption and Credentials** kısmından sertifikanın kurulumunu yaptık. Mobil cihazda Wi-Fi ayarlarından manuel proxy seçip kendi bilgisayarımızın IP adresini ve Burp Suite portunu girdik.

### SSL Pinning ve Root Tespiti Baypas (Bypass)
İnternet üzerinden zafiyetli bir cihaz bulduk ve APK dosyasını indirip kurduk. Fakat uygulamayı açmaya çalıştığımızda bizlere *"Emulator Detected"*, *"Phone is Rooted"* ve *"Frida is Running"* hatalarını verip uygulamayı direkt kapattı. Bunlar yazılımcıların uygulama güvenliği için ekledikleri adımlardır.

Bu hataları aşmak için **Frida Codeshare** sayfası üzerinden hazır baypas scriptlerini kullandık. `frida -U -f paket_ismi -l script.js` komutlarını kullanarak her bir hata için farklı baypas örneklerini çalıştırdık ve uygulamayı açabildik. Buraya kadarki kısım **Dinamik Analiz** olarak adlandırılan kısımdı.

### Statik Kod Analizi
Öğleden sonraki mesaide ise statik kod analizi kısmını pratik ettik. Bunu yapabilmek için **MobSF** (Mobile Security Framework) adlı bir uygulamanın kurulumunu yaptık. Bu cihaz içerisine APK dosyasını attığımızda içerisinde hassas bir bilginin olup olmadığını, koddaki olası problemleri hızlıca analiz edip bizlere veriyor. Kaynak kodları incelediğimizde kodların karışık olduğunu fark ettik; bunun sebebinin yazılımcıların **obfuscator** aracı ile kaynak kodu karmaşıklaştırdıkları olduğunu öğrendim.
