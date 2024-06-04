# 1. ASP’yi Anlama

## 1.1 İnternet Protokollerine Bakış ve HTTP

Bu bölüm, sunucu taraflı programlama dillerinden ASP’yi (Active Server Pages) ayrıntılı olarak anlatmak üzere hazırlanmıştır. Ancak, önce şu soruyu ele alalım: Bir sunucu taraflı programlama dili tam olarak nedir?

Sunucu taraflı programlamanın ne demek olduğunu anlamak için, önce istemci taraflı programlama dillerinden biri olan HTML'in çalışma prensibine bakalım.

HTTP (HyperText Transfer Protocol), web üzerindeki bilgi dağıtımı için kullanılan temel protokoldür. HTTP, dosyaların kolayca transfer edilebilmesi için oldukça etkin ve hızlıdır. Web teknolojileri ile birlikte gelişen HTTP’nin temel özellikleri, ilk sürümü olan HTTP/0.9'dan itibaren mevcuttur. Ancak HTTP/0.9 sürümünde birçok eksiklik vardı. Örneğin, içerik tiplemesine (content typing) izin vermemesi ve istek ve cevaplarda meta-bilgilerin kullanılmaması gibi.

Bu eksiklikleri gidermek için HTTP'nin şu anda kullandığımız sürümü olan HTTP/1.0 geliştirilmiştir. Bu sayede Content-Type (içerik tipi) alanı bulunan başlıklara ve diğer meta bilgilere izin verilmiştir. Aktarılan verinin tipi Content-Type alanında tanımlanır. Ayrıca, dil, verinin kodlanma tipi ve durum bilgisi gibi veriler hakkında başka bilgiler de sağlayabilirsiniz.

Çoğu web kullanıcısı ve yayıncısının HTTP'de istediği özelliklerden biri de güvenliktir. Web yayıncıları ve kullanıcıları, güvenli olarak işlemleri (transaction) iletebilmeyi istemektedir. Elektronik ticaretin yaygın olarak kullanılması için güvenlikle ilgili anahtar nokta, işlemlerin güvenliğini sağlayabilme ve şifreleyebilmektir. Şu anda HTTP'nin güvenlik özellikli sürümleri için çeşitli taslaklar bulunmaktadır. Bu özelliklerden biri kabul gördüğünde, HTTP kullanan güvenlikli işlemler hayal olmaktan çıkacaktır.

### 1.1.1 Bağlantısız Protokoller ve Bağlantılı Protokoller

HTTP etkin bir protokoldür çünkü hızlı, kullanışlı ve yeteneklidir. Bu hız ve yetenekliliği sağlayabilmek için HTTP, bağlantısız (connectionless) ve durumsuz (stateless) bir protokol olarak tanımlanmıştır.

Bağlantısız Protokoller: HTTP, bağlantısız bir protokoldür. Bağlantısız protokoller, bağlantı temelli protokollerden isteklere verilen cevaplar noktasında ayrılırlar. Bağlantısız bir protokolde, istemci sunucuya bağlanır, bir istekte bulunur, cevap alır ve ardından diğer isteklere hizmet vermek için bağlantıyı keser.

Bağlantılı Protokoller: Bağlantı temelli bir protokol örneği ise FTP'dir. Bir FTP sunucusuna bağlanıldığında, dosya aktarımı bittikten sonra bile bağlantı devam eder. Bu bağlantının korunabilmesi için sistem kaynakları gerekir. Çok sayıda açık bağlantı tutmak, sunucunun kolaylıkla çökmesine sebep olabilir. Bu nedenle, FTP sunucularının çoğu, belirli bir anda en fazla 250 açık bağlantıya izin verecek şekilde ayarlanır. Bu, FTP sunucusuna aynı anda en fazla 250 kullanıcının bağlanarak işlem yapabileceği anlamına gelir (bu özellik çeşitli FTP sunucularında değişiklik gösterebilir). Ayrıca, doğru sonlandırılmayan bağlantılar da problemler yaratabilir. Bu tip protokollerde, işlemler kontrolden çıkarak sunucunun çökmesi gibi istenmeyen sonuçlar doğurabilir.

Buna karşılık, HTTP bağlantısız bir protokoldür. İstemciler sunucuya bağlanır, istekte bulunur, cevabını alır ve ardından bağlantıyı keser. Bağlantı sürekli korunmadığı için işlemler tamamlanırken sistem kaynakları meşgul edilmez. Sonuç olarak, HTTP sunucuları sadece aktif bağlantılarla sınırlıdır ve az bir sistem yükü ile binlerce işleme olanak tanır.

Bağlantısız protokollerin dezavantajı ise aynı istemcinin tekrar bilgi talebinde bulunduğunda bağlantının yeniden kurulması gerekliliğidir. İstemci tarayıcıları, bu yükü en aza indirebilmek için alınan verileri saklayarak, tekrar aynı veri istendiğinde kullanan sistemler geliştirmişlerdir.

### 1.1.2 Durumu Olmayan Protokoller ve Durumu Olan Protokoller

HTTP, durumu olmayan (stateless) bir protokoldür. Durumu olmayan protokoller, durumu olan protokollerden isteklerle ilgili bilgilerin tutulma şeklinde farklılık gösterirler. Durumu olmayan protokollerde, işlem tamamlandıktan sonra işlem hakkında bilgi saklanmaz. Durumu olan protokollerde ise işlem tamamlandıktan sonra durum bilgisi saklanır.

Durumu Olan Protokoller: Durumu olan protokol kullanan sunucular, işlemlerle ilgili bağlantının durumu, çalışmakta olan işler ve bu işlerin durumu gibi bilgileri tutar. Genel olarak bu durum bilgisi bellekte kalır ve sistem kaynaklarını kullanır. İstemci, durumu olan bir protokol kullanan sunucu ile bağlantısını kestiğinde, bu durum bilgisi silinmeli ve oturum sonlanmalıdır.

Durumu Olmayan Protokoller: Durumu olmayan protokoller, küçük hacimlidir. Bu protokolleri kullanan sunucular, tamamlanmış işlemler ve işler hakkında hiçbir bilgi tutmazlar. Bir istemci, durumu olmayan protokol kullanan sunucu ile olan bağlantısını kestiğinde, hiçbir verinin silinmesine ya da oturumun sonlandırılmasına gerek kalmaz. Durum bilgisinin kaydının tutulmaması sayesinde sunucu üzerine daha az yük biner ve sunucular işlemleri hızlı bir şekilde işleyebilirler. Web yayıncıları için bu protokolün dezavantajı, eğer web dokümanları için durum bilgisinin tutulmasını istiyorsanız, bunun doküman başlığında meta-bilgi şeklinde belirtilmesi gerektiğidir.

## 1.2 HTML Nedir?

Tim Berners-Lee, webi tasarlarken herkesin webde yayımcılık yapmasına olanak sağlayacak kadar genel ve kullanımı kolay bir arayüz oluşturmayı hedefledi. CERN'deki çalışma arkadaşlarıyla birlikte HTML dilini geliştirdiler. HTML, SGML'nin (Standart Genelleştirilmiş İşaretleme Dili) bir alt kümesi olarak tasarlanmıştır. HTML'in SGML'yi temel alması, web için geliştirilen bu dilin platformlar arası bir çözüm olarak kendini kanıtlamış, sağlam bir standartta kök salmasına sebep olmuştur.

