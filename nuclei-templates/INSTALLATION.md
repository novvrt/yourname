# Nuclei IDOR Templates - Installation Guide

## Prerequisites

Before installing and using these templates, ensure you have:

- **Go 1.18+** installed
- **Git** for cloning repositories
- **Nuclei v3.0+** for running templates

## Installation Steps

### 1. Install Nuclei

```bash
# Using Go
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest

# Verify installation
nuclei -version
```

### 2. Clone This Repository

```bash
cd /path/to/your/projects
git clone https://github.com/novvrt/yourname.git
cd yourname
```

### 3. Switch to nuclei-templates Branch

```bash
git checkout nuclei-templates
```

### 4. Verify Template Structure

```bash
ls -la nuclei-templates/

# Expected output:
# ├── idor-detection.yaml
# ├── idor-enumeration.yaml
# ├── idor-advanced.yaml
# ├── idor-post-requests.yaml
# ├── config.yaml
# ├── targets.txt
# ├── examples.sh
# ├── README.md
# ├── INSTALLATION.md
# └── USAGE.md
```

## Configuration

### Update Target Hosts

Edit `nuclei-templates/targets.txt` and add your target URLs:

```bash
vi nuclei-templates/targets.txt
```

### Update Configuration

Edit `nuclei-templates/config.yaml` with your settings:

```yaml
headers:
  Authorization: "Bearer YOUR_AUTH_TOKEN"
  X-API-Key: "YOUR_API_KEY"
```

## Quick Start

```bash
# Test single template on single target
nuclei -t nuclei-templates/idor-detection.yaml -u https://api.example.com

# Test all IDOR templates
nuclei -t nuclei-templates/ -l nuclei-templates/targets.txt

# Generate HTML report
nuclei -t nuclei-templates/ -l nuclei-templates/targets.txt -o report.html -irr
```

## Troubleshooting

### Issue: Templates not found

```bash
# Make sure you're in the correct directory
pwd
ls nuclei-templates/*.yaml
```

### Issue: Authentication errors

```bash
# Update your auth token
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Bearer YOUR_VALID_TOKEN"
```

### Issue: Rate limiting

```bash
# Use rate limiter flag
nuclei -t nuclei-templates/ -u https://api.example.com -rl 5
```

## Next Steps

1. Read [USAGE.md](USAGE.md) for detailed usage examples
2. Customize templates in [README.md](README.md)
3. Review [examples.sh](examples.sh) for more scenarios

## Support

For issues or questions:
- Check [Nuclei Documentation](https://docs.projectdiscovery.io/tools/nuclei/)
- Review template syntax in [idor-detection.yaml](idor-detection.yaml)
- Create an issue on GitHub
