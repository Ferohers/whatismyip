# 🧐 Whatismyip (WhatsMyIP Klonu)

Bu proje, **dcarrillo** tarafından geliştirilen [Whatismyip](https://github.com/dcarrillo/whatismyip) projesine dayanır. Ancak orijinal sürüm Türkiye'den erişime kapalı ve Docker Compose desteği yoktu. Bu versiyon bu eksikleri gideriyor.

En güçlü özelliği, kullandığınız **DNS ** 'i öğrenme imkanı sunmasıdır. 

> **Not:** `curl` ile daha kısa komutlar kullanabilmek adına genellikle 80. port tercih edilmiştir.

---

## 🚀 Hızlı Başlangıç

Docker imajını çalıştırarak anında kullanmaya başlayabilirsiniz:

```bash
curl alanadiniz.com
```

IP adresinizi size döner. Http üzerinden de detaylı bilgilere ulaşabilirsiniz.

---

## 🌐 IP ve dns adresinizi öğrenme

DNS sunucu bilgilerinize ulaşmak için bazı adımlar var ve bunlar tam olmazsa sorgu doğru dönmez:

### 📝 1. DNS Kayıtlarını Oluşturma

Aşağıdaki kayıtları alan adınızın DNS yönetim panelinden ekleyin:

- **NS Kaydı:**
  - `dns.alanadiniz.com` → `xns.alanadiniz.com`
- **A Kayıtları:**
  - `xns.alanadiniz.com` → Sunucunun IP adresi (örn: `63.11.62.1`)
  - `dns.alanadiniz.com` → Aynı IP (gerekirse)

Not: Bunu yapmamızın sebebi dns.alanadıxxx.com un dns kaydını kendi serverımıza alma isteğimiz böylece sorgu bize geldiğinden yapanı bulma şansımız oluyor.

### ⚙️ 2. Resolver Yapılandırması

`data/` klasörü altında `resolver.yaml` dosyasını oluşturun:

```yaml
domain: dns.alanadiniz.com
redirect_port: ":80"
resource_records:
  - "1800 IN SOA xns.alanadiniz.com. hostmaster.alanadiniz.com. 1 10000 2400 604800 1800"
  - "3600 IN NS xns.alanadiniz.com."
ipv4:
  - "63.11.62.1" 
```

> Alternatif olarak yolunu `docker-compose.yml` içinde belirtebilirsiniz. 63.11.62.1 kendi dış ip adresim. siz kendinizinkini koyacaksınız :)

### 🌍 3. GeoIP Desteği (İsteğe Bağlı)

IP adreslerine ülke bilgisi eklemek için:

1. [MaxMind](https://www.maxmind.com/en/home) üzerinden hesap oluşturun.
2. GeoLite verilerini indirip `data/` klasörüne yerleştirin.

### 🧪 4. Test Etme

Kurulum tamamlandıysa aşağıdaki komut ile test edebilirsiniz:

```bash
curl benimdomain.com (bu ip adresinizi döner)
curl -L dns.benimdomain.com (bu dns'i döner)
```

Bu komut, IP adresinizi DNS üzerinden döndürecektir.

---

## 💡 Ek Bilgiler

- HTTP/2 ve HTTP/3 desteği mevcuttur.
- `certbot` ile oluşturduğunuz SSL sertifikaları desteklenir.
- Uygulama varsayılan olarak 80. portta çalışır. Eğer bu port doluysa, 81 gibi başka bir porta alıp **reverse proxy** kullanarak dışarıya yönlendirme yapabilirsiniz. 
- Farklı portta sorgunuzda değiştirir "curl benimdomain.com:81"

---

Artık sisteminiz hem HTTP hem de DNS üzerinden IP sorgularına yanıt verebilir. 🎉
