# Security Policy

## Supported Versions

Currently, all versions of Habibit are considered **development/educational** releases. Security updates are provided on a best-effort basis for the latest version only.

| Version | Supported | Status |
|---------|-----------|--------|
| Latest (main branch) | ✅ | Active development |
| Older commits | ❌ | Not supported |

⚠️ **Important**: The C++ and JavaScript implementations are **not production-ready** and should not be used in production environments without significant security enhancements.

---

## Known Security Limitations

### C++ CLI Application

**Critical Security Issues:**

1. **Plain Text Password Storage**
   - **Issue**: Passwords are stored in memory as plain text strings
   - **Location**: `mainFile.cpp` - account creation and sign-in functions
   - **Impact**: HIGH - Credentials can be easily extracted from memory dumps
   - **Recommendation**: Do not use for any real accounts

2. **No Input Validation**
   - **Issue**: User input is not sanitized
   - **Impact**: MEDIUM - Could lead to buffer overflows or injection attacks
   - **Location**: Throughout `mainFile.cpp`

3. **No Authentication Token/Session Management**
   - **Issue**: Simple username:password string comparison
   - **Impact**: MEDIUM - Easy to bypass with debugging tools
   - **Location**: `signIn()` function

**Recommended for**: Educational purposes only

### JavaScript Browser Implementation

**Critical Security Issues:**

1. **Plain Text Password Storage**
   - **Issue**: Same as C++ - credentials stored in JavaScript arrays
   - **Location**: `Habibit.js` - accounts array
   - **Impact**: HIGH - Visible in browser memory/DevTools
   - **Recommendation**: Educational use only

2. **Client-Side Only Authentication**
   - **Issue**: No server-side validation
   - **Impact**: HIGH - All data can be manipulated via console
   - **Location**: Entire `Habibit.js` file

3. **No XSS Protection**
   - **Issue**: User input displayed without sanitization
   - **Impact**: MEDIUM - Potential for cross-site scripting
   - **Location**: `displayEquipment()` and similar functions

**Recommended for**: Educational demonstrations only

### WordPress Platform

**Security Considerations:**

1. **Default Credentials**
   - **Issue**: Development database uses `root:root`
   - **Location**: `habifarm/app/public/wp-config.php`
   - **Impact**: CRITICAL if deployed publicly
   - **Mitigation**: Change before any public deployment

2. **Debug Mode**
   - **Issue**: May have debug logging enabled
   - **Impact**: MEDIUM - Information disclosure
   - **Mitigation**: Disable `WP_DEBUG` in production

3. **Plugin Vulnerabilities**
   - **Issue**: Third-party plugins may have known CVEs
   - **Impact**: VARIES
   - **Mitigation**: Keep all plugins updated

4. **Missing Security Keys**
   - **Issue**: `wp-config.php` may lack unique security salts
   - **Location**: `wp-config.php` authentication keys section
   - **Impact**: MEDIUM - Weak session security
   - **Mitigation**: Generate from https://api.wordpress.org/secret-key/1.1/salt/

**Better Security Posture**: WordPress implementation has more security features (password hashing, role-based access control, SQL injection protection via prepared statements)

---

## Reporting a Vulnerability

### ⚠️ Please DO NOT Report Security Vulnerabilities Publicly

If you discover a security vulnerability, please report it privately:

### Reporting Process

1. **Email**: Send details to the repository maintainer
   - **GitHub**: Create a private security advisory via the repository's Security tab
   - **Alternative**: Open a private issue (mark as security)

2. **Include in Your Report**:
   - Description of the vulnerability
   - Affected platform(s) (C++, JavaScript, WordPress)
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if available)
   - Your contact information (for follow-up)

3. **Response Timeline**:
   - **Acknowledgment**: Within 48-72 hours
   - **Initial Assessment**: Within 1 week
   - **Fix Timeline**: Depends on severity
     - Critical: 1-2 weeks
     - High: 2-4 weeks
     - Medium: 4-8 weeks
     - Low: Best effort

4. **Disclosure**:
   - We follow coordinated disclosure practices
   - Please allow 90 days before public disclosure
   - We will credit you in the security advisory (if desired)

### What to Expect

**After Reporting:**
1. We will confirm receipt of your report
2. We will investigate and reproduce the issue
3. We will develop and test a fix
4. We will release a security update
5. We will publish a security advisory (crediting you if desired)

**Bug Bounty**: This is an open-source educational project with no funding. We cannot offer monetary rewards but will:
- Credit you in the security advisory and CHANGELOG
- List you as a security contributor
- Provide a recommendation letter if requested

---

## Security Best Practices

### For C++ Implementation

**DO:**
- Use only for learning and educational purposes
- Assume all data can be inspected/modified
- Never store real passwords or sensitive data

**DON'T:**
- Deploy to production
- Use real user credentials
- Store sensitive equipment data

