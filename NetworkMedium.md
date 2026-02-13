# Ağ (Network) Dünyasının Derinlikleri: Protokoller, Mimariler ve Paket Analizi

**Siber güvenlik alanı başta olmak üzere birçok teknolojik sistemin belkemiğini oluşturan "Ağ" kavramı, sadece cihazların birbirine bağlanması değil, aynı zamanda verilerin güvenli ve düzenli bir şekilde akışının sağlanmasıdır. Bugün internete erişimi olan bir su şişesinden, kablo ile iletişim kuran devasa sanayi cihazlarına kadar her şey bu ekosistemin bir parçasıdır. Bu yazıda, temel ağ bileşenlerinden başlayarak TCP/IP detaylarına, DHCP'nin arka plandaki işleyişine ve Wireshark ile paket analizinin inceliklerine derinlemesine bir bakış atacağız.**

________________________________________

## Ağın Temel Kimliği: MAC ve NIC

Cihazların bir ağa bağlanmasında görevli olan donanım birimine **NIC (Network Interface Card)** adı verilir. Her ağ kartının, dolayısıyla her cihazın benzersiz bir fiziksel adresi vardır: **MAC (Media Access Control)** adresi.

Toplam 48 bitlik bu adresin yapısı şöyledir:
* **İlk 3 Byte:** Üretici firmayı temsil eder.
* **Son 3 Byte:** Ağ arayüzüne ait benzersiz seri numarasını içerir.

________________________________________

## İnternetin Dili: İletişim Protokolleri

Veri iletişimi sırasında kullanılan protokoller, trafiğin karakterini ve güvenliğini belirler. En sık karşılaşılan üç temel protokolü teknik detaylarıyla inceleyelim:

### 1. TCP (Transmission Control Protocol) – Güvenli İletişim
TCP, veri bütünlüğünü önemseyen güvenli bir iletişim protokolüdür. İki cihaz arasında veri aktarımı başlamadan önce **"Three-Way Handshake" (Üçlü El Sıkışma)** adı verilen bir süreç gerçekleşir.

Bağlantı şu bayraklar (flags) aracılığıyla kurulur:
* **SYN:** İletişimi başlatmak için gönderilen senkronizasyon bayrağıdır.
* **SYN-ACK:** Karşı tarafın isteği aldığını ve kabul ettiğini belirttiği bayraktır.
* **ACK:** Bağlantının kurulduğunu onaylayan son bayraktır.

Bu mekanizma sayesinde verinin karşı tarafa gidip gitmediği garanti altına alınır. Dosya transferleri, web siteleri (HTTP/HTTPS) ve e-postalar bu protokolü kullanır.

### 2. UDP (User Datagram Protocol) – Hız Odaklı İletişim
UDP, güvensiz ama hızlı bir protokoldür. TCP'nin aksine verinin ulaşıp ulaşmadığını kontrol etmez, sadece gönderir.

**Gerçek Hayat Örneği:** İnternet üzerinden sesli veya görüntülü konuşma yaparken görüntünün anlık donup sonra hızlıca akması UDP kullanımına en güzel örnektir. Kaybolan paketler (donma anı) tekrar istenmez, akış devam eder.

### 3. ICMP (Internet Control Message Protocol) – Ağın Sağlık Kontrolü
ICMP, ağ üzerinde veri taşıma işlemi yapmaz. Temel görevi, ağ üzerinde bir problem olup olmadığını kontrol etmek ve hata mesajı üretmektir. En bilinen kullanımı **Ping** komutudur; bir hedefe ulaşılamadığında veya ağda sıkışıklık olduğunda ICMP devreye girer.

________________________________________

## Adres Çözümleme ve Dağıtım Mekanizmaları

Bir ağdaki cihazların birbirini bulması ve konuşabilmesi için IP ve MAC adreslerinin eşleşmesi gerekir.

