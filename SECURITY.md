# Security Policy

## Supported Versions

We provide security updates for the following versions of the Weather Forecast Dashboard:

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |
| < 1.0   | :x:                |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please report it responsibly.

### How to Report

1. **DO NOT** open a public GitHub issue for security vulnerabilities
2. **Email** the maintainers directly at: [security@weatherdashboard.com]
3. **Include** the following information:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

### What to Expect

- **Acknowledgment** within 48 hours
- **Initial assessment** within 5 business days
- **Regular updates** on progress
- **Credit** for responsible disclosure (if desired)

### Security Considerations

#### API Keys
- Never commit API keys to version control
- Use environment variables or secure configuration files
- Rotate keys regularly
- Monitor usage for unauthorized access

#### Data Privacy
- Weather data may include location information
- Ensure compliance with local privacy laws
- Implement proper data retention policies
- Use HTTPS for all data transmission

#### PowerBI Security
- Enable row-level security when appropriate
- Use Azure Active Directory for authentication
- Regularly review user access permissions
- Monitor dashboard usage logs

### Best Practices

1. **Keep PowerBI updated** to the latest version
2. **Use secure connections** for all API calls
3. **Implement proper error handling** to prevent information disclosure
4. **Regular security audits** of the dashboard and data sources
5. **Follow the principle of least privilege** for user access

---

*For non-security related issues, please use the standard GitHub issue reporting process.*