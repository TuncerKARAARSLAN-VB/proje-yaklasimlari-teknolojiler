# Mikroservis mimarisinde servisler arasındaki iletişim

Mikroservis mimarisinde servisler arasındaki iletişim, sistemin performansı, esnekliği ve güvenilirliği açısından kritik bir konudur. Servisler arasındaki iletişim, iki temel yöntemle sağlanır: **senkron** ve **asenkron** iletişim. Bu iletişim için kullanılabilecek farklı teknolojiler ve protokoller vardır. Aşağıda bu yöntemleri ve yaygın kullanılan teknolojileri inceleyelim.

## 1. **Senkron İletişim**

Senkron iletişimde, bir mikroservis başka bir servisten yanıt alana kadar bekler. Genellikle **istek-yanıt** (request-response) tarzında bir iletişim modelidir. Bu modelde kullanılan yaygın teknolojiler şunlardır:

### a) **HTTP/REST**

- **REST (Representational State Transfer)**, mikroservisler arasında iletişim için en yaygın kullanılan yaklaşımlardan biridir. Servisler, HTTP protokolü üzerinden JSON veya XML formatında veri gönderir ve alır.
- REST API'leri, sadeliği ve geniş destek nedeniyle popülerdir. Mikroservislerin bağımsız geliştirilmesini ve konuşlandırılmasını destekler.
- **Artıları**: Basit, geniş çapta desteklenir, standart HTTP sunucuları ve istemcileriyle uyumludur.
- **Eksileri**: Ağ gecikmesine duyarlıdır, ağır yük altında performans sorunları olabilir, büyük miktarda veri için uygun olmayabilir.

### b) **gRPC**

- **gRPC**, Google tarafından geliştirilen bir **Remote Procedure Call (RPC)** framework'üdür. HTTP/2 protokolünü kullanır ve genellikle **Protobuf** (Protocol Buffers) adlı bir serileştirme formatını kullanarak yüksek performans sağlar.
- REST'e göre daha hızlı ve daha az veri transferi gerektirir.
- **Artıları**: Performansı yüksektir, HTTP/2 kullanarak bağlantı sürekliliği sağlar, özellikle dil bağımsız iletişimlerde avantajlıdır.
- **Eksileri**: REST'e kıyasla daha karmaşıktır ve daha fazla öğrenme eğrisi gerektirir.

### c) **GraphQL**

- **GraphQL**, Facebook tarafından geliştirilen bir sorgu dilidir. Mikroservislerden gelen verilere esnek erişim sağlar. İstemciler sadece ihtiyaç duydukları verileri talep eder ve sonuç olarak fazla veri taşımaktan kaçınılır.
- **Artıları**: Daha esnek veri sorgulama, istemcilerin sadece ihtiyacı olan verileri çekmesi.
- **Eksileri**: Büyük veri kümeleri için karmaşık olabilir, özellikle daha basit API ihtiyaçları için aşırı karmaşık bir çözüm olabilir.

## 2. **Asenkron İletişim**

Asenkron iletişimde, bir mikroservis başka bir mikroservise istek gönderdikten sonra yanıtı beklemeden çalışmaya devam eder. Bu model genellikle mesajlaşma kuyrukları ve olay tabanlı sistemlerle uygulanır. Asenkron iletişimde yaygın kullanılan teknolojiler şunlardır:

### a) **RabbitMQ**

- **RabbitMQ**, yaygın kullanılan bir mesaj kuyruğu sistemidir. Mesajlar, mikroservisler arasında asenkron olarak taşınır ve mikroservisler bu mesajları işleyebilir.
- **Artıları**: Güvenilir mesaj iletimi, mesajların kalıcı olması (persistent message), farklı mesajlaşma desenlerini (pub/sub, request/response) destekler.
- **Eksileri**: Doğru yapılandırılmadığında performans düşüşü yaşanabilir.

### b) **Apache Kafka**

- **Apache Kafka**, büyük ölçekli dağıtık sistemlerde veri akışlarını ve gerçek zamanlı olayları işlemek için kullanılan bir dağıtık yayın-abone (pub/sub) mesajlaşma platformudur. Kafka, yüksek hacimli veri işleme için tasarlanmıştır.
- **Artıları**: Yüksek performans, büyük veri akışlarını işleyebilme, kalıcı mesajlaşma (durable message).
- **Eksileri**: Karmaşık konfigürasyon ve yönetim, öğrenme eğrisi yüksek olabilir.

### c) **Amazon SQS (Simple Queue Service)**

