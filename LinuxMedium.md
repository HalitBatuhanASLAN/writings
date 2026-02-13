# Sıfırdan Linux: Çekirdek Yapısı, Dağıtımlar ve Temel Terminal Komutları

**Linux, bilinenin aksine bir işletim sistemi değil, bir çekirdektir (kernel). Çekirdek, en sade haliyle donanımlar ile yazılımlar arasındaki bağlantıyı sağlayan yapıdır. Bu yazımda Linux dünyasına giriş yaparak dağıtımları (Kali, Ubuntu, Arch) ve terminalde işinizi kolaylaştıracak temel komutları anlatacağım.**

________________________________________

## Linux ve GNU Felsefesi

Linux çekirdeği ile GNU projelerinin birleşmesi ile aslında farklı işletim sistemleri ortaya çıkmaktadır ve bunlar **GNU/Linux dağıtımı** olarak adlandırılmaktadır.

* **Kali Linux:** Özellikle siber güvenlik alanında, içerisinde sızma testi araçları (tools) barındırmasıyla bilinir.
* **Ubuntu:** Yazılım geliştiriciler ve son kullanıcılar için kullanıcı dostu bir sistemdir.
* **Arch Linux:** Kullanıcılara kurulumdan itibaren sistemi tamamen kendilerine göre özelleştirme imkanı sunar, ancak kullanımı biraz daha ileri seviye bilgi gerektirir.

________________________________________

## Sık Kullanılan Temel Linux Komutları

Terminal üzerinde kullanacağımız komutlara geçmeden önce bazı kısaltmaları bilmek işimizi hızlandırır:

* **.** (Nokta): Bulunduğumuz dizini temsil eder.
* **..** (İki nokta): Bir üst dizine geçmemizi sağlar.
* **~** (Tilda): Kullanıcının ev (home) klasörüne gitmeyi sağlar.
* **|** (Pipe): Birden fazla komutu ardışık çalıştırmayı sağlar; ilkinin çıktısı ikincinin girdisi olur.

### Komut Listesi

**Gezinme ve Listeleme**
* **ls:** Dosyaları listeler. `ls -la` gizli dosyaları ve detayları gösterir.
* **pwd:** O an hangi dizinde olduğumuzu gösterir (Print Working Directory).
* **cd:** Dizin değiştirmek için kullanılır (Örn: `cd Desktop`).

**Dosya İşlemleri**
* **cp:** Kopyalama (Örn: `cp dosya.txt /hedef/dizin`).
* **mv:** Taşıma veya isim değiştirme.
* **rm:** Silme. (`rm -rf` klasörleri zorla siler, dikkatli kullanılmalı!).
* **cat:** Dosya içeriğini ekrana basar.
* **tail -n 5:** Dosyanın son 5 satırını gösterir.
* **touch:** Boş bir dosya oluşturur.

**Arama ve Filtreleme**
* **grep:** Dosya içinde kelime arar.
* **find:** Dosya arama komutudur.
* **which:** Bir programın hangi dizinde kurulu olduğunu gösterir.

**İzinler ve Sistem**
* **chmod:** Dosya izinlerini değiştirir (Örn: `chmod +x script.sh` çalıştırılabilir yapar).
* **sudo:** Yetki yükseltme (Yönetici olarak çalıştırma) komutudur.
* **man:** Komutların kullanım kılavuzunu açar (Örn: `man ls`).

________________________________________

## Metin Editörleri

Linux'ta terminal üzerinden dosya düzenlemek için **Nano** ve **Vim** sıkça kullanılır.

* **Nano:** Daha basit ve başlangıç dostudur. `ctrl+x` ile çıkış yapılır.
* **Vim:** Daha gelişmiş, modlarla çalışan bir editördür. `i` ile yazma moduna geçilir, `esc` basıp `:wq` yazılarak kaydedip çıkılır.
