# Akademi CRM Config

Akademi CRM Lite mikroservislerinin Spring Cloud Config yapılandırma deposudur.

## Dosya düzeni

- `application.yml`: bütün servisler için ortak ayarlar.
- `<spring.application.name>.yml`: servise özel port, veri kaynağı, Springdoc ve readiness ayarları.

Config Server bu depoyu `main` branch'inden okur. Uygulamalar yapılandırmayı
`http://config-server:8888` üzerinden alır; domain servisleri bu GitHub deposuna doğrudan bağlanmaz.

Parola ve erişim bilgileri yalnız geliştirme varsayılanlarıdır. Gerçek ortam değerleri environment
variable veya secret manager üzerinden verilmelidir; production secret'ları bu depoya yazılmamalıdır.