HTML dili oluşturulurken SGML'den sadece gerekli olan alanlar alınmıştır. Bu sayede HTML'in karmaşıklığı ve dokümanların ağ üzerinden transferi için harcanan kaynakların miktarı oldukça azaltılmıştır. HTML'in SGML'yi temel almasının bir diğer avantajı ise doküman tanım tiplerinin (DTD) HTML standartlarını genişletmek için kolaylık sağlamasıdır. Sonuçta HTML'i geliştirenlerin amacı, zaman içinde geliştirilebilen basit bir dil yazmaktı.

HTML (HyperText Markup Language), web sayfalarının yapılandırılması ve sunulması için kullanılan standart bir işaretleme dilidir. Web tarayıcıları, HTML belgelerini alır, yorumlar ve kullanıcıya görsel bir sunum olarak gösterir.

### 1.2.1 HTML'in Çalışma Prensibi

HTML dilinin işleyiş prensibi oldukça basittir; kullanıcı, tarayıcısına (web browser) bir URL girdiği zaman (URL hakkında daha fazla bilgi için EK-1: URL'ler Hakkında Ayrıntılı Bilgi'ye bakınız), istek bu URL'ye sahip sunucuya gider. Örneğin, "http://www.akkoyun.net/test.htm" şeklinde bir giriş olduğunda tarayıcı, sunucudan "test.htm" dosyasını ister. Sunucu ise bu dosyayı kendi iç dosya yapısında arayarak bulur ve dosyayı aynen tarayıcıya yollar.

Eğer istek sonrasında dosya bulunamazsa, sunucu standart hata kodlarından "404 - Sayfa Bulunamadı" hatasını HTTP başlık bilgisi olarak tarayıcıya geri yollar. Tarayıcı, bu hata kodunu kendine göre yorumlayarak kullanıcıya gösterir.

Dosya sistemi tarafından bulunan doküman tarayıcıya yollanır. Tarayıcıya gelen HTML belgesi, tarayıcı tarafından yorumlanarak işletilir ve ekranda gösterilir. Burada vurgulanması gereken önemli bir nokta, HTML'in sunucu tarafında işletilmeyip tamamen istemci tarafında (client-side) işletiliyor olmasıdır. Böylece sunucu üzerinde hiçbir işlem yapılmadığı için sunucuya yük binmez.

Ek Bilgiler
HTML ve CSS: HTML, yapıyı tanımlarken, CSS (Cascading Style Sheets) bu yapının stilini tanımlar. HTML ve CSS birlikte kullanılarak görsel açıdan zengin ve işlevsel web sayfaları oluşturulur.
HTML5: HTML'in en güncel sürümü olan HTML5, ses ve video oynatma, çizim ve animasyonlar, web depolama ve daha birçok yeni özellik ile HTML'in yeteneklerini genişletmiştir.
Tarayıcı Uyumluluğu: Farklı tarayıcılar HTML'yi farklı şekillerde yorumlayabilir. Bu nedenle, web geliştiricileri, web sayfalarının tüm ana tarayıcılarda tutarlı görünmesini sağlamak için dikkatli olmalıdır.

## 1.3 ASP’nin Doğuşu

HTML'in gelişmesiyle birlikte, kullanıcılara web sayfalarına bilgi girebilmeleri için olanak tanındı (<input> elementi yardımıyla). Bu yenilikle birlikte birçok uygulama geliştirildi, çünkü artık kullanıcılar da sunucuya bilgi gönderebiliyordu. Ancak çoğu uygulamada, bu kullanıcıdan gelen bilgilerin anında işlenmesi ve yeniden bir metin tabanlı HTML dokümanı haline getirilmesi gerekiyordu. Bu, hızlı bir yöntem değildi ve sunucu tarafında büyük bir yük oluşturuyordu.

Bu zorluğu aşmak isteyen geliştiriciler, CGI (Common Gateway Interface) arabirimini geliştirdiler ve standart hale getirdiler. CGI tamamen "C" dili üzerine kuruldu. "Cgi-bin" dizini de bu şekilde doğdu ("bin" terimi, derlenmiş "C" kodu olmasından dolayı "binary code" anlamında eklenmiştir). İlk uygulamalar, derlenmiş ufak programcıklar şeklindeydi. Ancak bu haliyle bile kullanışlı değildi, çünkü dosya içinde yapılacak en ufak değişiklikte bile yeniden derlenmesi gerekmekteydi. Bu da CGI’ın kullanımını olumsuz yönde etkiliyordu.

Bu kısıtlamaları aşmak isteyen geliştiriciler, yeni bir script dili geliştirdiler. Bu dil "Practical Extraction and Report Language" yani PERL adını aldı. PERL, sunucu ile iletişim halinde olan ilk dildi ve "C" veya "C++" dilleri ile yazılan scriptlerin her seferinde derlenmesi derdini ortadan kaldırmış oldu. PERL, esnekliği ve metin işleme yetenekleri sayesinde hızla popülerlik kazandı ve özellikle Unix ve Linux tabanlı sistemlerde yaygın olarak kullanıldı.

Ancak, web geliştirme sürecini daha da kolaylaştırmak ve verimli hale getirmek için Microsoft, 1996 yılında ASP (Active Server Pages) teknolojisini tanıttı. ASP, sunucu tarafında çalışan script dillerini (genellikle VBScript veya JScript) kullanarak dinamik web sayfaları oluşturmayı kolaylaştırdı. ASP, HTML kodlarının içine gömülü scriptler yardımıyla çalışır ve sunucuda işlenerek istemciye (kullanıcıya) dinamik içerik sunar.

### 1.3.1. ASP'nin Özellikleri ve Avantajları

* Kolay Kullanım: ASP, HTML ile entegre çalıştığı için web geliştiricileri tarafından kolayca öğrenilip kullanılabilir.
* Dinamik İçerik Üretimi: Kullanıcı etkileşimlerine göre dinamik olarak içerik üretebilir. Örneğin, bir kullanıcı form gönderdiğinde, ASP bu bilgiyi işleyerek uygun bir yanıt oluşturur.
* Veritabanı Entegrasyonu: ASP, çeşitli veritabanlarıyla (örneğin, Microsoft Access, SQL Server) kolayca entegre olabilir. Bu sayede, web uygulamaları dinamik ve veritabanı destekli içerik sunabilir.
* Oturum Yönetimi: ASP, kullanıcı oturumlarını yönetmek için yerleşik destek sunar. Bu, kullanıcıların kişiselleştirilmiş içerik ve deneyim elde etmelerini sağlar.
* Genişletilebilirlik: ASP, üçüncü parti bileşenlerin (COM/DCOM) entegrasyonuna izin verir, bu da geliştiricilerin mevcut işlevselliği genişletmesine olanak tanır.

### 1.3.2. ASP'nin Etkisi ve Mirası

ASP, web geliştirme sürecini hızlandırmış ve daha dinamik, etkileşimli web uygulamaları oluşturmayı mümkün kılmıştır. ASP'nin popülaritesi, Microsoft'un daha gelişmiş bir platform olan ASP.NET'i geliştirmesiyle devam etmiştir. ASP.NET, daha güçlü ve esnek bir framework sunarak, ASP'nin temel yeteneklerini genişletmiş ve modern web uygulamalarının geliştirilmesini kolaylaştırmıştır.

ASP'nin tanıtılması, sunucu taraflı programlamada önemli bir dönüm noktası olmuştur ve dinamik web içeriği üretiminin temellerini atmıştır. Günümüzde ASP'nin yerini ASP.NET almış olsa da, ASP'nin web teknolojilerinin gelişimindeki rolü büyüktür ve hala bazı eski sistemlerde kullanılmaktadır.

