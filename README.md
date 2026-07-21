# Akademi CRM Config

Akademi CRM Lite mikroservislerinin Spring Cloud Config yapılandırma deposudur.

## Dizin düzeni

```text
common/
  application.yml
api-gateway/
  application.yml
  application-dev.yml
  application-test.yml
  application-prod.yml
customer-service/
  application.yml
  application-dev.yml
  application-test.yml
  application-prod.yml
... diğer servisler
```

Her servis klasöründe:

- `application.yml`: servisin default yapılandırması.
- `application-dev.yml`: yerel geliştirme profili; compose varsayılanıdır.
- `application-test.yml`: entegrasyon ve kabul testi profili.
- `application-prod.yml`: production profili; credential ve dış endpoint'leri environment/secret olarak zorunlu tutar.

Bütün servislere ait ortamdan bağımsız ortak değerler `common/application.yml` dosyasındadır.
Config Server `search-paths: [common, "{application}"]` ile ortak dosyayı yalnız istenen
servisin klasörüyle birleştirir. Uygulamalar yapılandırmayı `http://config-server:8888`
üzerinden alır; domain servisleri GitHub deposuna doğrudan bağlanmaz.

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
