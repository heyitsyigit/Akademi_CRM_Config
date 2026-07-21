# Akademi CRM Config

Akademi CRM Lite mikroservislerinin Spring Cloud Config yapılandırma deposudur.

## Dosya düzeni

- `application.yml`: bütün servisler için ortak ve ortamdan bağımsız ayarlar.
- `application-dev.yml`: yerel geliştirme katmanı; compose varsayılanıdır.
- `application-test.yml`: entegrasyon ve kabul testi katmanı.
- `application-prod.yml`: production katmanı; credential ve dış endpoint'leri environment/secret olarak zorunlu tutar.
- `<spring.application.name>.yml`: servise özel port, veri kaynağı, Springdoc ve readiness ayarları.

Config Server bu depoyu `main` branch'inden okur. Uygulamalar yapılandırmayı
`http://config-server:8888` üzerinden alır; domain servisleri bu GitHub deposuna doğrudan bağlanmaz.

Ana compose dosyası `APP_PROFILE` değerini uygulamalara `SPRING_PROFILES_ACTIVE` olarak geçirir.
Değer verilmezse `dev` kullanılır; test ve production için sırasıyla `test` veya `prod` seçilir.

## Production değişkenleri

`prod` profili aşağıdaki değerleri environment variable veya secret manager üzerinden bekler:

- `DATABASE_URL`, `DB_USER`, `DB_PASSWORD`
- `REDIS_HOST`, `REDIS_PASSWORD` (`REDIS_PORT` varsayılanı `6379`)
- `KAFKA_BOOTSTRAP_SERVERS`, `EUREKA_SERVICE_URL`
- `KEYCLOAK_ISSUER_URI`, `KEYCLOAK_JWKSET_URI`, `KEYCLOAK_ACCOUNT_URI`
- `SWAGGER_OAUTH2_AUTHORIZATION_URL`, `SWAGGER_OAUTH2_TOKEN_URL`

Production secret'ları bu depoya yazılmamalıdır.