### ARP (Address Resolution Protocol)
Yerel ağ (LAN) üzerinde cihazların birbirleriyle iletişim kurabilmesi için MAC adreslerine ihtiyaçları vardır. ARP, IP adresi bilinen bir cihazın MAC adresini öğrenmek için kullanılır.

**Çalışma Mantığı:**
1. Cihaz, LAN içinde *"Bu IP kime ait?"* diye **Broadcast** (herkese) yayın yapar.
2. İlgili IP'ye sahip cihaz *"Benim"* diyerek kendi MAC adresini **Unicast** (tekil) olarak bildirir.
3. Bu işlemlerin sürekli tekrarlanmaması için cihazlar bu eşleşmeleri **ARP Önbelleği'nde (Cache)** tutar.

### DHCP (Dynamic Host Configuration Protocol) ve DORA Süreci
Ağa yeni bağlanan bir cihaza manuel IP vermek yönetilebilir bir süreç değildir. DHCP; cihazlara otomatik IP adresi, Subnet Mask ve Gateway bilgilerini dağıtır.

Bu işlem **DORA** adı verilen 4 adımlı bir süreçle gerçekleşir:
* **D**iscover: İstemci, ağda bir DHCP sunucusu arar (Broadcast yayını yapar).
* **O**ffer: Sunucu, istemciye kullanılabilir bir IP teklif eder.
* **R**equest: İstemci, teklif edilen IP'yi kullanmak istediğini belirtir.
* **A**cknowledgment: Sunucu onayı verir ve IP kiralanmış olur.

________________________________________

## İnternetin Telefon Rehberi: DNS ve E-Posta Trafiği

### DNS (Domain Name System)
İnsanların IP adreslerini (örn: 142.250.185.78) akılda tutması zor olduğu için alan adlarını (google.com) IP adreslerine çeviren sisteme DNS denir.

DNS kayıtları farklı amaçlara hizmet eder:
* **A Kaydı:** Bir alan adını IPv4 adresine eşler.
* **MX Kaydı (Mail Exchanger):** E-posta sunucularının yönlendirilmesi için kullanılır.

### E-Posta Trafiği ve SMTP Analizi
E-posta iletimi sırasında **SMTP (Simple Mail Transfer Protocol)** kullanılır. Bir e-postanın yolculuğu şu aşamalardan geçer:
1. Alıcıdan gönderici sunucuya iletim.
2. Gönderici sunucudan alıcı sunucuya iletim.
3. Alıcı sunucudan alıcının posta kutusuna iletim.

**Siber Güvenlik Perspektifi: Tracking Image**
Saldırganlar veya pazarlamacılar, e-postaların okunup okunmadığını anlamak için **Tracking Image** yöntemini kullanır. Bu, genellikle 1x1 piksel boyutunda, kullanıcının göremediği şeffaf bir görseldir. Kullanıcı e-postayı açtığında bu görsel sunucudan çekilir; böylece saldırgan, kullanıcının IP adresini, cihaz türünü ve e-postayı ne zaman açtığını tespit edebilir.

________________________________________

## Ağ Trafiğini İzlemek: Wireshark ve Paket Analizi

Ağ üzerindeki sorunları çözmek veya siber saldırıların kaynağını tespit etmek için **Wireshark** gibi paket analiz araçları kullanılır.

Wireshark'ın verimli çalışabilmesi için dikkat edilmesi gereken en önemli nokta, ağ kartının **Promiscuous (Karmaşık) Mod** veya **Monitor Mod** özelliklerinde olmasıdır; aksi takdirde ağdaki tüm paketler yakalanamaz.

**Analiz ve Raporlama İpuçları**
Bir ağ analizi yaparken filtreleme operatörleri (C, Java ve PHP dillerine benzer mantıkta) kullanılarak trafiği ayrıştırmak işleri kolaylaştırır. Analiz sonucunda bir rapor oluşturulurken, teknik terimlere boğulmadan; kaynak ve hedef IP adresleri arasındaki veri akışının, dosya isimlerinin ve potansiyel bilgi sızıntılarının net bir şekilde açıklanması gerekir.