### 1.3.3. Sunucu-Tabanlı Script Teknolojileri

#### 1.3.3.1 CGI ve Perl

Şimdiye kadar anlattığım CGI (Common Gateway Interface) dilleri, web sunucusu üzerine bir yama yapmadan ya da ekstra bir program yüklemeden çalışmazlar. Bu programlar, kullanıcıdan gelen isteği algılar ve isteğe göre dosyayı okur, daha sonra onu sunucu içinde işler ve bir çıkış dosyası oluşturarak kullanıcıya sunarlar.

Perl, ilk popüler sunucu-tabanlı uygulama geliştirme dili olarak literatüre geçmiştir. Perl'in esnekliği ve güçlü metin işleme yetenekleri, web geliştirme için ideal bir araç haline gelmesini sağladı. Ancak zamanla, özellikle Unix ve Linux tabanlı sunucularda, yeni nesil bir programlama dili olan PHP (Hypertext Preprocessor) Perl'in yerini aldı. PHP, öğrenmesi kolay ve web geliştirme için özel olarak tasarlanmış olması nedeniyle hızlı bir şekilde popülerlik kazandı.

#### 1.3.3.2. Microsoft'un Atılımları ve ISAPI

Microsoft, web sunucu sektöründeki en önemli atılımını "Windows NT 3.51" ve bu işletim sistemine entegre halde olan "Internet Information Server 1.0" (IIS) sayesinde yapmıştır. Bu yazılım, geçmişe dönük olarak CGI desteği sağlamakla (C ve C++ dilleri ile geliştirilmiş uygulamaları) birlikte, yeni bir arabirim de içeriyordu.

Bu arabirime "Internet Server Application Programming Interface" yani ISAPI adı verilmiştir. ISAPI, web sunucusuna daha derinlemesine ve verimli bir şekilde entegre olabilen uygulamaların geliştirilmesini sağladı. Perl dilinin esnekliği ve gücü, ISAPI ile standart hale getirilmiş oldu. Bu atılımla birlikte, tüm yazılım geliştiriciler ISAPI ile uyumlu olan yazılımlar geliştirmeye başladılar.

#### 1.3.3.3. ASP'nin Tanıtılması

Microsoft, ISAPI ile beraber yeni bir teknoloji olan ASP'yi (Active Server Pages) duyurdu. ASP teknolojisi, IIS (Internet Information Services) ile ISAPI sayesinde bağlanmış oldu. ASP'den önce en çok "Internet Database Connector" (IDC) kullanılmaktaydı. IDC, dinamik veritabanı bağlantılarına olanak tanıyan basit bir araçtı, ancak ASP'nin sunduğu esneklik ve güçten yoksundu.

ASP, dinamik web sayfaları oluşturmak için sunucu tarafında çalışan script dillerini (genellikle VBScript veya JScript) kullanır. ASP, HTML kodlarının içine gömülü scriptler yardımıyla çalışır ve sunucuda işlenerek istemciye dinamik içerik sunar. Bu, daha etkileşimli ve kullanıcı dostu web uygulamalarının oluşturulmasını mümkün kılar.

#### 1.3.3.4. ASP'nin Yapısı ve İşleyişi

ASP, istemciden gelen isteği alır, sunucuda bu isteği işler ve dinamik olarak oluşturulmuş bir HTML sayfasını istemciye geri gönderir. Bu süreç aşağıdaki adımlarla gerçekleşir:

* İstemci İsteği: Kullanıcı, web tarayıcısı aracılığıyla bir ASP sayfası talep eder.
* Sunucu İşlemi: IIS, gelen isteği ASP motoruna iletir.
* Script İşleme: ASP motoru, sayfadaki scriptleri çalıştırır ve dinamik içerik üretir.
* HTML Çıkışı: Dinamik olarak oluşturulan HTML sayfası, istemciye geri gönderilir.

Aşağıdaki diyagramda (Diyagram 1: Microsoft Sunucu Yapısı) Microsoft'un sunucu yapısı ayrıntılı olarak anlatılmıştır. Bu yapı, ASP'nin nasıl işlediğini ve IIS ile olan entegrasyonunu göstermektedir.

### 1.3.2. ASP ile IIS İlişkisi

ASP, sadece kendisi için yazılmış olan bir DLL (Dynamic Link Library) kullanır, bu dosya genellikle "asp.dll" olarak adlandırılır. Bu dosya, standart olarak web sunucusunda yer alır (sadece IIS 1.0 ve sonrasında). Genellikle "Winnt\System32\inetsrv" dizininde bulunur.

ASP DLL'si, ASP dosyalarını (genellikle ".ASP" uzantılı) okur, içlerindeki script komutlarını işler ve sonuçlarını HTML ve metin içeriği ile birlikte web tarayıcısına göndermek için görevlendirilmiştir. Bu, ASP'nin dinamik içerik oluşturma yeteneğini sağlar. ASP dosyaları, sunucu tarafında işlendiğinden, istemciye (kullanıcıya) sadece sonuçları gönderilir, böylece istemci tarafında ek işlem yapılmasına gerek kalmaz.

ASP'nin bu işleyiş şekli, IIS (Internet Information Services) ile sıkı bir şekilde entegre olduğunu gösterir. IIS, ASP dosyalarını işleyebilmek için ASP DLL'sini kullanır ve dinamik web sayfalarının oluşturulmasını sağlar. Bu sayede, ASP kodu içeren web siteleri, IIS üzerinde sorunsuz bir şekilde çalışabilir ve istemcilere dinamik içerik sunabilir.

ASP ve IIS'in bu yakın ilişkisi, web geliştiricilerin dinamik ve etkileşimli web siteleri oluşturmasını mümkün kılar. IIS'in güçlü altyapısı ve ASP'nin esnekliği sayesinde, kullanıcılarla etkileşim halinde olan zengin içerikli web uygulamaları geliştirilebilir.

### 1.3.3. IIS Uygulama Yapıları

IIS içindeki işlemleri daha iyi anlamak için Windows içindeki uygulama yapılarını anlamak önemlidir. Her web sitesinin sunucu üzerinde bir kök dizini vardır. Varsayılan (Default) web sitesi otomatik olarak “c:\inetpub\wwwroot” dizinini kök dizin olarak atar (ancak bu değiştirilebilir). Her yeni web sitesi için bir kök dizini belirlenmesi zorunludur. Sunucu üzerindeki web sitelerini görmek için IIS yönetim arayüzü olan “Internet Service Manager” programı kullanılır.

Default web sitesine sağ tıkladığınızda "özellikler" seçeneğini seçtiğinizde, açılan "default web site özellikleri" ekranından “home directory” sekmesine tıklayabilirsiniz. Ardından, "configration" butonuna tıklayarak uzantılar ile ilişkilendirilen arabirimleri görebilirsiniz. (ISAPI yapısı "Diyagram 1: Microsoft Sunucu Yapısı"nda görüldüğü gibidir).

Resimde (Resim 2: ISAPI Yapısı) görülebileceği gibi, ASP uzantılı dosyalar ASP M, HTML uzantılı HTML sayfaları ve XML uzantılı XML dosyaları okunup (web sunucusu tarafından) istemciye gönderilir. Ancak ASP uzantılı dosyalar, ISAPI yardımıyla asp.dll tarafından okunur, derlenir ve ardından sonuç çıktıları istemciye gönderilir.

