# IDOR Template - Nuclei Templates untuk Deteksi IDOR

Repository ini berisi template Nuclei yang komprehensif untuk mendeteksi **IDOR (Insecure Direct Object Reference)** vulnerability pada aplikasi web dan API.

## 📋 Tentang IDOR

IDOR adalah jenis vulnerability yang terjadi ketika aplikasi menggunakan input yang dikendalikan user secara langsung sebagai referensi untuk mengakses objek tanpa melakukan validasi otorisasi yang tepat.

**Dampak IDOR:**
- Akses tidak sah ke data user lain
- Pembocoran informasi sensitif (personal data, invoice, dokumen)
- Privilege escalation
- Manipulasi data milik user lain

**Referensi OWASP:**
- [OWASP - Insecure Direct Object References](https://owasp.org/www-community/attacks/Insecure_Direct_Object_References)

## 🎯 Fitur Template

Repository ini menyediakan **4 template Nuclei utama**:

### 1. **idor-vulnerability-detection**
Template untuk mendeteksi IDOR vulnerabilities dengan 6 test cases:
- **User Profile Endpoint**: Testing akses ke endpoint `/api/users/{id}/profile`
- **Order/Product Access**: Testing akses endpoint `/api/orders/{id}`
- **Document Download**: Testing akses dokumen via path manipulation
- **Query Parameter IDOR**: Testing IDOR melalui query parameters
- **POST Request Body**: Testing IDOR di body request (invoice access)
- **File Management**: Testing IDOR di endpoint manajemen file

### 2. **idor-parameter-comparison**
Template untuk membandingkan response saat mengakses resource dengan user ID berbeda:
- Request ke `/api/users/1/details`
- Request ke `/api/users/2/details`
- Request ke `/api/users/999/details`
- Analisis response code dan content untuk mendeteksi perbedaan otorisasi

### 3. **idor-uuid-enumeration**
Template untuk mendeteksi IDOR vulnerability yang menggunakan UUID:
- Testing enumeration UUID secara sequential
- Mengidentifikasi pattern UUID yang dapat diprediksi
- Severity: Medium (UUID lebih sulit di-enumerate dibanding numeric ID)

### 4. **idor-response-comparison**
Template untuk membandingkan response ketika mengakses data user berbeda:
- Testing dengan menggunakan variable `user_id_1` dan `user_id_2`
- Ekstraksi informasi user (email, name, username)
- Deteksi jika sistem mengembalikan data user lain tanpa mengecek otorisasi

## 🚀 Cara Penggunaan

### Prerequisites
- [Nuclei](https://github.com/projectdiscovery/nuclei) - Vulnerability scanner
- Authorization token untuk API target (jika diperlukan)

### Instalasi Nuclei

```bash
# Menggunakan package manager
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest

# atau download binary dari: https://github.com/projectdiscovery/nuclei/releases
```

### Cara Menjalankan Template

#### 1. **Scan dengan template spesifik**
```bash
nuclei -t nuclei-templates/idor-detection.yaml -u https://target-api.com
```

#### 2. **Scan dengan custom variables**
```bash
nuclei -t nuclei-templates/idor-detection.yaml \
  -var user_id_1=10 \
  -var user_id_2=20 \
  -var product_id_1=100 \
  -var product_id_2=101 \
  -var auth_token="YOUR_AUTH_TOKEN" \
  -u https://target-api.com
```

#### 3. **Scan multiple targets dari file**
```bash
nuclei -t nuclei-templates/ -l targets.txt
```

#### 4. **Scan dengan JSON report output**
```bash
nuclei -t nuclei-templates/ -u https://target-api.com -json-export results.json
```

#### 5. **Scan dengan verbosity tinggi untuk debugging**
```bash
nuclei -t nuclei-templates/idor-detection.yaml -u https://target-api.com -v
```

## 📝 Template Structure Explanation

Setiap template YAML terdiri dari komponen:

### Header Informasi
```yaml
id: idor-vulnerability-detection          # Unique identifier
info:
  name: IDOR Detection                     # Nama template
  author: Security Research Team           # Penulis
  severity: high                           # Severity level
  description: |                           # Deskripsi detail
    ...
  reference:                               # Link referensi
    - https://owasp.org/...
  tags:                                    # Kategori dan tags
    - idor
    - access-control
```

### Variables
```yaml
variables:
  user_id_1: "1"                          # User ID pertama
  user_id_2: "2"                          # User ID berbeda untuk comparison
  product_id_1: "100"                     # Product/Object ID
```

### Requests
Mendefinisikan HTTP request yang akan dikirim:
```yaml
requests:
  - raw:
      - |
        GET /api/endpoint HTTP/1.1
        Host: {{Hostname}}
        Authorization: Bearer {{auth_token}}
```

### Matchers
Mendefinisikan kondisi untuk mendeteksi vulnerability:
```yaml
matchers:
  - type: status                           # Check HTTP status code
    status: [200, 201]
  
  - type: word                             # Check kata-kata spesifik
    part: body
    words: ["user", "email"]
    condition: and
  
  - type: regex                            # Menggunakan regex pattern
    part: body
    regex: ["\"id\":\"([0-9]+)\""]
```

### Extractors
Mengekstraksi data dari response:
```yaml
extractors:
  - type: regex
    part: body
    regex: ["\"email\":\"([^\"]+)\""]
    name: email_found
```

## 🔍 Cara Menganalisis Hasil

### Vulnerability Ditemukan
Jika Nuclei menemukan IDOR vulnerability, output akan menunjukkan:
```
[idor-vulnerability-detection] [http] [high] https://target.com/api/users/1/profile
 └─ Request: GET /api/users/1/profile
 └─ Response: 200 OK
 └─ Matched: "user", "email" found in body
```

### Interpretasi Hasil
- **Status 200 + Data Sensitif**: Kemungkinan besar ada IDOR
- **Status 403/404**: Aplikasi menolak akses (secure)
- **Sama untuk User ID berbeda**: Indikasi IDOR

## ⚙️ Kustomisasi Template

### Menambah Test Case Baru

Jika ingin menambah test case baru, edit file YAML:

```yaml
requests:
  - raw:
      - |
        GET /api/custom-endpoint/{{user_id}} HTTP/1.1
        Host: {{Hostname}}
        Authorization: Bearer {{auth_token}}
    
    matchers:
      - type: status
        status: [200]
      
      - type: word
        part: body
        words: ["sensitive_data"]
```

### Mengubah Authentication
Sesuaikan header authorization dengan jenis authentication target:

```yaml
# JWT Token
Authorization: Bearer {{auth_token}}

# API Key
X-API-Key: {{api_key}}

# Custom Header
X-Custom-Auth: {{custom_token}}
```

## 📊 Parameter yang Dapat Dikustomisasi

| Parameter | Default | Keterangan |
|-----------|---------|-----------|
| `user_id_1` | 1 | User ID untuk test pertama |
| `user_id_2` | 2 | User ID untuk test kedua (comparison) |
| `product_id_1` | 100 | Product/Object ID pertama |
| `product_id_2` | 101 | Product/Object ID kedua |
| `auth_token` | - | Bearer token untuk authentication |

## ⚠️ Disclaimer & Legal Notice

**PENTING**: Template ini hanya boleh digunakan untuk:
- ✅ Testing pada aplikasi Anda sendiri
- ✅ Testing dengan persetujuan tertulis dari pemilik sistem
- ✅ Tujuan pendidikan dan penelitian keamanan

**LARANGAN**:
- ❌ Testing pada sistem orang lain tanpa izin
- ❌ Akses tidak sah ke data atau sistem
- ❌ Penggunaan untuk tujuan ilegal atau berbahaya

Pengguna bertanggung jawab penuh atas penggunaan template ini. Penulis tidak bertanggung jawab atas kerusakan atau konsekuensi negatif dari penggunaan template.

## 📚 Referensi & Sumber Belajar

- [OWASP - IDOR](https://owasp.org/www-community/attacks/Insecure_Direct_Object_References)
- [Nuclei Documentation](https://nuclei.projectdiscovery.io/)
- [HackTricks - IDOR](https://book.hacktricks.xyz/pentesting-web/idor)
- [PortSwigger - IDOR](https://portswigger.net/burp/documentation/desktop/testing-workflow/access-control#testing-for-insecure-direct-object-references)

## 📝 Kontribusi

Silakan berkontribusi dengan:
- Menambah template baru untuk kasus IDOR yang berbeda
- Memperbaiki dan meningkatkan matcher logic
- Melaporkan false positive/negative
- Menambah dokumentasi

## 📄 License

[Sesuaikan dengan license yang Anda inginkan - contoh: MIT, Apache 2.0, dll]

## 👤 Author

**Security Research Team**
- Repository: [novvrt/idor-template](https://github.com/novvrt/idor-template)
- Untuk pertanyaan dan issue reporting, buat GitHub Issue di repository ini

---

**Last Updated**: 2026-05-23
**Nuclei Version**: v3+
