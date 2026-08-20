# Visual Guide: SSL Certificate Deployment Screenshots

This document provides step-by-step visual guidance for deploying certificates in IIS and Apache.

---

## IIS Certificate Binding Workflow

### Step 1: Open IIS Manager

```
┌─────────────────────────────────────────────────────────────┐
│  Press: Win + R                                              │
├─────────────────────────────────────────────────────────────┤
│  Type:  inetmgr                                              │
│  Press: Enter                                                │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│  Internet Information Services (IIS) Manager opens           │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ + SERVER-NAME (your server)                          │   │
│  │   ├─ Application Pools                               │   │
│  │   ├─ Sites                                           │   │
│  │   │  ├─ Default Web Site                             │   │
│  │   │  └─ YOUR-WEBSITE-NAME  ← SELECT THIS            │   │
│  │   ├─ Server Certificates    ← CLICK HERE FIRST       │   │
│  │   └─ [More options...]                               │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

### Step 2: Import the Certificate

**Click: Server Certificates (in left panel)**

```
┌─────────────────────────────────────────────────────────────┐
│  Center Panel: "Server Certificates"                         │
├─────────────────────────────────────────────────────────────┤
│  Existing Certificates (if any):                             │
│  ├─ example.com (Expires: Jan 15 2026)                       │
│  └─ [old-cert.com] (Expires: Dec 30 2024) ❌ EXPIRED        │
│                                                              │
│  Right Panel Actions:                                        │
│  ┌──────────────┐                                            │
│  │ Import...    │  ← CLICK HERE                              │
│  └──────────────┘                                            │
│  ┌──────────────┐                                            │
│  │ Bind...      │                                            │
│  └──────────────┘                                            │
│  ┌──────────────┐                                            │
│  │ Renew...     │                                            │
│  └──────────────┘                                            │
└─────────────────────────────────────────────────────────────┘
```

**Import Dialog:**

```
┌─────────────────────────────────────────────────────────────┐
│  Import Certificate                                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Certificate File:  [C:\Certs\certificate.pfx ___________]  │
│                                                      [Browse]│
│                                                              │
│  Password:          [***** ___________________________]      │
│                     ☑ Allow this password to be exported    │
│                                                              │
│                          [Import]  [Cancel]                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**After Import (Status):**

```
✓ Certificate imported successfully

Certificate details:
├─ CN: example.com
├─ Issuer: GoDaddy Secure Certificate Authority
├─ Valid From: Jan 15 2025
├─ Valid To: Jan 15 2026
└─ Thumbprint: 3F5E2A1B9C7D4E6F8A2B3C4D5E6F7A8B9C0D1E2F
```

---

### Step 3: Bind Certificate to Website

**Right-click your website → Edit Bindings**

```
┌─────────────────────────────────────────────────────────────┐
│  Left Panel: Sites                                           │
│                                                              │
│  ├─ Default Web Site                                        │
│  └─ example.com  ← RIGHT-CLICK HERE                         │
│                                                              │
│     Context Menu:                                            │
│     ┌──────────────────────────┐                             │
│     │ Edit Site                │                             │
│     │ Bindings...    ← CLICK    │                             │
│     │ Explore                  │                             │
│     │ Remove                   │                             │
│     │ Refresh                  │                             │
│     │ Properties...            │                             │
│     └──────────────────────────┘                             │
└─────────────────────────────────────────────────────────────┘
```

---

### Step 4: Add HTTPS Binding

**Site Bindings Window:**

```
┌──────────────────────────────────────────────────────────────┐
│  Site Bindings for "example.com"                             │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  Protocol  │ IP Address  │ Port │ Host Name  │ Binding Info  │
│  ───────────────────────────────────────────────────────────  │
│  http      │ All Unassign │ 80   │ example.  │ example.com   │
│            │ ed          │      │ com       │               │
│  ───────────────────────────────────────────────────────────  │
│  https     │ All Unassign │ 443  │ example.  │ example.com   │
│            │ ed          │      │ com       │ (SNI: yes)    │
│                                                               │
│  [Add...]  [Edit...]  [Remove]  [OK]  [Cancel]              │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

**Click "Edit..." (HTTPS binding):**

```
┌──────────────────────────────────────────────────────────────┐
│  Edit Site Binding                                           │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  Type:                [https ▼]                              │
│                                                               │
│  IP Address:          [All Unassigned ▼]                     │
│                                                               │
│  Port:                [443]                                  │
│                                                               │
│  Host name:           [example.com________________]          │
│                       ⚠ MUST MATCH your domain               │
│                                                               │
│  SSL Certificate:     [example.com (Expires: Jan 15 2026) ▼] │
│                                 ↑                             │
│                    SELECT YOUR NEW CERT HERE                 │
│                                                               │
│  ☑ Require Server Name Indication                            │
│                                                               │
│                          [OK]  [Cancel]                      │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

