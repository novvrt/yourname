# Nuclei IDOR Detection Templates

Koleksi template Nuclei untuk mendeteksi vulnerability **IDOR (Insecure Direct Object Reference)** secara otomatis.

## 📁 Struktur Repository

```
nuclei-templates/
├── idor-detection.yaml          # Template IDOR detection utama
├── idor-enumeration.yaml        # Template untuk UUID/ID enumeration
└── README.md                      # Dokumentasi
```

## 🔍 Template yang Tersedia

### 1. idor-detection.yaml
Template comprehensive untuk mendeteksi IDOR vulnerabilities dengan 6 test cases:
- User Profile Endpoint Testing
- Order/Product Access Testing
- Document Download Testing
- Query Parameter Testing
- POST Request Body Testing
- File Management Testing

### 2. idor-enumeration.yaml
Template untuk mendeteksi IDOR melalui enumeration UUID dan numeric IDs.

## 🚀 Instalasi & Penggunaan

### Prerequisite
```bash
# Install Nuclei
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

### Quick Start
```bash
# Scan single target
nuclei -t nuclei-templates/idor-detection.yaml -u https://target.com

# Scan dengan authentication
nuclei -t nuclei-templates/idor-detection.yaml -u https://target.com \
  -H "Authorization: Bearer YOUR_TOKEN"

# Scan multiple targets
nuclei -t nuclei-templates/idor-detection.yaml -l targets.txt

# Dengan JSON output
nuclei -t nuclei-templates/idor-detection.yaml -u https://target.com -o results.json
```

## 🔧 Kustomisasi

Edit `idor-detection.yaml` untuk menyesuaikan:
- Endpoint paths sesuai target API
- Parameter names
- Headers (Authorization, etc)
- User ID dan Object ID ranges

Contoh:
```yaml
variables:
  user_id_1: "1"
  user_id_2: "2"
  product_id_1: "100"
```

## ⚠️ Disclaimer

Template ini hanya untuk **security research dan testing dengan izin**. Penggunaan pada sistem yang bukan milik Anda tanpa izin adalah ilegal.

## 📚 Referensi

- [OWASP IDOR](https://owasp.org/www-community/attacks/Insecure_Direct_Object_References)
- [Nuclei Documentation](https://docs.projectdiscovery.io/tools/nuclei/overview)
- [ProjectDiscovery](https://projectdiscovery.io/)

---

**Author:** Security Research Team  
**Last Updated:** 2026-05-23