**If You Must Use It:**
```cpp
// Add password hashing (example with OpenSSL)
#include <openssl/sha.h>

string hashPassword(const string& password) {
    unsigned char hash[SHA256_DIGEST_LENGTH];
    SHA256_CTX sha256;
    SHA256_Init(&sha256);
    SHA256_Update(&sha256, password.c_str(), password.length());
    SHA256_Final(hash, &sha256);
    
    // Convert to hex string
    stringstream ss;
    for(int i = 0; i < SHA256_DIGEST_LENGTH; i++) {
        ss << hex << setw(2) << setfill('0') << (int)hash[i];
    }
    return ss.str();
}
```

### For JavaScript Implementation

**DO:**
- Use only in controlled environments (demos, local testing)
- Validate all user input
- Use Content Security Policy (CSP) headers if hosting

**DON'T:**
- Deploy to public web servers
- Store any sensitive data
- Use for actual equipment rental

**If You Must Deploy:**
- Implement server-side authentication
- Use HTTPS only
- Add CSRF protection
- Sanitize all inputs/outputs

### For WordPress Platform

**DO:**
1. **Change Default Credentials**:
```php
// wp-config.php
define('DB_NAME', 'habibit_production');
define('DB_USER', 'habibit_user');  // Not 'root'
define('DB_PASSWORD', 'STRONG_RANDOM_PASSWORD');
define('DB_HOST', 'localhost');
```

2. **Generate Unique Security Keys**:
```bash
# Get fresh keys from WordPress
curl -s https://api.wordpress.org/secret-key/1.1/salt/

# Paste into wp-config.php
```

3. **Disable Debug in Production**:
```php
define('WP_DEBUG', false);
define('WP_DEBUG_LOG', false);
define('WP_DEBUG_DISPLAY', false);
```

4. **File Permissions**:
```bash
# Set proper permissions
find habifarm/app/public -type d -exec chmod 755 {} \;
find habifarm/app/public -type f -exec chmod 644 {} \;
chmod 600 habifarm/app/public/wp-config.php
```

5. **Keep Updated**:
```bash
# Update WordPress core, plugins, themes regularly
# Dashboard → Updates
```

6. **Use Security Plugins**:
- [Wordfence Security](https://wordpress.org/plugins/wordfence/)
- [iThemes Security](https://wordpress.org/plugins/better-wp-security/)
- [Sucuri Security](https://wordpress.org/plugins/sucuri-scanner/)

7. **SSL/TLS**:
```php
// Force HTTPS
define('FORCE_SSL_ADMIN', true);
if (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https') {
    $_SERVER['HTTPS'] = 'on';
}
```

**DON'T:**
- Use default admin username ('admin')
- Allow file editing from dashboard
- Use outdated PHP/MySQL versions
- Ignore security updates

---

## Security Checklist for Deployment

### Before Deploying WordPress Platform to Production

- [ ] Changed all default passwords
- [ ] Generated unique WordPress security keys
- [ ] Disabled WP_DEBUG
- [ ] Set proper file permissions (644 for files, 755 for directories)
- [ ] Removed default 'admin' user
- [ ] Installed SSL certificate (HTTPS)
- [ ] Enabled FORCE_SSL_ADMIN
- [ ] Updated all plugins and themes
- [ ] Installed security plugin (Wordfence/iThemes)
- [ ] Configured firewall rules
- [ ] Set up regular backups
- [ ] Limited login attempts
- [ ] Changed database table prefix from 'wp_'
- [ ] Disabled XML-RPC if not needed
- [ ] Reviewed user roles and permissions
- [ ] Configured WooCommerce security settings
- [ ] Tested payment gateway integration in sandbox mode
- [ ] Set up security monitoring/logging

### Never Deploy to Production

- [ ] C++ CLI application (educational only)
- [ ] JavaScript browser implementation (educational only)

---

## Security Headers (WordPress/Nginx)

Add to Nginx configuration:

```nginx
# habifarm/conf/nginx/site.conf
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';" always;
```

---

## Incident Response

If a security incident occurs:

1. **Immediate Actions**:
   - Take affected systems offline if critical
   - Preserve logs and evidence
   - Notify maintainers immediately

2. **Investigation**:
   - Determine scope and impact
   - Identify entry point
   - Document timeline

3. **Remediation**:
   - Apply security patches
   - Change all credentials
   - Review access logs

4. **Recovery**:
   - Restore from clean backups if needed
   - Verify system integrity
   - Monitor for re-occurrence

5. **Post-Incident**:
   - Document lessons learned
   - Update security practices
   - Publish security advisory

---

## Resources

**WordPress Security:**
- [WordPress Security Codex](https://wordpress.org/support/article/hardening-wordpress/)
- [WooCommerce Security Best Practices](https://woocommerce.com/document/security-best-practices/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

**General Security:**
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)

---

## Acknowledgments

We thank the security community for their responsible disclosure and contributions to improving Habibit's security.

**Security Contributors:**
- (None yet - be the first!)

---

**Last Updated**: 2026-02-05  
**Policy Version**: 1.0
