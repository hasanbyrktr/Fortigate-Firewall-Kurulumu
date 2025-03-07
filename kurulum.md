FORTIGATE® Firewall Kurulumu

![FortiGate Firewall](/1.webp)

Firewall (Güvenlik Duvarı) Nedir?
Firewall’lar, iç ve dış ağ trafiğini denetlemeyi ve kontrol altına almayı sağlayan cihazlardır. Bu cihazlar sayesinde yerel makinemizden izinsiz olarak dış ağlara veri alışverişi yapılması engellenir. Yani bilgisayarımızdan dış internete açılan ağda kısıtlamalar, kontroller ve kurallar belirleyerek, yalnızca bu kurallara uygun bağlantılara izin verirler.

Bugünkü konumuz ise siber dünyada ağ güvenliği çözümleri sağlayan, özellikle yüksek performanslı güvenlik duvarları ve entegre tehdit yönetimi sistemleri ile tanınan bir lider konumunda olan Fortinet’in firewall markası olan FortiGate kurulumundan bahsedeceğim.

FortiGate Nedir? Nasıl Kurulur?
Fortigate, Fortinet tarafından üretilen ağ güvenliğini sağlamak amacıyla kullanılan next-generation bir firewall (NGFW) cihazıdır. FortiGate, ağ trafiğini güvenli bir şekilde yönlendirmek ve kötü amaçlı yazılımlardan, siber saldırılardan ve diğer tehditlerden korunmayı sağlamak için çeşitli güvenlik özelliklerine sahip bir güvenlik duvarıdır.

FortiGate’in sunduğu başlıca özelliklerden örnek vermek istiyorum:

1- Gelişmiş Tehdit Koruması: Fortigate, anti-virüs, IPS (Intrusion Prevention System), web filtreleme, e-posta güvenliği, uygulama kontrolü, VPN ve bir çok özelliği sunarak ağınızı korur.

2- Uygulama Kontrolü: Uygulama seviyesinde kontrol sağlar, ağdaki belirli uygulamalar izin verir veya bunları engeller.

3- VPN Desteği: Hem site-to-site hem de remote access VPN bağlantıları sunarak uzak erişim ve şube bağlantılarında güvenlik sağlar.

4- Deep Packet Inspection(DPI): Veri paketlerinin içeriğini analiz ederek daha derin bir güvenlik seviyesi sağlar.

5- Yük Dengeleme Ve Trafik Yönetimi: Ağ trafiğini optimize eder ve yüksek kullanılabilirlik sağlar.

6-Gelişmiş Loglama Ve İzleme: FortiGate, ağ trafiği hakkında ayrıntılı günlükler ve raporlar sunar, bu da tehditlerin tespit edilmesi ve müdahale edilmesini kolaylaştırır.

Şimdi bugün size FortiOS işletim sistemini kullanan cihazımınız arayüzüne ufak bir bakış atalım

Erdinç Tandoğan hocam aracılığıyla eriştiğim süre sınırlı lisans ile kurulum yapmaktayım.

Cihazı ilk açtığımızda bizi FortiGate’in Status penceresi karşılar.
İlk önce cihazımıızı internete çıkarırken static route oluşturmalıyız.

![FortiGate Firewall](/2.webp)
![FortiGate Firewall](/3.webp)

Static route kısmına gelip new dedikten sonra 0.0.0.0/0.0.0.0 yazıyoruz
Buradaki Gateway address internete çıkış cihazımız olan modemimizin IP adresidir. Gateway IP’nizi öğrenmek için işletim sisteminize göre değişerek Windows için cmd ekranında “ipconfig” komutuyla öğrenebilirsiniz.

FortiGate ve diğer cihazlarda da olmak üzere 0.0.0.0/0.0.0.0 internete erişim demektir. 0.0.0.0/0.0.0.0, ağ yönetiminde “herhangi bir IP” anlamına gelir ve tüm internet trafiğini kapsar. FortiGate gibi güvenlik cihazlarında, bu adres genellikle internete erişim sağlamak için kullanılır. Bu ayar, dışa açık trafik için tüm IP adreslerine izin vererek cihazın internete erişmesini sağlar. Özellikle yönlendirme ve güvenlik duvarı kuralları ile yapılandırıldığında, 0.0.0.0/0.0.0.0, internet bağlantısı için temel bir yapı taşını oluşturur.