### Step 5: Restart Website

**Right-click website → Restart (or Manage Website → Restart)**

```
┌──────────────────────────────────────────────────────────────┐
│  Left Panel: Sites                                           │
│                                                              │
│  └─ example.com  ← RIGHT-CLICK HERE                         │
│                                                              │
│     Context Menu:                                            │
│     ┌──────────────────────────┐                             │
│     │ Manage Website           │                             │
│     │ ├─ Start                 │                             │
│     │ ├─ Stop                  │                             │
│     │ └─ Restart    ← CLICK    │                             │
│     │ Browse                   │                             │
│     └──────────────────────────┘                             │
└──────────────────────────────────────────────────────────────┘

Status Changes:
Started  →  Stopping  →  Stopped  →  Starting  →  Started ✓
                                                    (Ready)
```

---

## Apache Certificate Deployment Workflow

### Step 1: Copy Files to Apache

```bash
# Terminal/SSH Command Sequence:

┌────────────────────────────────────────────────────────────┐
│ $ sudo cp privatekey.key /etc/apache2/ssl/                 │
│ $ sudo cp server-cert.pem /etc/apache2/ssl/                │
│ $ sudo cp intermediate-ca.pem /etc/apache2/ssl/            │
│ $ sudo chmod 600 /etc/apache2/ssl/privatekey.key           │
│                                                             │
│ Output (no errors = success):                              │
│ $ sudo ls -la /etc/apache2/ssl/                            │
│ -rw------- 1 root root 1748 Aug 20 14:32 intermediate-ca.p │
│ -rw------- 1 root root 1234 Aug 20 14:32 privatekey.key    │
│ -rw-r--r-- 1 root root 2048 Aug 20 14:32 server-cert.pem   │
│                                                             │
│ ✓ Permission 600 (read/write for root only) ✓              │
└────────────────────────────────────────────────────────────┘
```

---

### Step 2: Enable SSL Module

```bash
┌────────────────────────────────────────────────────────────┐
│ $ sudo a2enmod ssl                                          │
│                                                             │
│ Output:                                                     │
│ Enabling module ssl.                                       │
│ To activate the new configuration, you need to run:        │
│   sudo systemctl restart apache2                           │
│                                                             │
│ ✓ SSL module enabled                                        │
└────────────────────────────────────────────────────────────┘
```

---

### Step 3: Edit Virtual Host Configuration

**File: `/etc/apache2/sites-available/default-ssl.conf`**

```apache
┌──────────────────────────────────────────────────────────────┐
│ Find and update these lines:                                 │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│ <VirtualHost *:443>                                          │
│     ServerName example.com                                   │
│     ServerAlias www.example.com                              │
│                                                               │
│     # ← UPDATE THESE PATHS ↓                                 │
│     SSLEngine on                                             │
│     SSLCertificateFile /etc/apache2/ssl/server-cert.pem      │
│     SSLCertificateKeyFile /etc/apache2/ssl/privatekey.key    │
│     SSLCertificateChainFile /etc/apache2/ssl/intermediate-ca.│
│     pem                                                      │
│     # ← /UPDATE                                              │
│                                                               │
│     DocumentRoot /var/www/html                               │
│                                                               │
│     <Directory /var/www/html>                                │
│         Options Indexes FollowSymLinks                       │
│         AllowOverride All                                    │
│         Require all granted                                  │
│     </Directory>                                             │
│                                                               │
│ </VirtualHost>                                               │
└──────────────────────────────────────────────────────────────┘

Nano Editor Shortcuts:
  Ctrl + O  → Save (then Enter to confirm)
  Ctrl + X  → Exit
```

---

### Step 4: Test Apache Configuration

```bash
┌────────────────────────────────────────────────────────────┐
│ $ sudo apache2ctl configtest                               │
│                                                             │
│ Expected Output (SUCCESS):                                  │
│ Syntax OK ✓                                                 │
│                                                             │
│ If there's an error, you'll see:                            │
│ AH00112: Warning: DocumentRoot [/var/www/html] does not    │
│ exist                                                       │
│ Syntax error on line 123 of /etc/apache2/...               │
│                                                             │
│ ⚠️ Fix the error before restarting                           │
└────────────────────────────────────────────────────────────┘
```

---