İstemci sunucudan bir dosya talep ettiğinde istek paketi aşağıdaki gibi olacaktır:

```HTML
GET /deneme/ornek.asp HTTP/1.1
Accept: application/msword, application/vnd.ms-excel, application/vnd.ms-powerpoint, image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, application/x-comet, */*
Accept-Language: en-us
Encoding: gzip, deflate
Referer: http://www.akkoyun.net/kitap.asp
Cookie: VisitCount=2&LastDate=6%2F4%2F99
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows XP)
Host: 212.98.198.111
Connection: Keep-Alive
Bu isteği takiben, sunucu istemciye dosyayı gönderirken şu başlık kısmıyla birlikte gönderecektir:
```

```HTML
HTTP/1.1 200 OK
Server: Microsoft-IIS/5.0
Connection: Keep-Alive
Date: Thu, 11 Nov 2002 10:22:21 GMT
Content-Type: text/html
Accept-Ranges: bytes
Content-Length: 2946
Last-Modified: Thu, 11 Nov 2002 10:22:21 GMT
Cookie: VisitCount=2&LastDate=6%2F4%2F99

<HTML>
--Sayfanın geri kalanı
```

Bu bilgileri dikkatlice incelemeniz önemlidir çünkü ilerideki konularımızda bu sunucu ve istemci işlemlerini okutmak ve değiştirmek üzerine konular göreceğiz.

### 1.3.4. ASP Dosyalarının İşlenmesi

ASP dosyaları, genellikle "asp.dll" adlı bir DLL dosyası tarafından derlenir ve işlenir. Bu derleme süreci, ASP dosyasının içeriğine bağlı olarak farklı şekillerde gerçekleşir.

Sunucu Taraflı Kod Kontrolü: İlk olarak, ASP dosyası içinde sunucu taraflı kod olup olmadığı kontrol edilir. Eğer dosya içerisinde sunucu taraflı kod bulunmuyorsa, IIS (Internet Information Services) tarafından doğrudan istemciye gönderilir. Bu, Windows 2000'den sonra gelen bir özelliktir ve sunucu taraflı işlem gerektirmeyen dosyalara bile ".asp" uzantısı verilmesine olanak tanır.

Sunucu Taraflı Kod İşleme: Eğer dosya içinde sunucu taraflı kod bulunursa, ASP dosyası satır satır işlenir. İçerisindeki script blokları içerisindeki komutlar işletilir ve sonuçlar bir çıkış dosyasına (genellikle hafızada bir tampon bölgesine) kaydedilir. Script komutu içermeyen kısımlar aynen değiştirilmeden tampona yazılır.

İşlenen Dosyanın İstemciye Gönderilmesi: Tüm işlemler tamamlandıktan sonra, eğer herhangi bir hata oluşmamışsa, bu işlenen dosya (tampon bölgede tutulan) istemci bilgisayarına gönderilerek işlem tamamlanır. Bu gönderme işlemi tamamlandıktan sonra tampon bölge temizlenir.

ASP dosyalarının işlenmesi sırasında kullanılacak olan sistem kaynakları belirli bir yapı içinde çalışır. Bu yapı, işlemi daha iyi anlayabilmek için Windows içinde nasıl çalıştığını anlamak önemlidir.

ASP'nin gelişmiş yetenekleri sayesinde, bazı güvenlik önlemleri alınabilir. Örneğin, "File System Object" (FSO) kullanarak yazılmış ASP dosyaları, sunucudaki diğer dosyalara erişebilir. Ancak, bu, sunucu yöneticilerinin doğru yetkilendirme yapmaması durumunda bir güvenlik açığına neden olabilir. Bu tür sorunları önlemek için, sunucuda kullanıcıların ve dosya erişim izinlerinin doğru şekilde yapılandırılması önemlidir.

Bilgi: Bazıları tarafından bilinse de, ASP'de File System Object (FSO) kullanılarak yazılan ve sunucudaki diğer dosyalara erişimi sağlayan bir script bulunmaktadır. Birçok programcının düşüncesinin aksine, bu, bir güvenlik açığı olarak kabul edilmez. Bunun nedeni, bu scriptin sunucu yöneticilerinin yanlış veya yetersiz yetkilendirme yapması sonucu kendi yetkisi dışındaki dosyalara erişebilmesidir. Bu yanlış yetkilendirmeyi düzeltmek için aşağıdaki adımları izleyebiliriz:

Web sunucusunun "root" klasörüne (tüm web sitesi dosyalarının bulunduğu sanal olarak en üst dizin) bir klasör oluşturun. Örneğin, bu klasörün adı "guvenli" olabilir.
"Computer Management" üzerinden "Local Users and Groups" seçeneğine tıklayın. Burada "Users" seçeneğini seçerek, sağ tarafta bilgisayarda kayıtlı tüm kullanıcıların listesini göreceksiniz. Boş bir yere sağ tıklayarak "new user" seçeneği ile yeni bir kullanıcı oluşturun.
Ardından tekrar "Computer Management" penceresinden "Services and Application" bölümüne gidin ve buradan "IIS" sekmesine tıklayın. "Default Web Site" sekmesini işaretleyin. Sağ tarafta, web sayfanız içindeki tüm klasörlerin listesi görünecek. Oluşturduğumuz "guvenli" klasörü de burada olmalıdır.
"guvenli" klasörüne sağ tıklayarak "Properties" seçeneğini seçin. Açılan pencereden "Directory > Application Settings" bölümündeki "create" düğmesine tıklayın. Bu, seçili klasör için ayrı bir uygulama oluşturacaktır.
Artık bu klasöre yetkili bir kullanıcı seçebilirsiniz. Bu kullanıcının "IUSER" (İnternet Kullanıcısı) olmasına gerek yoktur.
"Directory Security > Anonymous access and auth.. control" bölümünde "Edit" düğmesine tıklayın. "Anonymous access" bölümündeki "Edit" düğmesine tıklayarak açılan pencereden önceden oluşturduğunuz kullanıcıyı seçin.
"Allow IIS to control password" seçeneğini aktif hale getirin ve OK'ye tıklayarak çıkın.
"c:\inetpub..." gibi dizinlerden "guvenli" klasörünü bulun. Klasöre sağ tıklayarak "Properties" seçeneğini seçin.
"Security" sekmesine gidin ve buradaki tüm kullanıcıları silin. Sadece Administrator ve oluşturduğunuz kullanıcıyı ekleyin.
Eğer bu klasörde dosya oluşturma izni vermek istiyorsanız, kendi kullanıcınıza "modify, write veya full control" izni verebilirsiniz.
Son olarak, OK'ye tıklayarak pencereden çıkın.
Artık "http://127.0.0.1/guvenli" klasöründen FSO kullanarak c:\ dizinine veya bir üst klasöre erişim sağlayamazsınız. Bir üst klasördeki "txt" dosyalarını bile okuyamazsınız.

#### 1.3.4.1. <% ve %> Kullanımı

En yaygın kullanılan yöntem, sunucu taraflı kodu işaretlemek için "<%" ve "%>" arasında yazmaktır. Bu şekilde, bir HTML belgesi içinde sunucu taraflı kod bloğu tanımlanabilir. Örnek olarak:

```HTML
<HTML>
	<Body>
		Bu bir HTML metnidir
		<%
		' Burası bir script bloğudur
		Response.Write("Bu ASP kodu çalıştırılır ve çıktı HTML sayfasına eklenir.")
		%>
	</Body>
</HTML>
```

