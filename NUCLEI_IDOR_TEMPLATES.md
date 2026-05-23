# 🔍 Nuclei IDOR Detection Templates

Komprehensif template collection untuk mendeteksi **IDOR (Insecure Direct Object Reference)** vulnerabilities menggunakan Nuclei.

## ✨ Fitur

✅ **6 Template Utama**
- IDOR Detection (6 test cases)
- IDOR Enumeration (UUID & Numeric IDs)
- IDOR Advanced Testing (Cookie, Header, Base64)
- IDOR POST Exploitation
- IDOR REST Methods (PUT, PATCH, DELETE)
- IDOR Batch Processing

✅ **Dokumentasi Lengkap**
- Installation guide
- Detailed usage guide
- Examples dan best practices
- CI/CD integration examples

✅ **Production Ready**
- Tested templates
- Error handling
- Multiple authentication methods
- Rate limiting support

## 🚀 Quick Start

### 1. Install Nuclei
```bash
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
```

### 2. Clone & Setup
```bash
git clone https://github.com/novvrt/yourname.git
cd yourname
git checkout nuclei-templates
```

### 3. Run Templates
```bash
# Scan single target
nuclei -t nuclei-templates/idor-detection.yaml -u https://api.example.com

# Scan with authentication
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Bearer YOUR_TOKEN"

# Scan multiple targets
nuclei -t nuclei-templates/ -l nuclei-templates/targets.txt -o results.json
```

## 📁 Repository Structure

```
Repository: novvrt/yourname (Branch: nuclei-templates)
│
├── nuclei-templates/
│   ├── idor-detection.yaml          ⭐ Main IDOR detection
│   ├── idor-enumeration.yaml        🔢 ID enumeration tests
│   ├── idor-advanced.yaml           🔐 Advanced techniques
│   ├── idor-post-requests.yaml      📤 POST exploitation
│   ├── config.yaml                  ⚙️  Configuration template
│   ├── targets.txt                  🎯 Target list template
│   ├── examples.sh                  📚 Usage examples
│   ├── README.md                    📖 Documentation
│   ├── INSTALLATION.md              🛠️  Installation guide
│   ├── USAGE.md                     📝 Detailed usage guide
│   ├── LICENSE                      📄 MIT License
│   └── .gitignore                   🚫 Git ignore rules
│
└── NUCLEI_IDOR_TEMPLATES.md         👈 You are here
```

## 🔍 Available Templates

### 1. **idor-detection.yaml**
Template utama dengan 4 test cases:
- User Profile Endpoint
- Order/Product Access
- Document Download
- Response Differentiation

**Usage:**
```bash
nuclei -t nuclei-templates/idor-detection.yaml -u https://api.example.com
```

### 2. **idor-enumeration.yaml**
6 template untuk UUID dan numeric ID enumeration:
- Numeric ID Enumeration
- Order ID Enumeration
- Resource ID Enumeration
- Document ID Enumeration
- API Key Enumeration
- Profile Enumeration

**Usage:**
```bash
nuclei -t nuclei-templates/idor-enumeration.yaml -u https://api.example.com
```

### 3. **idor-advanced.yaml**
Advanced IDOR testing techniques:
- Cookie-based IDOR
- Header-based manipulation
- Base64 encoded IDs
- JSON Pointer exploitation
- Multi-level IDOR
- Batch processing

**Usage:**
```bash
nuclei -t nuclei-templates/idor-advanced.yaml -u https://api.example.com
```

### 4. **idor-post-requests.yaml**
POST request exploitation:
- JSON POST with ID manipulation
- Form-data IDOR
- XML-based IDOR
- Nested JSON IDOR
- Array-based IDOR
- Different HTTP methods

**Usage:**
```bash
nuclei -t nuclei-templates/idor-post-requests.yaml -u https://api.example.com
```

## 💡 Usage Examples

### Basic Scan
```bash
nuclei -t nuclei-templates/idor-detection.yaml -u https://api.example.com
```

### With Authentication
```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### Multiple Targets
```bash
nuclei -t nuclei-templates/ -l nuclei-templates/targets.txt
```

### JSON Report
```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o results.json -j
```

### HTML Report
```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o report.html -irr
```

### With Rate Limiting
```bash
nuclei -t nuclei-templates/ -u https://api.example.com -rl 10
```

### With Proxy
```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -proxy http://127.0.0.1:8080
```

## 🔧 Configuration

### Edit targets.txt
```bash
vi nuclei-templates/targets.txt
```

Add your target URLs:
```
https://api.example.com
https://api.example.com/v1
https://api.example.com/v2
```

### Edit config.yaml
```bash
vi nuclei-templates/config.yaml
```

Update headers and settings:
```yaml
headers:
  Authorization: "Bearer YOUR_TOKEN"
  X-API-Key: "YOUR_API_KEY"
```

## 📊 Output Formats

- **JSON**: `-o results.json -j`
- **HTML**: `-o report.html -irr`
- **SARIF**: `-o report.sarif`
- **Markdown**: `-o report.md`
- **Console**: Default output

## 🔐 Security Disclaimer

⚠️ **LEGAL WARNING**

These templates are designed for **authorized security testing only**. Usage without explicit permission is illegal and unethical. Always:

1. ✅ Get written permission from system owner
2. ✅ Test only on systems you own or have permission to test
3. ✅ Follow responsible disclosure practices
4. ✅ Comply with all applicable laws
5. ✅ Document your findings properly

## 📚 Documentation

- **[README.md](nuclei-templates/README.md)** - Overview and features
- **[INSTALLATION.md](nuclei-templates/INSTALLATION.md)** - Setup guide
- **[USAGE.md](nuclei-templates/USAGE.md)** - Detailed usage
- **[examples.sh](nuclei-templates/examples.sh)** - Code examples

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Test your changes
4. Submit a pull request

## 📞 Support

- 📖 Check [Nuclei Documentation](https://docs.projectdiscovery.io/tools/nuclei/)
- 🐛 Create issues for bugs
- 💡 Share ideas via discussions
- 📧 Contact: novvrt@example.com

## 📄 License

MIT License - See [LICENSE](nuclei-templates/LICENSE) for details

## 🙏 Credits

- **Nuclei** by [ProjectDiscovery](https://projectdiscovery.io/)
- **OWASP** for vulnerability definitions
- **Security Research Team**

---

**Last Updated:** 2026-05-23  
**Repository:** [novvrt/yourname](https://github.com/novvrt/yourname)  
**Branch:** nuclei-templates
