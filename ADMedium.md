# Active Directory (AD) Nedir? Windows Server Üzerinde Kurulum ve Güvenlik İpuçları

**Bu yazı, kurumsal ağların belkemiği olan Active Directory (AD) yapısını, Windows Server 2019 üzerinde kurulum adımlarını ve Windows sistem güvenliği üzerine staj sürecimde edindiğim önemli notları içermektedir. AD’nin temel terimlerini anlamak ve güvenli bir kurulum gerçekleştirmek isteyenler için bir başlangıç rehberi niteliğindedir.**

Kurumsal ağların belkemiği olan Active Directory (AD), merkezi yönetim ve güvenlik için vazgeçilmezdir. Bu yazımda AD'nin ne olduğunu, temel terimlerini (Domain, Forest, OU) ve Windows Server 2019 üzerinde adım adım kurulumunu ele alacağız. Ayrıca Windows sistem güvenliğine dair stajda öğrendiğim önemli detaylara, özellikle dosya uzantılarının gizlenmesinin yarattığı risklere ve PowerShell'in gücüne değineceğiz.

________________________________________

## Active Directory (AD) Nedir?

AD, Microsoft tarafından geliştirilmiş, genellikle Windows Server'lar üzerinde çalıştırılan, şirketlerdeki cihazların yönetiminin kontrolünü sağlayan bir dizin hizmetidir. Bir şirketteki cihazların işleyişini kontrol eden, ağ kaynaklarının kontrolünün sağlanmasını merkezi bir yerden yöneten yapıdır. Ağ kaynaklarına erişimi sağlar, kullanıcıları gruplandırarak daha rahat bir kontrol sistemi sunar.

________________________________________

## Temel Terimler

Active Directory yapısını anlamak için aşağıdaki temel kavramları bilmek gerekir:

* **Domain:** AD yapısındaki en temel yapıdır. Aynı veritabanını kullanan cihazlar topluluğudur. İsimlendirmesi benzersiz olmalıdır (Örn: `sirketAdi.local`).
* **Tree ve Forest:** Bir domaine ait alt domainler oluşturulması ile oluşan yapı **Tree** olarak adlandırılır. Farklı kök domainlerin birbirleri ile güvenilir ilişki (trust relationship) kurması sonucu oluşan yapı ise **Forest** olarak adlandırılır.
* **OU (Organizational Unit):** Domain içinde cihazların yönetimini sağlayan birimdir.
* **Global Catalog:** Forest yapısındaki ilk domain içinde bulunan, tüm nesnelerin bilgilerini içeren DC'dir.

________________________________________

## Windows Server Üzerinde AD Kurulum Adımları

AD kullanımı için Windows Server üzerinde yapılması gereken adımlar sırasıyla şöyledir:

**1. IP Yapılandırması**
Kuruluma başlamadan önce sunucuya statik bir IP adresi tanımlanmalıdır.

**2. Rol Ekleme**
Server Manager üzerinden `Add Roles and Features` diyerek **Active Directory Domain Services** rolünü seçip yüklüyoruz.

**3. Promote Etme**
Kurulum bittikten sonra bildirimler kısmından *"Promote this server to a domain controller"* seçeneğine tıklıyoruz.

**4. Konfigürasyon**
*"Add a new forest"* diyerek domain adımızı veriyoruz. Domain Controller Options kısmında functional level olarak **Server 2016** seçmemiz daha garanti bir yaklaşımdır.

**5. DSRM Şifresi**
Bu şifre ileride AD'yi kaldırmak veya kurtarmak istediğimizde lazım olacağı için mutlaka not edilmelidir.

**6. İstemci (Client) Ekleme**
İstemci bilgisayarın DNS adresini, kurduğumuz Server'ın IP adresi yapıyoruz. Ardından *Bilgisayar Adı -> Değiştir -> Etki Alanı* kısmına domain adımızı yazarak dahil ediyoruz.

________________________________________

## Windows Sistem Güvenliği ve Geçmişten Dersler

Staj sürecinde Windows güvenliği ile ilgili dikkat çekici bazı tarihsel olayları ve teknik detayları da inceleme fırsatı buldum.

**Dosya Uzantılarının Gizlenmesi Riski**
Dosya uzantılarının gizlenmesi büyük bir güvenlik riski yaratmaktadır. 2000'lerin başında çıkan **"ILOVEYOU"** virüsü, kendisini `iloveyou.txt.vbs` şeklinde gizleyerek (kullanıcı sadece `.txt` kısmını gördüğü için) milyonlarca cihaza bulaşmıştı. Bu yüzden dosya uzantılarının sistemlerde açık olarak kullanılması tavsiye edilmektedir.

**PowerShell vs CMD**
Komut satırı (CMD) klasik siyah ekranlı eski terminalken, PowerShell daha gelişmiş ve güçlü otomasyonlar yapabilen bir yapıdır. PowerShell'in gücünü anlamak için **Stuxnet** saldırısı önemli bir örnektir. 2010 yılında İran'ın nükleer santrallerine yapılan bu saldırı, PowerShell'in neler yapabileceğini gösteren en büyük kanıtlardan biridir.

________________________________________

## Pekiştirme Soruları

Konuyu daha iyi kavramak adına stajda üzerinden geçtiğimiz bazı kritik sorular ve cevapları:

* **Domain nedir ve ne işe yarar?**
Ağdaki cihazların merkezi bir şekilde yönetilebilmesini sağlayan sistemdir. Ağ kaynaklarına erişimi sağlar.
* **Domain kurmak için hangi rol kurulur?**
AD DS (Active Directory Domain Services) rolü kullanılarak kurulur.
* **Domain Controller (DC) nedir?**
Domainin kullanımı sırasında kimlik doğrulaması işlemlerini yürütür.
* **Domain kurulmadan önce sunucuya hangi IP tipi verilmelidir?**
Statik bir IP adresi verilmelidir.
* **DNS rolü neden önemlidir?**
Bu sayede ağdaki cihazların birbiriyle iletişimi doğru ve eksiksiz bir şekilde sağlanır.
* **Bilgisayar domaine alındıktan sonra hangi değişiklikler olur?**
Artık o cihaz merkezi yönetime dahil olmuş olur ve domain hesabı ile sisteme giriş yapılabilir.
* **Gruptaki kullanıcıları ve izinleri merkezi olarak yönetmek için ne kullanılır?**
AD Group Policy.

________________________________________

**Sonuç**

Active Directory, modern şirket yapılarında kimlik yönetimi ve güvenlik politikalarının uygulanması için merkezi bir rol oynamaktadır. Doğru yapılandırılmış bir Domain ortamı ve bilinçli güvenlik önlemleri (dosya uzantılarının açılması gibi basit adımlar dahil), siber güvenliğin temel taşlarını oluşturur.

Sorularınız veya katkılarınız varsa, yorumlarda paylaşabilirsiniz.

________________________________________

**Kaynaklar**

* Staj Defteri Notları ve Active Directory Dokümantasyonu
