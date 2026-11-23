# SeniorMulakatRehberi
1. İki kullanıcı aynı hesaptan eşzamanlı para çekmek isterse nasıl engellersin?

Bu problem Race Condition olarak adlandırılır. İki kullanıcının aynı bakiyeyi okuyup eşzamanlı işlem yapması veri tutarsızlığına neden olabilir. Bunu önlemek için Serializable Transaction, Row-Level Locking, UPDLOCK ve Atomic Conditional Update kullanabilirim. Örneğin SQL tarafında UPDATE Accounts SET Balance = Balance - @Amount WHERE Id = @Id AND Balance >= @Amount sorgusunu çalıştırıp etkilenen satır sayısını kontrol ederim. Böylece bakiye negatif değere düşmez.

2. RabbitMQ mesajı iki kez gelirse ne yaparsın?

RabbitMQ At-Least-Once teslimat yaklaşımı nedeniyle aynı mesajı birden fazla kez iletebilir. Bu problemi Idempotency ile çözerim. Her mesajın benzersiz bir MessageId değeri olur. İşlenen mesajları veritabanında tutarım. Unique Constraint ve Transaction kullanarak aynı mesajın ikinci kez işlenmesini engellerim. Böylece ödeme gibi kritik işlemlerin tekrarlanmasını önlerim.

3. Outbox Pattern neden gereklidir?

Outbox Pattern, veritabanı işlemleri ile mesajlaşma sistemi arasındaki tutarlılığı sağlar. Örneğin sipariş veritabanına kaydedilir ancak RabbitMQ erişilemezse mesaj kaybolabilir. Bunun önüne geçmek için sipariş ve Outbox mesajını aynı veritabanı transaction'ında kaydederim. Background Worker veya MassTransit Outbox mekanizması mesajı daha sonra RabbitMQ'ya gönderir. Böylece event kaybı riski azaltılır.

4. SQL Server'da Deadlock nasıl analiz edilir?

Deadlock, iki veya daha fazla transaction'ın birbirlerinin tuttuğu kaynakları beklemesi durumudur. Analiz için SQL Server Extended Events, Deadlock Graph, Query Store ve Execution Plan kullanırım. Çözüm olarak transaction sürelerini kısaltır, tablolara aynı sırada erişir, uygun indeksler oluşturur ve gereksiz kilitlenmeleri önlerim. Gerekli durumlarda kontrollü Retry mekanizması uygularım.

5. Büyük veri setlerinde Pagination ve Indexing nasıl tasarlanır?

Küçük ve orta ölçekli veri setlerinde Offset Pagination kullanabilirim. Ancak milyonlarca kayıt içeren tablolarda derin sayfalama maliyetli olabileceğinden Keyset (Cursor) Pagination tercih ederim. Filtreleme ve sıralama kriterlerine uygun Composite Index oluştururum. EF Core tarafında AsNoTracking kullanır, gereksiz kolonları çekmez, N+1 problemini önler ve Execution Plan üzerinden sorgu performansını incelerim.

6. React veya Angular tarafında State Management nasıl yaparsın?

State yönetimini verinin kullanım alanına göre belirlerim. Component seviyesinde local state, uygulama genelinde global state, API verilerinde server state kullanırım. React tarafında Redux Toolkit ve TanStack Query; Angular tarafında Reactive Forms, RxJS, Signals ve gerektiğinde NgRx tercih ederim. Form validasyonu için React Hook Form veya Angular Reactive Forms kullanabilirim. API entegrasyonlarında loading, error ve success durumlarını ayrı yönetirim.

7. Bir servisin performansını nasıl ölçer ve iyileştirirsin?

Öncelikle performansı ölçmeden optimizasyon yapmam. k6 ile yük ve stres testleri gerçekleştiririm. Response Time, Throughput, Error Rate ve p95/p99 latency değerlerini incelerim. OpenTelemetry ile distributed tracing, Prometheus ile metrik takibi yaparım. SQL sorgularını ve Execution Plan'ları analiz ederim. Redis caching, asenkron işlemler, pagination, uygun indeksleme ve connection pooling gibi yöntemlerle performansı iyileştiririm. Sonrasında tekrar yük testi yaparak iyileştirmenin sonuçlarını karşılaştırırım.