Bu kod örneğinde, "<%" işaretiyle başlayan ve "%>" işaretiyle biten kısım arasına yerleştirilen kodlar, sunucu taraflı ASP kodunu temsil eder. Bu kodlar çalıştırıldığında, sonuç HTML sayfasına eklenir.

#### 1.3.4.2. <script> Elementini Kullanma

Nadir olarak kullanılan bir yöntemde sunucu taraflı kodun yer aldığı script bloğunu <script> elementiyle açıp </script> elementiyle kapatmaktır. Bu kullanımda element içerisine yazılacak olan "runat" özelliği sayesinde istemci veya sunucu taraflı çalışma özelliği eklenir. Örnek olarak:

```HTML
<HTML>
	<Body>
		Bu bir HTML metnidir
		<script runat="server">
			' Burası bir script bloğudur
		</script>
	</Body>
</HTML>
```

Ayrıca, <script> elementi kullanılarak sunucu üzerinde bulunan bir dosya script bloğu içerisine dahil edilebilir. Bu şekilde tüm sayfalarda kullanılan ortak kodlar bir defaya mahsus olmak üzere yazılır ve gereken yerlere dahil edilir. Örnek olarak:

```HTML
<HTML>
	<Body>
		<script runat="server" src="/script.inc"></script>
	</Body>
</HTML>
```

Bu uygulamada, içeriği dahil edilen "script.inc" dosyasının içerisinde geçerli bir script olmalıdır. Bu dosya, metin veya HTML içeremez ve <script> elementinin içerisine sadece script kodu eklenmelidir.

Bilgi: Harici dosyaları ASP dosyasına dahil etmek için Server Side Includes (SSI) kullanabiliriz. Bu yöntemle, script blokları içerisindeki kodları harici dosyalarda saklayabiliriz. Bu konuyu ilerideki konularımızda detaylı olarak ele alacağız.

### 1.3.5. Özel ASP Komutları

#### 1.3.5.1. Script Dilini Belirlemek

ASP, IIS tarafından desteklenen iki ana script motoru olan "VBScript" ve "JScript" ile birlikte gelir. Bunlar genellikle bir arada bulunurlar, ancak IIS ile birlikte gelmeyen ancak sonradan eklenebilen diğer script motorları da mevcuttur, örneğin TCL ve PerlScript.

ASP sayfalarında hangi script motorunun kullanılacağını belirlemek için dil tanımlama önemlidir. Bu genellikle IIS'de yapılandırılmış birkaç varsayılan değerle sağlanır. Bu, ASP sayfasının başında özel bir içerik tanımı kullanılarak gerçekleştirilir.

Örneğin, sayfanın hangi script diliyle çalışacağını belirtmek için şu kod kullanılır:

```asp
<%@Language = “dil”%>
```

Burada "dil" yerine "VBScript" veya "JScript" gibi bir değer kullanılabilir. Örneğin, VBScript için:

```asp
<%@Language = “VBScript”%>
```

Ve aynı şekilde, JScript için:

```asp
<%@Language = “JScript”%>
```

Bu tanımlama yapılsa da yapılmasa da, script bloğu <script> elementi içinde tanımlanmışsa, kullanılan dil orada da belirtilebilir.

Örneğin:

```HTML
<HTML>
	<Body>
		Bu bir HTML metinidir
		<script Runat = ”server” Language = “VBScript”>
			Rem burası script bloğudur ve dil olarak VBScript Kullanılmıştır
		</script>

		<script Runat = ”server” Language = “JScript”>
			Rem burası script bloğudur ve dil olarak JScript kullanılmıştır
		</script>
	</Body>
</HTML>
```

Bu örneklerde, VBScript ve JScript dillerinin nasıl belirlendiğini görebiliriz. Bu sayede, ASP sayfalarının hangi script diliyle yürütüleceği önceden belirlenebilir.

#### 1.3.5.2. CodePage

CodePage, bir web sayfasının karakter kodlamasını belirtmek için kullanılır. Karakter kodlaması, bilgisayarın metinleri nasıl okuyup işleyeceğini belirler. Örneğin, Latin alfabesindeki harfleri içeren bir web sayfası için "1252" veya "ISO-8859-1" karakter kodlaması kullanılabilir. Türkçe karakterlerin bulunduğu bir sayfa için ise "1254" veya "ISO-8859-9" karakter kodlaması tercih edilebilir.

CodePage, web sayfasının doğru bir şekilde işlenmesi ve gösterilmesi için önemlidir. Doğru karakter kodlaması kullanılmadığında, sayfanın içeriği yanlış okunabilir veya görüntülenebilir. Bu nedenle, özellikle farklı dilleri destekleyen çok dilli web siteleri için doğru karakter kodlaması seçimi kritik öneme sahiptir.

CodePage belirtme işlemi, ASP sayfasının başında <% %> etiketleri arasında yapılır. Örneğin:

```asp
<%@CODEPAGE = “1254”%>
```

Bu komut, web sayfasının "1254" karakter kodlamasını kullanacağını belirtir. Böylece, Türkçe karakterlerin doğru bir şekilde görüntüleneceği sağlanır.

Ayrıca, dil belirleme komutu olan LANGUAGE ile birlikte de kullanılabilir:

```asp
<%@CODEPAGE = “1254” LANGUAGE=“VbScript”%>
```

Bu örnek, sayfanın "1254" karakter kodlaması ve VBScript dilinde çalışacağını belirtir.

#### 1.3.5.3. LCID

LCID (Locale ID - Yerel Kimlik), bir web sayfasının bölgesel ayarlarını belirlemek için kullanılır. Bu ayarlar arasında para birimi simgesi, tarih ve saat biçimi, sayı biçimi ve diğer bölgesel özellikler yer alır. Örneğin, LCID değeri "2057" olarak ayarlandığında, İngiltere'nin bölgesel ayarları kullanılacak ve para birimi simgesi olarak "£" işareti gösterilecektir.

LCID belirtme işlemi, ASP sayfasının başında <% %> etiketleri arasında yapılır. Örneğin:

```asp
<%@LCID = “2057”%>
```

Bu komut, sayfanın İngiltere'nin bölgesel ayarlarını kullanacağını belirtir. Böylece, tarih, saat ve para birimi gibi değerler, İngiltere'ye özgü olarak görüntülenecektir.

Ayrıca, dil belirleme komutu olan LANGUAGE ve CodePage ile birlikte de kullanılabilir:

```asp
<%@CODEPAGE=“1254” LANGUAGE=“VbScript” LCID=“2057”%>
```

Bu örnek, sayfanın "1254" karakter kodlaması, VBScript dilinde çalışması ve İngiltere'nin bölgesel ayarlarını kullanması gerektiğini belirtir.

LCID belirleme, özellikle çok dilli web siteleri için önemlidir. Doğru bölgesel ayarların kullanılması, kullanıcıların web sayfasını daha doğru bir şekilde anlamasını sağlar ve kullanıcı deneyimini iyileştirir.

### 2. ASP'nin Temelleri

#### 2.1. Değişkenler

Tüm programlama dillerinde olduğu gibi, VBScript'te de değişkenler kullanılır. Değişkenler, sayısal veya metinsel değerleri doğrudan kullanmak yerine, bu değerleri temsil eden isimlerdir. Bu sayede program içinde daha esnek bir yapı kurabiliriz.