![FortiGate Firewall](/4.webp)

Interfaces menüsüne gelip new diyerek yeni bir Interface oluşturuyoruz
Interfaces menüsüne gelip new diyerek yeni bir Interface oluşturuyoruz. Ben bu Interface’i IT birimim kullansın istiyorum ve IT birimimin girişni bu port üzerinden yapacağım. Role kısmını LAN olarak seçiyorum. IP/Netmask olarak 192.168.10.2/255.255.255.0 ayarlıyorum IT Birimimden internete bağlanan kullanıcılara 192.168.10.x şeklinde DHCP protokülünün 2 den 254e kadar IP dağıtması için address range kısmını görseldeki gibi dolduruyorum. Bağlanan ilk cihazımız 192.168.10.2 den başlayarak sırayla IP alır.

Şimdi bu IT birimimizi internete çıkarmak için gerekli policyleri (kuralları) yazmamız gerekiyor.

![FortiGate Firewall](/5.webp)

Biraz karışık gelmiş olabilir tane tane açıklayacağım:

Name: Kuralı tanımlar ve yönetim kolaylığı sağlar. “IT_UNIT_TO_INTERNET” adı, bu kuralın IT biriminden internete trafik akışını kontrol ettiğini gösterir.

Incoming Interface: Trafiğin geldiği ağ arayüzünü belirtir. “IT_UNIT (port4)” IT biriminin bağlı olduğu fiziksel port.

Outgoing Interface: Trafiğin çıkacağı ağ arayüzünü belirtir. “WAN-INTERNET (port1)” internet bağlantısının olduğu port.

Source: Trafiğin nereden başladığını tanımlar. “all” seçildiğinde IT_UNIT arayüzündeki tüm cihazların trafiği kapsanır.

Destination: Trafiğin nereye gittiğini tanımlar. “all” seçildiğinde internetteki tüm hedeflere erişim sağlanır.

Schedule: Politikanın ne zaman aktif olacağını belirler. “always” seçildiğinde politika sürekli aktiftir.

Service: Hangi protokol ve port numaralarına izin verileceğini belirler. Boş olduğunda tüm servislere izin verilecektir.

Action: ACCEPT -Eşleşen trafiğin geçmesine izin verir./DENY- Eşleşen trafiği engeller.

NAT (Network Address Translation): İç ağ IP adreslerini dış ağda kullanılabilir adreslere çevirir, iç ağın gizliliğini sağlar. NAT’ı aktif etmezsek internete çıkışımız engellenir. O yüzden açıyoruz

Güvenlik Profilleri:

AntiVirus: Zararlı yazılım tespiti ve engelleme işlevi sağlar.
Web Filtresi: Web sitelerini kategorilere göre filtreleme ve zararlı siteleri engelleme işlevi sağlar.
DNS Filtresi: DNS sorgularını filtreleyerek zararlı alan adlarına erişimi engeller.
Uygulama Kontrolü: Belirli uygulamaları tanıyıp kontrolünü sağlar (örn. Facebook, YouTube).
IPS (Saldırı Önleme Sistemi): Bilinen ağ saldırılarını tanıyıp engeller.
SSL İnceleme: Şifreli SSL/TLS trafiğini inceler. “certificate-inspection” modu sertifikaları doğrular ancak içeriği incelemez.

![FortiGate Firewall](/6.webp)

Ben görseldeki gibi default(varsayılan) profilleri aktif ettim ve kuralı kaydettim. Bundan sonraki işlemde bir sanal makine kullanarak ya da kendi bilgisayarınızı kullanıp belirlediğimiz kurallarla firewall üzerinden internete çıkış yapabilirsiniz.

Bir sonraki yazımda bu profilleri de özelleştirip filtreleme ve konfigürasyon ayarlarından bahsedeceğim