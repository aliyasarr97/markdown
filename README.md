# Make 
>Büyük ve karmaşık yazılım uygulamaları,yüzlerce kaynak kod ve diğer dosyalardan oluşur. Bu dosyaları derlemek ve birbirine bağlamak kolay değildir ve hatalı olabilir. Bu uygulamaların oluşturma işlemi sırasında, birkaç nesne dosyası da oluşturulur. Bu dosyaları ve tüm yazılım geliştirme projesini yönetmek için make komutu kullanılır.

>Make , makefile dosyasını okuyarak kaynak koddan yürütülebilir programları ve kitaplıkları otomatik olarak oluşturan bir derleme aracıdır.

>Kısacası, make tek bir komut kullanarak karmaşık yazılım uygulamalarını otomatik olarak oluşturmamıza, derlememize, yürütmemize ve hatta yüklememize olanak tanır.

 >Make, özellikle Unix ve Unix benzeri işletim sistemlerinde yaygın olarak kullanılmaya devam ediyor.
 
## Ortaya Çıkışı  
---
 >İlk olarak Stuart Feldman tarafından Nisan 1976'da Bell Laboratuarlarında oluşturuldu.Feldman, çalıştırılabilir dosyanın yanlışlıkla değişikliklerle güncellenmediği bir programında boş yere hata ayıklama konusunda bir iş arkadaşının deneyiminden yola çıkarak Make yazmak için ilham aldı.Feldman , bu yaygın aracın yazılmasından dolayı 2003 ACM Yazılım Sistemi Ödülü'nü aldı.Make'in tanıtımından önce,Unix yapı sistemi yaygın olarak programlarının kaynağına eşlik eden işletim sistemine bağlı "make" ve "install" komut dosyalarından oluşuyordu.Farklı hedefler için komutları tek bir dosyada birleştirebilmek, modern yapı ortamları yönünde önemli bir adımdı.          
 ![Stuart Feldman](https://i0.wp.com/citris-uc.org/wp-content/uploads/2019/07/Stu-Feldman_22-1-e1525717753249-1200x1200.jpg?resize=250%2C250&ssl=1)
 >* Stuart Feldman

## Temel Kurallar
---
>Başlıca kullanılan hedefler(target) aşağıdaki gibidr.Bu parametre isimlerini kullanmak zorunda değiliz ya da parametrelerimizi bunlarla sınırlamak durumunda da değiliz ama belli başlı görevler için (kurulum, silme vs.) en fazla kullanılan parametreler bunlardır.

| Hedef        | İşlev                                                |
| ---          | -----                                                |
|`make`        | Projeyi derler.                                      |
|`make install`| Derlenen projenin sisteme kurulmasını sağlar.        |
|`make clean`  | Derlenmiş olan proje dosyalarının silinmesini sağlar.|

>Make uygulaması çalıştırıldığında, bulunulan dizinde makefile dosyalarını arar.(Başka bir isme sahip bir makefile dosyası kullanmak istiyorsak bunu -f parametresi ile make programına bildirmeniz gerekir.)Make neyi nasıl yapacağını bu dosyalardan öğrenir. Eğer bulunduğumuz dizinde bir Makefile yok ise aşağıdaki gibi bir çıktı alacağız demektir:
```bash
$ make
make: *** No targets specified and no makefile found.  Stop.
```

# Makefile

>Makefile, projeleri otomatik olarak oluşturmaya ve yönetmeye yardımcı olan özel biçimli dosyalardır.

>Makefile aşağıdaki formattaki Rule’lardan oluşur.Rule’ların başında Target ismi verilerek Target’ın bağımlılıkları (başka Target’lar ya da dosyalar) verilir. İkinci ve sonraki satırlarda ise TAB karakteri ile Target’a göre Indent edilmiş komutlar bulunur. Bu komutlar make aracının Target’ı koşturması sırasında çalıştırılır.     
    
Rule sentaks’ı aşağıdaki gibidir.
```bash
target: bağımlılıklar
[tab] komutlar 1
[tab] komutlar 2
[tab] komutlar 3
```

>Make aracı, Makefile’daki komutları koşturmakla görevlidir.Make, Makefile’daki Target’ların (ki çoğu zaman Target’lar dosyalardır) bağımlılıklarının son güncelleme tarihlerini tutarak, ilgili Target’ın bağımlıklarının değişip değişmediği dolayısıyla da ilgili Target’ın yeniden koşturulmasına gerek olup olmadığına karar verir. Eğer Target’ın bağımlılıkları son koşturulma zamanından sonra değiştirilmediyse aynı girdilerle aynı çıktı meydana geleceği için make, Target’ın altındaki komutları koşturmaz.


## Bağımlılık Yapısı
---
>Target’lar genel olarak dosya isimleridir ancak dosya ismi olmak zorunda değildirler.Genellikle bir Rule’da sadece bir Target bulunmaktadır ancak birden fazla target bulunmaması için herhangi bir engel yoktur. Rule, Makefile’da iki şey ifade eder; birincisi Target’ın güncel olup olmadığı, ikincisi ise eğer güncel değil ise hangi komutlarla tekrar güncel hale getirilebileceğidir.

>Makefile’larda Shell (Terminal) komutları koşturabiliriz. Rule’larla kurguladığımız iş parçacıklarından birbirleri ile ilgili olanlar için bağımlılıklar tanımlanabilir ve iş parçacıkları Shell (genellikle Bash) komutları ile uygulanarak karmaşık sistemler tasarlanabilir.

 Örnek senaryo olarak Makefile ile hazırlanan bir Pipeline’ı olsun.

> * Eğer standart çıktı birimine istediğimiz yerlerde bazı uyarı mesajları göndermek istiyorsak echo komutunu kullanabiliriz
```bash
all: deploy 

build :
    echo "Building the project now";

unit_test : build
    echo "Running Unit Tests";

acceptance_test : unit_test
    echo "Running Acceptance Tests";

deploy : acceptance_test
    echo "Everything is fine deploying the app";
```

> İlgili Pipeline’da kaynak dosyaların Build edilmesi, başarılı Build sonrası Unit (Birim) Test’lerin çalıştırılması, Unit Test’lerin de başarılı olması ile Acceptance (Kabul) Test’lerin koşturulması, Acceptance Test’lerin de başarılı bir şekilde geçmesiyle uygulamanın Deploy edilmesi bulunmaktadır.

`Make all ` veya `make deploy ` komutlarını çalıştırınca aşağıdakine benzer bir çıktı görmeliyiz.

```bash
echo "Building the project now";
Building the project now
echo "Running Unit Tests";
Running Unit Tests
echo "Running Acceptance Tests";
Running Acceptance Tests
echo "Everything is fine deploying the app";
Everything is fine deploying the app
```
>Daha karmaşık Makefile’larda bir bağımlılık birden fazla Target için sağlanmış olabilir. Eğer make’in iki Target’ı birden çalıştırması gerekiyorsa bağımlılık sadece bir kere çalıştırılır.


## Phony Targets (Sahte Hedefler)
---

>Sahte hedef, gerçekten bir dosyanın adı olmayan hedeftir.Sahte bir hedef kullanmanın iki nedeni vardır: aynı ada sahip bir dosyayla çakışmayı önlemek ve performansı artırmak.

>Eğer Target’ın gerçekten dosyalarla ilişkisi yoksa Target PHONY olarak tanımlanır ve make’in Target’ı sürekli güncellemeye çalışması (dolayısıyla komutları çalıştırması) sağlanabilir.

Aşağıdaki Makefile’ı kopyaladığımız klasöre output adında bir dosya yaratalım.

```bash
all: output 

output :
    echo "Running Output Target"
```

>Terminal’de ilgili klasöre gidip make all ya da make output komutlarını verdiğimizde Target’ın koşturulmadığını göreceğiz.Aynı klasörde Target ile aynı isimde output bir dosya olduğu için ve dosya değiştirilmediği için make, Target’ın güncel olduğunu düşünmekte ve güncellemeye çalışmamaktadır.

>Eğer Target’ın gerçekten dosyalarla ilişkisi yoksa Target ` PHONY ` olarak tanımlanır ve make’in Target’ı sürekli güncellemeye çalışması sağlanabilir. Makefile’ı aşağıdaki gibi değiştirerek aynı adımları tekrarladığımızda bu kez Target’ın make tarafından güncellenmeye çalışılacağını görebiliriz.


```bash
.PHONY: output
all: output 

output :
    echo "Running Output Target"

```
