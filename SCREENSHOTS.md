# Visual Guide: SSL Certificate Deployment Screenshots

This document provides step-by-step visual guidance for deploying certificates in IIS and Apache, with links to official documentation and external screenshot resources.

---

## 📚 Official Documentation References

### Microsoft IIS Official Documentation

**Core SSL Configuration Guides:**

- **[How to Set Up SSL on IIS 7 or later](https://learn.microsoft.com/en-us/iis/manage/configuring-security/how-to-set-up-ssl-on-iis/)** ⭐ Microsoft Learn
  - Covers obtaining certificates, creating HTTPS bindings, and troubleshooting
  - Step-by-step instructions for all IIS versions
  
- **[Configuring SSL in IIS Manager](https://learn.microsoft.com/en-us/iis/manage/configuring-security/configuring-ssl-in-iis-manager/)** ⭐ Microsoft Learn
  - Visual tutorial for self-signed certificates, CA certificates, and SSL binding setup
  - Screenshots included for major steps

- **[Install Imported Certificates on Windows Server](https://learn.microsoft.com/en-us/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/install-imported-certificates)** - Microsoft Learn
  - Covers importing .PFX certificates into Windows Certificate Store

### Apache Official Documentation

- **[Apache SSL/TLS Encryption (mod_ssl)](https://httpd.apache.org/docs/2.4/ssl/)** ⭐ Official Apache Docs
  - Complete mod_ssl directive reference and configuration guide
  - SSL/TLS best practices and examples

- **[mod_ssl Module Reference](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html)** - Official Apache Docs
  - Directive reference: SSLCertificateFile, SSLCertificateKeyFile, SSLCertificateChainFile
  - Complete configuration examples

- **[SSL/TLS How-To](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)** - Official Apache Docs
  - Quick start guide for SSL configuration
  - Common scenarios and troubleshooting

---

## 🖼️ Screenshot Resources with Visual Walkthroughs

### IIS Certificate Import & Binding

**Third-Party Guides with Screenshots:**

- **[SSL Dragon: How to Install SSL Certificate on IIS 10+](https://www.ssldragon.com/how-to/install-ssl-certificate/iis/)**
  - Visual step-by-step with screenshots for:
    - Opening IIS Manager
    - Navigating to Server Certificates
    - Import dialog walkthrough
    - HTTPS binding configuration
    - Restarting the website

- **[My-SSL: Install SSL on IIS 10+ (Windows Server 2016-2022)](https://my-ssl.com/learn/iis-10-ssl-installation)**
  - Screenshots of:
    - IIS Manager interface
    - Certificate import wizard
    - SSL binding dialog
    - Verification steps

---

### Apache SSL Configuration

**Third-Party Guides with Examples:**

- **[DigitalOcean: How to Set Up Apache with a Free SSL Certificate](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-20-04)**
  - Virtual host configuration examples with screenshots
  - File permission examples
  - Apache restart procedures

- **[Linode: How to Install and Configure OpenSSL](https://www.linode.com/docs/guides/secure-http-with-ssl-certificates/)**
  - SSL certificate file organization
  - Virtual host configuration walkthrough
  - Configuration testing steps

---

## 🔒 Browser Certificate Verification

### Viewing SSL Certificate Details

**Google Chrome:**

1. Click the **🔒 Padlock icon** in the address bar
2. Click **"Connection is secure"**
3. Click **"Certificate is valid"** to view full details
4. Certificate details show:
   - Issuer (Certificate Authority)
   - Subject CN (domain name)
   - Validity dates
   - Certification path (chain)

Reference: Chrome's Help Center - [Check if a site is secure](https://support.google.com/chrome/answer/95617)

**Mozilla Firefox:**

1. Click the **🔒 Padlock icon** in the address bar
2. Click **"Connection secure"** arrow
3. Click **"More Information"**
4. In the pop-up, click **"View Certificate"**
5. Review certificate details including:
   - Issuer
   - Subject CN
   - Valid From / Valid Until
   - Certification path

Reference: Firefox Security - [Connection Security](https://support.mozilla.org/en-US/kb/connection-security-error)

**Microsoft Edge:**

1. Click the **🔒 Padlock icon** in the address bar
2. Click **"Certificate (Valid)"**
3. View certificate details in the sidebar
4. Same information as Chrome/Firefox

Reference: Edge Security Documentation

---

## ✅ Successful Deployment Examples

### What a Valid HTTPS Connection Looks Like

**Visual Indicators:**

```
Browser Address Bar:
🔒 https://example.com    ← Green lock icon present
   Connection is secure
   └─ Issued by: GoDaddy Secure Certificate Authority
   └─ Valid from Jan 15, 2025 to Jan 15, 2026
   └─ No warnings or errors
```

**How to Verify:**

1. **Green/Grey Padlock** — Indicates encryption is active
2. **No Warning Messages** — No "Not Secure" or "Deceptive Site"
3. **Certificate Details Show:**
   - ✓ Correct domain name (CN matches URL)
   - ✓ Issued by trusted CA (not self-signed)
   - ✓ Current date is within validity period
   - ✓ Complete certificate chain present

### SSL Labs A Grade Results

**See Real Examples:**

- **[SSL Labs Test Site](https://www.ssllabs.com/ssltest/)** — Enter any HTTPS domain to see its certificate grade
- An **A grade** indicates:
  - ✓ Valid certificate from trusted CA
  - ✓ Strong encryption (TLS 1.2+)
  - ✓ No weak ciphers or protocols
  - ✓ HSTS enabled (bonus points)
  - ✓ No known vulnerabilities

**Public A-Grade Examples:**
- `https://www.google.com` (Grade A+)
- `https://www.github.com` (Grade A+)
- `https://www.microsoft.com` (Grade A)

Test them yourself to see what an ideal certificate configuration looks like.

---

## ❌ Common Certificate Errors

### Browser Error Examples

**Error 1: Self-Signed Certificate (ERR_CERT_AUTHORITY_INVALID)**

Visual indicators:
```
🔴 URL: https://example.com
   ❌ "Your connection is not private"
   ❌ NET::ERR_CERT_AUTHORITY_INVALID
   ❌ "The certificate is not trusted"
```

How to see this error live:
- Visit any site with self-signed certificates
- Many internal corporate sites show this before proper CA certificates are installed

**Fix:** Ensure certificate is issued by a trusted CA (GoDaddy, DigiCert, Let's Encrypt, etc.)

---

**Error 2: Expired Certificate**

Visual indicators:
```
🔴 URL: https://example.com
   ❌ "This site's certificate has expired"
   ❌ "Valid until: Jan 15, 2025" (date is in the past)
```

To see this error, you can visit sites that deliberately expired their certificates for testing purposes.

**Fix:** Deploy the newly renewed certificate using the steps in the main README.md

---

**Error 3: Domain Mismatch**

Visual indicators:
```
🔴 URL: https://example.com
   ❌ "The certificate is not valid for example.com"
   ❌ "Certificate is for: old-site.com"
```

**Fix:** Verify the certificate Subject CN matches the domain being accessed
- Check: `openssl x509 -noout -subject -in cert.pem`
- Ensure IIS binding host name matches certificate CN
- Ensure Apache ServerName matches certificate CN

---

## 🛠️ Interactive Tools & Validators

### Online Certificate Checkers

**[SSL Labs by Qualys](https://www.ssllabs.com/ssltest/)**
- Free, comprehensive SSL/TLS security analyzer
- Shows certificate chain, protocols, ciphers
- Assigns letter grade (A+, A, B, C, etc.)
- How to use:
  1. Visit https://www.ssllabs.com/ssltest/
  2. Enter your domain
  3. Click "Analyze"
  4. Wait 2-5 minutes for results
  5. Review grade and recommendations

**[DigiCert SSL Certificate Checker](https://www.digicert.com/help/certificate-validator)**
- Quick certificate validation
- Shows certificate details and chain
- Highlights potential issues

**[Crt.sh Certificate Transparency Search](https://crt.sh/)**
- View all certificates issued for a domain
- Certificate transparency logs
- Historical certificate records

---

## 📋 File Locations Reference

### Windows (IIS)

**Certificate Store:**
```
Control Panel → Manage User Certificates → Personal → Certificates

OR

mmc.exe → Add Certificates snap-in → Computer account → Local Computer
  → Certificates (Local Computer) → Personal → Certificates
```

**IIS Configuration:**
```
C:\Windows\System32\inetsrv\config\applicationHost.config
  (Contains SSL binding configurations)
```

**IIS Logs:**
```
C:\inetpub\logs\LogFiles\W3SVC1\  (Website logs)
C:\inetpub\logs\LogFiles\FTPSVC1\ (FTP logs if applicable)
```

**View in IIS Manager:**
```
inetmgr → Server node → Server Certificates
  (View, import, and manage certificates here)
```

---

### Linux/Apache

**Certificate Files:**
```
/etc/apache2/ssl/privatekey.key          (Private key - chmod 600)
/etc/apache2/ssl/server-cert.pem         (Server certificate)
/etc/apache2/ssl/intermediate-ca.pem     (Intermediate certificates)
```

**Virtual Host Configuration:**
```
/etc/apache2/sites-available/default-ssl.conf     (Ubuntu/Debian)
/etc/apache2/sites-available/example.com-ssl.conf (Custom site)
/etc/httpd/conf.d/ssl.conf                        (CentOS/RHEL)
```

**Apache Logs:**
```
/var/log/apache2/error.log      (Error log - check here first)
/var/log/apache2/access.log     (Access log)
/var/log/httpd/error_log        (CentOS/RHEL)
/var/log/httpd/access_log       (CentOS/RHEL)
```

**Enabled Sites:**
```
/etc/apache2/sites-enabled/     (Symlinks to active site configs)
```

---

## 🔄 Step-by-Step Quick Reference

### IIS Workflow (ASCII Diagram)

```
1. Obtain .PFX from CA
   ↓
2. Open IIS Manager (Win + R → inetmgr)
   ↓
3. Select Server → Server Certificates
   ↓
4. Click "Import..." (right panel)
   ├─ Browse to .PFX file
   ├─ Enter password
   └─ Click OK
   ↓
5. Select Website → Bindings (right panel)
   ↓
6. Click "Add..." or "Edit..." (HTTPS)
   ├─ Type: https
   ├─ Port: 443
   ├─ Host: example.com
   └─ SSL Certificate: (select from dropdown)
   ↓
7. Click OK
   ↓
8. Right-click Website → Restart
   ↓
9. Test in browser → should show 🔒 green lock
```

---

### Apache Workflow (ASCII Diagram)

```
1. Obtain .key, .pem, and intermediate files
   ↓
2. Copy files to /etc/apache2/ssl/
   $ sudo cp *.key *.pem /etc/apache2/ssl/
   ↓
3. Set permissions
   $ sudo chmod 600 /etc/apache2/ssl/privatekey.key
   ↓
4. Enable SSL module
   $ sudo a2enmod ssl
   ↓
5. Edit /etc/apache2/sites-available/default-ssl.conf
   ├─ SSLCertificateFile /etc/apache2/ssl/server-cert.pem
   ├─ SSLCertificateKeyFile /etc/apache2/ssl/privatekey.key
   └─ SSLCertificateChainFile /etc/apache2/ssl/intermediate-ca.pem
   ↓
6. Test syntax
   $ sudo apache2ctl configtest  (should return "Syntax OK")
   ↓
7. Restart Apache
   $ sudo systemctl restart apache2
   ↓
8. Test in browser → should show 🔒 green lock
```

---

## 📞 Getting Help

### When Something Goes Wrong

**IIS Troubleshooting:**
1. Check IIS Application Event Log: **Event Viewer → Windows Logs → Application**
2. Verify certificate thumbprint matches binding: `netsh http show sslcert`
3. Clear SSL bindings cache: `netsh http delete sslcert ipport=0.0.0.0:443`
4. Re-import and re-bind certificate

Reference: [Microsoft Troubleshooting SSL](https://learn.microsoft.com/en-us/troubleshoot/windows-server/certificates-and-public-key-infrastructure-pki/ssl-https-errors)

**Apache Troubleshooting:**
1. Check error log: `sudo tail -f /var/log/apache2/error.log`
2. Test config syntax: `sudo apache2ctl configtest`
3. Check file permissions: `ls -la /etc/apache2/ssl/`
4. Verify certificate chain: `openssl verify -CAfile intermediate.pem cert.pem`

Reference: [Apache SSL Documentation](https://httpd.apache.org/docs/2.4/ssl/)

---

## 🎓 Learning Resources

**Official Training:**
- [Microsoft Learn IIS Documentation](https://learn.microsoft.com/en-us/iis/)
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [OpenSSL Official Documentation](https://www.openssl.org/docs/)

**Community Guides:**
- [Linux Academy (now A Cloud Guru)](https://www.acloud.guru/)
- [Pluralsight IIS Courses](https://www.pluralsight.com/)
- [Udemy SSL/TLS Courses](https://www.udemy.com/)

**Quick Reference:**
- [SSL.com Knowledge Base](https://www.ssl.com/kb/)
- [Digicert Learning Center](https://www.digicert.com/learning/)

---

*For more detailed deployment instructions, see [README.md](./README.md) in this repository.*