### Step 5: Restart Apache

```bash
┌────────────────────────────────────────────────────────────┐
│ $ sudo systemctl restart apache2                            │
│                                                             │
│ Status Check:                                               │
│ $ sudo systemctl status apache2                             │
│                                                             │
│ Expected Output:                                            │
│ ● apache2.service - The Apache HTTP Server                 │
│   Loaded: loaded (...) active (running) ✓                  │
│   Active: active (running) since Aug 20 14:35:22 UTC 2026  │
│                                                             │
│ ✓ Apache is running with new certificate                    │
└────────────────────────────────────────────────────────────┘
```

---

## Browser Verification Walkthrough

### What You Should See (✓ Success)

```
┌───────────────────────────────────────────────────────────┐
│ Browser Address Bar (Chrome/Firefox)                       │
├───────────────────────────────────────────────────────────┤
│                                                             │
│  🔒 example.com ✓                                           │
│  ├─ Green lock icon                                        │
│  └─ "Connection is secure"                                 │
│                                                             │
│  Click lock → "Certificate is valid"                       │
│  ├─ Subject: example.com                                   │
│  ├─ Issuer: GoDaddy Secure Certificate Authority           │
│  ├─ Valid from: Jan 15, 2025                               │
│  ├─ Valid until: Jan 15, 2026                              │
│  └─ ✓ No warnings or errors                                │
│                                                             │
└───────────────────────────────────────────────────────────┘
```

### What You Should NOT See (❌ Problems)

```
┌───────────────────────────────────────────────────────────┐
│ Bad Certificate Error Example 1: Self-Signed              │
├───────────────────────────────────────────────────────────┤
│                                                             │
│  🔴 Not secure                                              │
│  ├─ Red lock icon with X                                  │
│  ├─ "Your connection is not private"                       │
│  ├─ "NET::ERR_CERT_AUTHORITY_INVALID"                      │
│  └─ ❌ Self-signed certificate detected                     │
│                                                             │
│  FIX: Ensure certificate chain includes intermediates      │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│ Bad Certificate Error Example 2: Expired                  │
├───────────────────────────────────────────────────────────┤
│                                                             │
│  🔴 Not secure                                              │
│  ├─ Red lock icon with X                                  │
│  ├─ "This site's certificate has expired"                  │
│  ├─ Valid until: Jan 15, 2025 (date is in past)            │
│  └─ ❌ Certificate is outdated                              │
│                                                             │
│  FIX: Import the newly renewed certificate                 │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│ Bad Certificate Error Example 3: Domain Mismatch          │
├───────────────────────────────────────────────────────────┤
│  URL:  https://example.com                                 │
│                                                             │
│  🔴 Not secure                                              │
│  ├─ "Certificate name mismatch"                            │
│  ├─ "The certificate is not valid for example.com"         │
│  ├─ "Certificate is for: old-site.com"                     │
│  └─ ❌ Certificate subject doesn't match domain            │
│                                                             │
│  FIX: Verify domain name in certificate matches binding    │
└───────────────────────────────────────────────────────────┘
```

---

## Quick Reference: File Locations

### Windows (IIS)

```
┌──────────────────────────────────────────────────────────┐
│ Certificate Store:                                        │
│ C:\Users\[Username]\AppData\Roaming\Microsoft\SystemCerts │
│                                                           │
│ OR (via Cert Manager):                                   │
│ certmgr.msc → Personal → Certificates                    │
│                                                           │
│ IIS Config:                                              │
│ C:\Windows\System32\inetsrv\config\applicationHost.config │
│                                                           │
│ IIS Logs:                                                │
│ C:\inetpub\logs\LogFiles\W3SVC1\                          │
└──────────────────────────────────────────────────────────┘
```

### Linux/Apache

```
┌──────────────────────────────────────────────────────────┐
│ Certificate Files:                                        │
│ /etc/apache2/ssl/privatekey.key                           │
│ /etc/apache2/ssl/server-cert.pem                          │
│ /etc/apache2/ssl/intermediate-ca.pem                      │
│                                                           │
│ Virtual Host Config:                                      │
│ /etc/apache2/sites-available/default-ssl.conf            │
│                                                           │
│ Apache Logs:                                              │
│ /var/log/apache2/error.log                               │
│ /var/log/apache2/access.log                              │
│                                                           │
│ Enabled Sites:                                            │
│ /etc/apache2/sites-enabled/                              │
└──────────────────────────────────────────────────────────┘
```

---

## Troubleshooting: Common Visual Errors

### Error 1: "The server certificate is not trusted"

