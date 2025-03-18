### Genel Açıklama

Bu ödevde, event tabanlı mimariyi kullanarak üç adet mikroservis geliştirmeniz beklenmektedir. Mikroservisler sırasıyla:

1. *Order Microservice*
2. *Stock Microservice*
3. *Payment Microservice*

olarak tanımlanmıştır.

RabbitMQ mesaj kuyruğu ve MassTransit kütüphanesi kullanılarak mikroservislerin haberleşmesi sağlanacaktır.

### Mikroservislerin Görevleri

### 1. Order Microservice

- Sipariş oluşturulduğunda, siparişi ilk olarak durumunu "Suspend" (Askıda) olacak şekilde veritabanına kaydedin.
- Sipariş başarıyla kaydedildikten sonra, diğer mikroservislere bildirmek amacıyla "OrderCreatedEvent" isimli bir event oluşturup kuyruğa gönderin.
- Payment Microservice'den gelen "PaymentCompletedEvent" eventini dinleyerek sipariş durumunu "Completed" (Tamamlandı) olarak güncelleyin.

### 2. Stock Microservice

- "OrderCreatedEvent" eventini dinleyerek siparişte istenen ürünlerin stok durumlarını kontrol edin.
- Eğer stok mevcutsa:
    - Stok miktarını güncelleyip ilgili kaydı veritabanına kaydedin.
    - Sonrasında ödeme sürecinin başlatılması amacıyla "StockReservedEvent" eventini yayınlayın.
- Eğer stok yeterli değilse:
    - "StockNotAvailableEvent" eventini yayınlayarak siparişin başarısız olduğunu Order Microservice'e bildirin.

### 3. Payment Microservice

- "StockReservedEvent" eventini dinleyerek ödeme işlemlerini gerçekleştirin.
- Ödeme başarılı gerçekleşirse:
    - "PaymentCompletedEvent" eventini göndererek Order Microservice'e siparişin başarılı tamamlandığını bildirin.
- Ödeme başarısız olursa:
    - "PaymentFailedEvent" eventini göndererek siparişin başarısız olduğunu Order Microservice'e bildirin.