Değişkenleri temsil eden belirli adlar vardır, örneğin "isim", "tarih" veya "toplam" gibi. Değişkenin adı sabit kalır, ancak içerisinde tuttuğu değer programın akışına bağlı olarak değişebilir. Programcı, değişkenlerin değerlerini kontrol eder ve gerektiğinde güncelleyebilir. Bu esneklik, programların dinamik ve uyarlanabilir olmasını sağlar. Bu nedenle, değişken kavramı programlama dünyasında temel bir öneme sahiptir.

Değişken isimlendirmede kullanılan bazı yaygın stiller şunlardır:

- **Snake Case:** Değişken isimlerindeki kelimeler alt çizgi (_) ile ayrılır. Örneğin, `isim_soyisim` veya `dogum_tarihi`. Bu stil genellikle okunabilirliği artırır ve birçok programlama dilinde yaygın olarak kullanılır.
- **Camel Case:** İlk kelimenin tamamı küçük harflerle, sonrakilerin ilk harfi büyük harflerle yazılır. Örneğin, `isimSoyisim` veya `dogumTarihi`. Bu stil, JavaScript ve Java gibi dillerde sıkça tercih edilir.
- **Pascal Case:** Camel Case'e benzer, ancak ilk kelimenin ilk harfi de büyük harf ile başlar. Örneğin, `IsimSoyisim` veya `DogumTarihi`. Bu stil, genellikle sınıf ve fonksiyon isimlendirmelerinde kullanılır.

İyi bir değişken ismi seçmek, kodun okunabilirliği ve sürdürülebilirliği açısından önemlidir. Anlamlı ve tutarlı değişken isimleri, kodun anlaşılmasını kolaylaştırır ve hata yapma olasılığını azaltır.

Piyasada kullanılan bütün programlama dillerinde, değişkenler kullanılmadan önce tanımlanmalı ve boyutlandırılmalıdır. Ancak VBScript, bu dillerden farklı olarak değişkenlerin tanımlanması ve boyutlandırılmasını zorunlu kılmaz. VBScript'te tanımlanmamış değişkenler herhangi bir hata vermeden kabul edilir. Ancak, karmaşık programlarda bu durum sorunlara yol açabilir. Tanımlanmamış bir değişkene değer atanmadan başka bir yerde kullanılması, programda hatalara neden olabilir.

Bu tür sorunları önlemek için değişkenlerin kullanılmadan önce tanımlanması önerilir. Profesyonel bir programcı, değişkenlerini kullanmadan önce mutlaka tanımlar, böylece programın ilerleyen safhalarında değişken hataları yapmaz. Bu alışkanlığı zorunlu hale getirmek için VBScript'te `Option Explicit` komutu kullanılır. Bu komut, ASP yordamcısının her kullanılan değişkenin tanımlanmasını zorunlu kılar, böylece sayfalar arası değişken karmaşası oluşmaz ve değişken adlarının yanlış yazılması engellenir. `Option Explicit` komutunu mutlaka ASP programlarınızın ilk satırında kullanmalısınız, aksi halde program hata verecektir.

ASP'de değişken tanımlama işlemi için "Dimension" (boyut) kelimesinin kısaltılmışı olan `DIM` komutu kullanılır. Bu komut yardımıyla, kullanılan değişkenler kodun başlangıcında belirtilerek tanımlanır. İyi bir programlama pratiği olarak değişkenleri kodun başında tanımlamak daha iyidir, ancak sayfa içerisinde herhangi bir yerde değişken tanımlaması yapmak da bir hata oluşturmaz.

Örneğin:

```vbscript
Option Explicit
Dim isim, yas, dogumTarihi

isim = "Ahmet"
yas = 30
dogumTarihi = "01/01/1990"
```

Bu örnekte, isim, yas ve dogumTarihi değişkenleri Dim komutuyla tanımlanmış ve daha sonra bu değişkenlere değerler atanmıştır.

Option Explicit komutunu kullanmazsanız, değişkenleri tanımlamadan da kullanabilirsiniz, ancak bu durum kodunuzu karmaşık hale getirebilir ve hatalara neden olabilir. Örneğin:

```vbscript
<%
Ad = "Günce Akkoyun"
%>
```

Bu kod, Option Explicit komutu kullanılmadığı için hata vermeden çalışır. Ancak, kodun sağlamlığı ve okunabilirliği açısından Option Explicit komutunu kullanmak daha iyidir.

Option Explicit komutunu kullandığınızda, tanımlanmamış değişkenlerin kullanılması bir hata ile sonuçlanır:

```vbscript
<%
Option Explicit
Ad = "Günce Akkoyun"
%>
```

Bu kod, aşağıdaki hatayı verir:

```vbscript
Microsoft VBScript runtime (0x800A01F4)
Variable is undefined: 'Ad'
/asp/test.asp, line 3
```

Bu hata, değişkenin tanımlanmadan kullanıldığını belirtir. Doğru kullanım şu şekilde olmalıdır:

```vbscript
<%
Option Explicit
Dim Ad
Ad = "Günce Akkoyun"
%>
```

Bu şekilde yapılan değişken tanımlamalarında, programın ilerleyen aşamalarında tekrar Ad isimli bir değişken kullanamayacağınız için bir karışıklık olmayacaktır. Bu, büyük kodlar (10,000-20,000 satır) yazarken özellikle önemlidir ve kodunuzu daha okunabilir ve yönetilebilir hale getirir.

Temelde kullanılan değişken türleri sayısal, alfasayısal ve mantıksal olmak üzere üçe ayrılmaktadır. Ancak VBScript, diğer programlama dillerinde karşılaştığımız integer, real, string, boolean gibi temel veri tipi tanımlarından yoksundur. Peki VBScript bir değişkenin türünü nasıl anlar? Bunu o değişkene değer atandığı zaman anlar. Aşağıdaki atamaları inceleyelim:

```vbscript
<%
Option Explicit
Dim x, y, z, t, u
x = 15
y = "15"
z = "Zeytinyağlı yiyemem aman"
t = True
u = ASPgüncesi
%>
```

Yukarıdaki örnekte x değişkenine sayısal bir değer atandığı için, x değişkeni sayısal bir veri tipinde olacaktır. Bu nedenle tüm sayısal işlemlerde kullanılabilir. y değişkeni tırnak içerisinde olduğundan dolayı alfasayısal (string) veri tipinde olacaktır. Bu nedenle sayısal işlemlerde kullanılamaz ve sadece metinsel işlemlerde kullanılabilir. z değişkeni de metinsel bir değere sahiptir ve sayısal işlemlerde kullanılamaz. t değişkeni ise sadece “doğru” (true - 1) veya “yanlış” (false - 0) değerini alabilen bir boolean veri tipidir. u değişkeni ise tanımsız bir değişken olduğundan hata verecektir. Metin değişkenleri sadece tırnak işareti içerisinde tanımlanabilir, aksi halde hata oluşur.

Bu nedenle, değişkenleri doğru şekilde tanımlamak ve kullanmak programın doğruluğu ve kararlılığı açısından kritik öneme sahiptir.

#### 2.1.1. Seri Değişkenler

VBScript programları içerisinde bir değişkene birden çok değer verilebilmektedir. Bu olaya "Seri Değişken" denilmektedir. Örnek vermek gerekirse, 7 kişilik bir sınıftaki öğrencilerin isimlerini tek bir değişkende tutabiliriz.

