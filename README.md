# 📘 Darbas su sertifikatais | OpenSSL

Šis dokumentas pateikia kruopščiai atrinktą OpenSSL komandų sąrašą, skirtą TLS/SSL sertifikatų tikrinimui, generavimui ir patvirtinimui.
---

## 🔍 Sertifikato tikrinimas

### Parodyti serverio sertifikato SHA256 piršto atspaudą

```bash
openssl s_client -connect example.com:443 </dev/null 2>/dev/null | openssl x509 -noout -fingerprint -sha256
```

### Parodyti sertifikatą naudojant -showcert parinktį

```bash
openssl s_client -showcerts -connect example.com:443 -servername subdomain.example.com
```

### Show Certificate with `-showcert` Flag

```bash
openssl s_client -connect example.com:443 -servername example.com -showcert
```

---

## 🛠️ Rakto ir CSR generavimas

### Sugeneruoti 2048 bitų RSA privatų raktą

```bash
openssl genpkey -algorithm RSA -out private.key -pkeyopt rsa_keygen_bits:2048
```

### Sugeneruoti CSR (sertifikato pasirašymo užklausą)

```bash
openssl req -new -key private.key -out request.csr -config custom_config.cnf
```

### Peržiūrėti CSR turinį

```bash
openssl req -noout -text -in request.csr
```

---

## 📄 Sertifikato failo analizė

### Peržiūrėti sertifikatą tekstiniu formatu

```bash
openssl x509 -noout -text -in certificate.pem
```

### Patikrinti sertifikato galiojimo datas

```bash
openssl x509 -noout -dates -in certificate.pem
```

---

## 🌐 Nuotolinio sertifikato galiojimo tikrinimas

### Iš localhost (pvz., testavimo scenarijus)

```bash
openssl s_client -connect 127.0.0.1:443 -servername example.com | openssl x509 -noout -dates
```

### Iš viešo domeno

```bash
openssl s_client -connect example.com:443 -servername example.com | openssl x509 -noout -dates
```

### Su senesne TLS versija (pvz., TLS 1.1)

```bash
openssl s_client -connect 192.0.2.1:443 -tls1_1 -servername example.com
```

---

## 📡 Sertifikato gavimas iš serverio (naudojant IP arba vardą)

### Naudojant IP ir SNI

```bash
openssl s_client -connect 192.0.2.1:443 -servername example.com
```

### Išsaugoti nuotolinį sertifikatą į failą

```bash
openssl s_client -connect example.com:443 -showcerts </dev/null 2>/dev/null | openssl x509 -outform PEM > public_cert.pem
```

---

## 🔐 Kiti naudingi veiksmai

### Sugeneruoti RSA privatų raktą (be nurodyto dydžio)

```bash
openssl genpkey -algorithm RSA -out example-private-key.pem
```

### Sugeneruoti CSR iš rakto

```bash
openssl req -new -key example-private-key.pem -out example.csr -config csr_config.cnf
```

### Peržiūrėti sertifikato informaciją su serijos numeriu

```bash
openssl x509 -noout -dates -serial -in /config/ssl/ssl.crt/example.crt
```

---

## 📅 Sertifikato galiojimo tikrinimas pagal IP

```bash
openssl s_client -connect 203.0.113.25:443 -servername www.example.com 2>/dev/null | openssl x509 -noout -dates
```