- **Amazon SQS**, AWS tarafından sağlanan tamamen yönetilen bir mesaj kuyruğu servisidir. Mesajlar mikroservisler arasında güvenli bir şekilde taşınır.
- **Artıları**: Yönetim gerektirmez, AWS ile tam uyumlu, yüksek ölçeklenebilirlik.
- **Eksileri**: AWS bağımlılığı, bazı özel gereksinimlerde sınırlı olabilir.

### d) **NATS**

- **NATS**, hafif ve yüksek performanslı bir mesajlaşma sistemidir. Pub/Sub modeliyle mikroservislerin mesaj alışverişini hızlı ve esnek bir şekilde yönetir.
- **Artıları**: Hafif, çok hızlı, düşük gecikme süresi.
- **Eksileri**: Kafka veya RabbitMQ'ya göre daha az özellik sunar.

### e) **ZeroMQ**

- **ZeroMQ**, bir mesajlaşma kütüphanesidir ve ağ üzerinde mesaj iletimi için kullanılır. Çok hızlı ve düşük gecikmeli bir çözümdür, ancak tam yönetilen bir mesaj kuyruğu sistemi değildir.
- **Artıları**: Performansı çok yüksektir, düşük gecikmeli uygulamalar için uygundur.
- **Eksileri**: Mesaj kuyruğu özelliklerini manuel olarak yönetmek gerekir.

### f) **AWS SNS (Simple Notification Service)**

- **Amazon SNS**, olay tabanlı mikroservis iletişimi için kullanılan bir pub/sub sistemidir. Mikroservisler arasında mesajları veya olayları dağıtır.
- **Artıları**: AWS ile tam uyumluluk, yönetim gerektirmeyen dağıtık sistem.
- **Eksileri**: Yalnızca AWS ekosisteminde kullanıma uygun.

## 3. **Olay Tabanlı İletişim (Event-Driven Communication)**

Mikroservisler arasında iletişimi olaylar üzerinden sağlamak, özellikle bağımsız sistemlerin birbirini tetiklemesi gereken durumlarda faydalıdır. Olay tabanlı iletişim, **asenkron** olarak işlediği için mikroservisler arasında gevşek bağlılık sağlar. Olay tabanlı sistemlerde kullanılan teknolojiler:

### a) **Event Sourcing ve CQRS (Command Query Responsibility Segregation)**

- **Event Sourcing**, her veri değişikliğini bir olay olarak kaydeder ve gelecekteki durum değişikliklerini bu olayları yeniden oynatarak oluşturur.
- **CQRS**, veri yazma ve okuma işlemlerinin ayrı sorumluluklar altında yürütüldüğü bir mimaridir.
- Kafka gibi mesajlaşma sistemleri ile kullanılabilir.

### b) **Redis Streams**

- **Redis Streams**, Redis üzerinde olay tabanlı mesajlaşmayı sağlar. Bu yöntemle mikroservisler arasında olaylar üzerinden iletişim sağlanabilir.
- **Artıları**: Redis'in hafızada çalışmasından dolayı yüksek performans sunar.
- **Eksileri**: Redis'in bellek tabanlı yapısı nedeniyle büyük veri kümeleri için uygun olmayabilir.

## Hangi Teknoloji Ne Zaman Kullanılmalı?

- **Senkron İletişim**: Mikroservisler arasında doğrudan yanıt gerektiren işlemler (örneğin, kullanıcıya anında yanıt vermek gereken işlemler) için idealdir. REST veya gRPC gibi çözümler burada uygundur.
- **Asenkron İletişim**: Mikroservislerin işlemlerinin birbirinden bağımsız olarak yürütüldüğü ve yanıt gecikmesinin sorun yaratmadığı işlemler için uygundur (örneğin, arka plan işlemleri, raporlamalar). RabbitMQ, Kafka, NATS gibi çözümler burada devreye girer.
- **Olay Tabanlı İletişim**: Sistem içinde bir olay tetiklendiğinde diğer mikroservislerin bu olaylara tepki vermesi gerektiği durumlarda kullanılır. Kafka, Redis Streams, AWS SNS gibi çözümler uygundur.

## Sonuç

Mikroservis mimarisinde iletişim, sistemin ihtiyaçlarına ve performans gereksinimlerine göre şekillenir. Eğer düşük gecikme süresi, basit iletişim ve hızlı geliştirme isteniyorsa, **HTTP/REST** veya **gRPC** tercih edilebilir. Ancak daha büyük, karmaşık ve yüksek hacimli mesajlaşmalar için **RabbitMQ**, **Kafka** gibi mesajlaşma sistemleri uygun olur.