```vbscript
<%
Option Explicit
Dim Ad(7)
Ad(0) = "Günce"
Ad(1) = "Naz"
Ad(2) = "Aras"
Ad(3) = "Mehmet"
Ad(4) = "Berna"
Ad(5) = "Liva"
Ad(6) = "Eren"
%>
```

Bu şekilde bir tanımlama ile “Ad” değişkenine 7 farklı değer vermiş olduk. Burada dikkat edilmesi gereken nokta, dizi elemanlarının indeks değerlerinin 0’dan başlamasıdır. Yani, Dim Ad(6) komutu ile 0’dan 6’ya kadar toplam 7 eleman içeren bir dizi oluşturmuş olduk. Bu örnekte neden her isim için farklı değişken kullanmadık da bir seri kullandık diye soracak olursanız, işte cevabı:

```vbscript
<%
Option Explicit
Dim Ad1, Ad2, Ad3, Ad4, Ad5, Ad6, Ad7
Ad1 = "Günce"
Ad2 = "Naz"
Ad3 = "Aras"
Ad4 = "Mehmet"
Ad5 = "Berna"
Ad6 = "Liva"
Ad7 = "Eren"
%>
```

Dizi değişkenlerde bir değişkeni numarasıyla kullanabilirsiniz. Ayrıca, indis numarası yerine bir değişken vererek kod içerisinde daha dinamik bir yapı oluşturabilirsiniz.

Örneğin:

```vbscript
<%
Option Explicit
Dim Ad(7)
Ad(0) = "Günce"
Ad(1) = "Naz"
Ad(2) = "Aras"
Ad(3) = "Mehmet"
Ad(4) = "Berna"
Ad(5) = "Liva"
Ad(6) = "Eren"

Dim numara
numara = 5
Response.Write Ad(numara)
%>
```

Bu şekilde bir kullanım ile Ad(5) değişkenini ekrana yazdırabilirsiniz. Bu örnekte, numara değişkenine 5 değeri atandı ve Ad(numara) şeklinde seri indisi olarak kullanıldı.

Bir dizinin boyutu ihtiyaçlarımızın altında kalırsa, yani elimizdeki toplam dizi öğesi adedi dizinin tanım aralığı dışında kalırsa, dizinin tanım aralığını yani boyutunu genişletebiliriz. Bunun için ReDim komutu kullanılır (yeniden boyutlandır). Ancak bu komutun şöyle bir özelliği vardır: yeniden tanımlanan bir dizi, yeniden tanımlama öncesi kendisine verilen hiçbir değeri yeniden boyutlandırma sonrası korumaz. Yani içeriği sıfırlanır.

Örneğin:

```vbscript
<%
Option Explicit
Dim Ad(5)
Ad(0) = "Günce"
Ad(1) = "Naz"
Ad(2) = "Aras"
Ad(3) = "Mehmet"
Ad(4) = "Berna"

ReDim Ad(7)
Ad(5) = "Liva"
Ad(6) = "Eren"
%>
```

Verilen örnekte yeniden tanımlama sonrası Ad(0) – Ad(4) değişkenleri sıfırlanacak yani null içeriğe sahip olacaktır. Yeniden tanımlama sırasında eski dizi değerlerinin korunmasını istersek ReDim komutunu Preserve komutuyla beraber kullanmalıyız. Bu şekilde eski verileri yeniden tanımlama sırasında korumuş oluruz.

Örneğin:

```vbscript
<%
Option Explicit
Dim Ad(5)
Ad(0) = "Günce"
Ad(1) = "Naz"
Ad(2) = "Aras"
Ad(3) = "Mehmet"
Ad(4) = "Berna"

ReDim Preserve Ad(7)
Ad(5) = "Liva"
Ad(6) = "Eren"
%>
```

Bu şekilde yeniden tanımlanan dizi eski değerlerinden hiçbir veriyi kaybetmez. Böylece eski veriler de kullanılabilir.

Dizi değişkenleri yukarıdaki örneklerdeki gibi tek boyutlu olabilmesinin yanında birden çok boyuta da sahip olabilirler. Örnek vermek gerekirse, matematikte kullanılan matris sistemini düşünün. Bu, iki boyutlu dizi hakkında fikir verecektir. Matris üzerinde birinci satır birinci sütunda bulunan değer x(0,0) şeklinde ifade edilmektedir. İki boyutlu diziler, satır ve sütun olmak üzere iki indeks ile tanımlanır. Örneğin, aşağıdaki 2x2 matrisi ASP içerisinde tanımlamamız için:

```vbscript
<%
Option Explicit
Dim x(2,2)

x(0,0) = 24
x(0,1) = 3
x(1,0) = 19
x(1,1) = 81
%>
```

Bu şekilde bir değişken ataması yapabiliriz. Bu atamada iki boyutlu bir matrisi VBScript’imize tanımlamış olduk. Benzer şekilde, üç ve dört boyutlu diziler de tanımlayabiliriz. Örneğin, üç boyutlu bir dizi şu şekilde tanımlanabilir:

```vbscript
<%
Option Explicit
Dim y(2,2,2)

y(0,0,0) = 5
y(0,0,1) = 10
y(0,1,0) = 15
y(0,1,1) = 20
y(1,0,0) = 25
y(1,0,1) = 30
y(1,1,0) = 35
y(1,1,1) = 40
%>
```

Bu örnekle, üç boyutlu bir diziyi tanımlamış olduk. Dizinin boyutu, her bir boyuttaki eleman sayısını belirler ve her boyut için indeksler 0’dan başlar. Bu nedenle, Dim y(2,2,2) komutu ile her boyutta 3 eleman içeren bir dizi oluşturmuş olduk.

Bu şekilde çok boyutlu diziler tanımlayarak, daha karmaşık veri yapıları oluşturabilir ve bu veri yapıları üzerinde çeşitli işlemler gerçekleştirebiliriz.

### 2. ASP'nin Temelleri

#### 2.2. Sabitler

Program içerisinde her zaman değişken değerleri değiştirilebilir değildir; bazen de sabit olmaları istenebilir. Bu tip değişkenlere (değişken demek biraz yanlış ama...) bir kere değer atandıktan sonra bir daha değiştirilemez. Sabit değer bütün program boyunca (hatta isterseniz bütün site boyunca) değişmeden kalır. Bu işlemi VBScript içerisinde "Constant" (sabit) kelimesinin kısaltılmışı olan `Const` komutu yardımı ile yaparız. `Const` terimi ile değer verilmiş bir değişkenin tanımlanmasına gerek yoktur.

Sabit olduğu `Const` komutu ile belirtilmiş bir değişkene:

```vbscript
<%
Option Explicit
Const Url = "www.akkoyun.net"
%>
```

Şeklinde kullanılan bir sabit değişkene başka bir veri atanmaya çalışıldığı zaman hata verecektir:

```vbscript
<%
Option Explicit
Const Url = "akkoyun.net"
....
Url = "stf.com.tr"
%>
```

Bu durumda "Url" sabitine kendi değeri dışında bir değer verilmek istenmiştir fakat hata mesajı verecektir (Illegal assignment: 'Url').

