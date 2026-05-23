# Nuclei IDOR Templates - Detailed Usage Guide

## Table of Contents

1. [Basic Usage](#basic-usage)
2. [Authentication](#authentication)
3. [Output Formats](#output-formats)
4. [Advanced Scanning](#advanced-scanning)
5. [Integration](#integration)
6. [Troubleshooting](#troubleshooting)

## Basic Usage

### Scan Single Target

```bash
nuclei -t nuclei-templates/idor-detection.yaml -u https://api.example.com
```

### Scan Multiple Targets

```bash
nuclei -t nuclei-templates/ -l nuclei-templates/targets.txt
```

### Scan Specific Template

```bash
nuclei -t nuclei-templates/idor-enumeration.yaml -u https://api.example.com
```

## Authentication

### Bearer Token

```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### API Key

```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "X-API-Key: YOUR_API_KEY"
```

### Basic Auth

```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Basic $(echo -n 'user:pass' | base64)"
```

### Custom Headers

```bash
nuclei -t nuclei-templates/ -u https://api.example.com \
  -H "Authorization: Bearer TOKEN" \
  -H "X-Custom-Header: value" \
  -H "X-API-Version: v2"
```

## Output Formats

### JSON Output

```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o results.json -j
```

### HTML Report

```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o report.html -irr
```

### SARIF Format

```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o report.sarif -irr
```

### Markdown Report

```bash
nuclei -t nuclei-templates/ -u https://api.example.com -o report.md
```

## Advanced Scanning

### Rate Limiting

```bash
# 10 requests per second
nuclei -t nuclei-templates/ -u https://api.example.com -rl 10
```

### Timeout Configuration

```bash
# 30 second timeout
nuclei -t nuclei-templates/ -u https://api.example.com -timeout 30
```

### Proxy Configuration

```bash
# HTTP Proxy
nuclei -t nuclei-templates/ -u https://api.example.com \
  -proxy http://127.0.0.1:8080

# SOCKS5 Proxy
nuclei -t nuclei-templates/ -u https://api.example.com \
  -proxy socks5://127.0.0.1:1080
```

### SSL Certificate Handling

```bash
# Disable SSL verification (not recommended for production)
nuclei -t nuclei-templates/ -u https://api.example.com -insecure
```

### Verbose Output

```bash
# Detailed logging
nuclei -t nuclei-templates/ -u https://api.example.com -v

# Very verbose
nuclei -t nuclei-templates/ -u https://api.example.com -vv
```

### Silent Mode

```bash
nuclei -t nuclei-templates/ -u https://api.example.com -silent
```

## Integration

### CI/CD Pipeline (GitHub Actions)

```yaml
name: IDOR Security Scan

on: [push]

jobs:
  nuclei-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Run Nuclei IDOR Scan
        run: |
          go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
          nuclei -t nuclei-templates/ -u ${{ env.TARGET_URL }} \
            -H "Authorization: Bearer ${{ secrets.API_TOKEN }}" \
            -o results.json -j
      
      - name: Upload Results
        uses: actions/upload-artifact@v2
        with:
          name: nuclei-results
          path: results.json
```

### Jenkins Pipeline

```groovy
pipeline {
    agent any
    
    stages {
        stage('Install Nuclei') {
            steps {
                sh 'go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest'
            }
        }
        
        stage('Run IDOR Scan') {
            steps {
                sh '''
                    nuclei -t nuclei-templates/ -u ${TARGET_URL} \
                      -H "Authorization: Bearer ${API_TOKEN}" \
                      -o results.json -j
                '''
            }
        }
        
        stage('Archive Results') {
            steps {
                archiveArtifacts artifacts: 'results.json'
            }
        }
    }
}
```

### GitLab CI

```yaml
idor-scan:
  image: golang:1.20
  script:
    - go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
    - nuclei -t nuclei-templates/ -u $TARGET_URL \
        -H "Authorization: Bearer $API_TOKEN" \
        -o results.json -j
  artifacts:
    paths:
      - results.json
```

## Troubleshooting

### Template Validation

```bash
# Validate all templates
nuclei -validate nuclei-templates/

# Validate specific template
nuclei -validate nuclei-templates/idor-detection.yaml
```

### Debug Mode

```bash
# Debug information
nuclei -t nuclei-templates/ -u https://api.example.com -debug
```

### Common Issues

#### Issue: Connection Timeout
```bash
# Increase timeout
nuclei -t nuclei-templates/ -u https://api.example.com -timeout 60
```

#### Issue: Authentication Fails
```bash
# Verify token with verbose output
nuclei -t nuclei-templates/ -u https://api.example.com -v
```

#### Issue: Rate Limited
```bash
# Reduce request rate
nuclei -t nuclei-templates/ -u https://api.example.com -rl 2
```

#### Issue: SSL Certificate Error
```bash
# Disable SSL verification (use with caution)
nuclei -t nuclei-templates/ -u https://api.example.com -insecure
```

## Best Practices

1. **Always get permission** before scanning
2. **Use rate limiting** to avoid overwhelming the target
3. **Run during maintenance windows** if possible
4. **Store results securely** with proper access controls
5. **Update templates regularly** for new attack vectors
6. **Test in staging first** before production scans
7. **Use proxies** for additional logging and monitoring
8. **Validate targets** before bulk scanning

## Performance Tips

- Use `-timeout 5` for faster scanning
- Use `-rl 50` for parallel requests
- Use `-proxy` to distribute load
- Scan during off-peak hours
- Use `-silent` mode to reduce overhead

## Support

For additional help:
- Check [Nuclei Docs](https://docs.projectdiscovery.io/tools/nuclei/)
- Review [examples.sh](examples.sh)
- Check template comments in YAML files