```
┌────────────────────────────────────────────────────────────┐
│ Browser Shows:                                              │
│ 🔴 SEC_ERROR_UNKNOWN_ISSUER (Firefox)                       │
│ 🔴 ERR_CERT_AUTHORITY_INVALID (Chrome)                      │
│                                                             │
│ Root Cause:                                                │
│ ├─ Intermediate certificate missing from deployment       │
│ ├─ Chain not bundled correctly                             │
│ └─ Wrong certificate authority                             │
│                                                             │
│ Fix Steps:                                                 │
│ 1. Verify intermediate files exist:                        │
│    $ ls -la /etc/apache2/ssl/                              │
│                                                             │
│ 2. Check Apache config points to intermediates:            │
│    SSLCertificateChainFile /etc/apache2/ssl/intermediate-ca│
│    .pem                                                    │
│                                                             │
│ 3. Test chain validation:                                  │
│    $ openssl verify -CAfile intermediate-ca.pem \         │
│      server-cert.pem                                       │
│    Output should be: "server-cert.pem: OK"                 │
│                                                             │
│ 4. Restart Apache:                                         │
│    $ sudo systemctl restart apache2                        │
└────────────────────────────────────────────────────────────┘
```

### Error 2: "NET::ERR_CERT_AUTHORITY_INVALID"

```
┌────────────────────────────────────────────────────────────┐
│ Browser Shows:                                              │
│ https://example.com                                         │
│ 🔴 "Your connection is not private"                         │
│ "NET::ERR_CERT_AUTHORITY_INVALID"                           │
│                                                             │
│ Root Cause:                                                │
│ └─ Browser cannot trace certificate back to trusted root   │
│                                                             │
│ Fix Steps:                                                 │
│                                                             │
│ For IIS:                                                   │
│ 1. Open IIS Manager → Server Certificates                  │
│ 2. Find the INTERMEDIATE certificate                       │
│ 3. If it's not there, import the intermediate:             │
│    - Right-click Intermediate Certification Authorities    │
│    - Import → Select intermediate-ca.pem                   │
│ 4. Return to website binding and re-select the cert        │
│ 5. Restart website                                         │
│                                                             │
│ For Apache:                                                │
│ 1. Verify SSLCertificateChainFile is set:                  │
│    grep SSLCertificateChainFile /etc/apache2/sites-enabled │
│    /default-ssl.conf                                       │
│ 2. Check file exists and is readable:                      │
│    sudo cat /etc/apache2/ssl/intermediate-ca.pem           │
│ 3. Ensure it contains "-----BEGIN CERTIFICATE-----"        │
│ 4. Restart Apache                                          │
└────────────────────────────────────────────────────────────┘
```

---

## Command Reference Card

```bash
┌─────────────────────────────────────────────────────────────┐
│ OpenSSL Quick Commands                                       │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│ # Verify private key & cert match                            │
│ openssl rsa -noout -modulus -in key.key | openssl md5        │
│ openssl x509 -noout -modulus -in cert.pem | openssl md5      │
│                                                               │
│ # Check certificate expiration                               │
│ openssl x509 -noout -dates -in cert.pem                      │
│                                                               │
│ # Verify entire chain                                        │
│ openssl verify -CAfile intermediate.pem cert.pem             │
│                                                               │
│ # View certificate details                                   │
│ openssl x509 -text -noout -in cert.pem                       │
│                                                               │
│ # Test live server certificate                               │
│ openssl s_client -connect example.com:443 -showcerts         │
│                                                               │
│ # Extract certificate from server                            │
│ openssl s_client -connect example.com:443 </dev/null | \     │
│   openssl x509 -outform PEM -out downloaded-cert.pem         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Summary Checklist

```
Before Deployment:
  ☐ Certificate received from CA
  ☐ PFX/P7B downloaded securely
  ☐ Password stored safely (not in repo)
  ☐ Certificate converted to PEM format
  ☐ Private key extracted and secured (chmod 600)
  ☐ Chain validation passed (openssl verify)

During Deployment:
  ☐ Files copied to correct directories
  ☐ File permissions set correctly
  ☐ IIS binding updated with correct thumbprint
  ☐ Apache config file updated with correct paths
  ☐ Configuration syntax tested (apache2ctl configtest)
  ☐ Service restarted

After Deployment:
  ☐ Browser shows green lock 🔒
  ☐ Certificate expiration is correct
  ☐ No mixed content warnings
  ☐ openssl s_client returns "Verify return code: 0 (ok)"
  ☐ Dependent services running without errors
  ☐ SSL Labs score is A or higher
```