VBScript programlama dilinde kullanılan değişkenlere genel olarak variant denir. Variant genel olarak bir grubu temsil eder ve alt grupları mevcuttur. Bunlar değişkenin cinsine göre otomatik olarak tanınır. (Diğer programlama dillerinde bu özellik yoktur ve değişken tanımlanırken veri tipi yani variant grubu tanımlanır fakat ASP'de bu olay yordamcı tarafından otomatik gerçekleşir).

VBScript içerisinde kullanıcıya kolaylık sağlamak için bazı hazır tanımlanmış sabitler mevcuttur. Bu sabitler kodlama sırasında istenildiği şekilde kullanılabilmektedir. Bu sabitler ve değerleri aşağıda verilmiştir.

#### 2.2.1. Renk Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbBlack|&h00|Siyah|
|vbRed|	&hFF|Kırmızı|
|vbGreen|	&hFF00|	Yeşil|
|vbYellow|	&hFFFF|	Sarı|
|vbBlue|	&hFF0000|	Mavi|
|vbMagenta|	&hFF00FF|	Gül Kurusu|
|vbCyan|	&hFFFF00|	Turkuaz|
|vbWhite|	&hFFFFFF|	Beyaz|

#### 2.2.2. Tarih ve Zaman Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbSunday|	1|	Pazar|
|vbMonday|	2|	Pazartesi|
|vbTuesday|	3|	Salı|
|vbWednesday|	4|	Çarşamba|
|vbThursday|	5|	Perşembe|
|vbFriday|	6|	Cuma|
|vbSaturday|	7|	Cumartesi|
|vbUseSystem|	0|	Sunucu bölgesel ayarlarında yer alan tarih ve zaman biçimini kullan|
|vbFirstJan1|	1|	1 Ocak gününün içerisinde yer aldığı haftayı kullan|
|vbFirstFourDays|	2|	En az 4 günü yeni yılda olan ilk haftayı kullan|
|vbFirstFullWeek|	3|	7 günü de yeni yıl içerisinde yer alan ilk haftayı kullan|

#### 2.2.3. Tarih Biçim Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbGeneralDate|	0|	Sistem ayarlarındaki gösterim biçimi|
|vbLongDate|	1|	Uzun tarih gösterimi (June 26, 1943)|
|vbShortDate|	2|	Kısa tarih gösterimi (6/26/43)|
|vbLongTime|	3|	Uzun zaman gösterimi (3:48:01 PM)|
|vbShortTime|	4|	Kısa zaman gösterimi (15:48)|

#### 2.2.4. Çeşitli Sabitler

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbObjectError|	-2147221504|	Kullanıcı tarafından tanımlanan hata tipi numaraları|

#### 2.2.5. Mesaj Kutusu Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbOkOnly|	0|	Sadece OK butonunu göster|
|vbOkCancel|	1|	OK ve İptal butonunu göster|
|vbAbortRetryIgnore|	2|	Abort, Retry ve Ignore butonunu göster|
|vbYesNoCancel|	3|	Yes, No ve Cancel butonunu göster|
|vbYesNo|	4|	Yes ve No butonunu göster|
|vbRetryCancel|	5|	Retry ve Cancel butonunu göster|
|vbCritical|	16|	Kritik mesaj ikonunu göster|
|vbQuestion|	32|	Uyarı soru mesajı ikonunu göster|
|vbExclamation|	48|	Uyarı mesajı ikonunu göster|
|vbInformation|	64|	Bilgi mesajı ikonunu göster|
|vbDefaultButton1|	0|	İlk buton varsayılan|
|vbDefaultButton2|	256|	İkinci buton varsayılan|
|vbDefaultButton3|	512|	Üçüncü buton varsayılan|
|vbDefaultButton4|	768|	Dördüncü buton varsayılan|

#### 2.2.6. Mesaj Kutusu Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbOk|	1|	OK Butonu tıklandı|
|vbCancel|	2|	Cancel butonu tıklandı|
|vbAbort|	3|	Abort butonu tıklandı|
|vbRetry|	4|	Retry butonu tıklandı|
|vbIgnore|	5|	Ignore butonu tıklandı|
|vbYes|	6|	Yes butonu tıklandı|
|vbNo|	7|	No butonu tıklandı|

#### 2.2.7. String Cevap Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbCr|	chr(13)|	|
|vbCrLf|	chr(13) & chr(10)|	|
|vbFormFeed|	chr(12)|	Form besleme|
|vbLf|	chr(10)|	Satır besleme|
|vbNewLine|	chr(13) & chr(10) veya chr(10)|	Yeni satır|
|vbNullChar|	chr(0)|	0 değerine sahip karakter|
|vbNullString|	-|	""|
|vbTab|	chr(9)|	Horizantal Tab|
|vbVerticalTab|	chr(11)	Vertical Tab|

#### 2.2.8. Durum Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbTrue|	1|	Doğru|
|vbFalse|	0|	Yanlış|

#### 2.2.9. Karşılaştırma Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|VbBinaryCompare|	0|	Binary Karşılaştırma|
|VBTextCompare|	1|	Metin karşılaştırma|
|VBDataBaseCompare|	2|	Veri tabanı karşılaştırması|

#### 2.2.10. Değişken Tipi Sabitleri

| Sabit | Değer | Açıklama |
|-------|-------|----------|
|vbEmpty|	0|	Tanımsız data|
|vbNull|	1|	Geçerli data içermeyen|
|vbInteger|	2|	Integer|
|vbLong|	3|	Long|
|vbSingle|	4|	Single|
|vbCurrency|	6|	Currency|
|vbDate|	7|	Date|
|vbString|	8|	String|
|vbObject|	9|	Object|
|vbError|	10|	Error|
|vbBoolean|	11|	Boolean|
| vbVariant    | 12    | Variant                   |
| vbDataObject | 13    | Data Object               |
| vbDecimal    | 14    | Decimal                   |
| vbByte       | 15    | Byte                      |
| vbArray      | 16    | Array                     |

#### 2.2.11. Sürücü Tipi Sabitleri

| Sabit     | Değer | Açıklama                         |
|-----------|-------|----------------------------------|
| Unknown   | 0     | Tanımlanmamış sürücü             |
| Removable | 1     | Tüm taşınabilir medya aygıtları |
| Fixed     | 2     | Sabit diskler                    |
| Remote    | 3     | Network sürücüleri               |
| CDROM     | 4     | CDRom sürücüsü                  |
| RamDisk   | 5     | RAM üzerinde yer alan sanal disk alanı |

#### 2.2.12. Dosya Özelliği Sabitleri

| Sabit       | Değer | Açıklama                        |
|-------------|-------|---------------------------------|
| Normal      | 0     | Normal dosya                    |
| ReadOnly    | 1     | Salt okunur                     |
| Hidden      | 2     | Gizli                           |
| System      | 4     | Sistem dosyası                  |
| Directory   | 16    | Dizin                           |
| Archive     | 32    | Arşiv                           |
| Alias       | 1024  | Link veya kısayol               |
| Compressed  | 2048  | Sıkıştırılmış dosya             |

#### 2.2.13. Dosya Girdi Çıktı Sabitleri

| Sabit          | Değer | Açıklama                           |
|----------------|-------|------------------------------------|
| ForReading     | 0     | Sadece okuma için aç.              |
| ForWriting     | 1     | Dosyayı yazma için açar ve üzerine yazar |
| ForAppending   | 8     | Dosyayı yazmak için açar ve sonuna yazar |

#### 2.2.14. Özel Dizin Sabitleri

| Sabit           | Değer | Açıklama                                   |
|-----------------|-------|--------------------------------------------|
| WindowsFolder   | 0     | Windows kurulu olduğu dizin               |
| SystemFolder    | 1     | Fontların ve sürücülerin bulunduğu sistem dizini |
| TemporaryFolder| 2     | Temporary dizini                           |
