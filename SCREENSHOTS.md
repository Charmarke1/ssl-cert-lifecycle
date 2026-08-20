# Visual Screenshot Guide: SSL Certificate Deployment

This guide contains detailed descriptions of where to click in each interface, similar to the annotated screenshot you provided.

---

## IIS Manager - Step-by-Step Visual Guide

### **Screenshot 1: Open IIS Manager & Select Your Website**

```
┌─────────────────────────────────────────────────────────────────────┐
│ Internet Information Services (IIS) Manager                          │
│ File  View  Help                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Connections Panel (LEFT)                                            │
│  ┌─────────────────────┐                                             │
│  │ 📁 UROSI (GALAXY)   │ ← Your Server                              │
│  │   ├─ Sites          │                                             │
│  │   │   ├─ Default    │                                             │
│  │   │   └─ EPMWebDAV  │ ← 🔴 CLICK #1: Select your website        │
│  │   └─ App Pools      │                                             │
│  └─────────────────────┘                                             │
│                                                                      │
│  Center Panel: EPMWebDAV Home                                        │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │ Feature Name    │ Description                               │    │
│  │ .NET Auth...    │ Configure rules for authorizing users...  │    │
│  │ .NET Compl...   │ Configure properties for compiling...     │    │
│  │ [more features]                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
│  Right Panel: Actions                                                │
│  ┌──────────────────────┐                                            │
│  │ 🔍 Explore           │                                            │
│  │ ✏️ Edit Permissions  │                                            │
│  │ ✏️ Edit Site         │                                            │
│  │ ✅ Bindings...       │ ← 🟢 CLICK #2: Configure HTTPS binding    │
│  │ 🌐 Basic Settings... │                                            │
│  └──────────────────────┘                                            │
└─────────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Select your website (EPMWebDAV) in the Connections panel (LEFT)
  2️⃣  In the RIGHT panel, click "Bindings..."
```

---

### **Screenshot 2: Site Bindings Dialog - Add HTTPS**

```
┌─────────────────────────────────────────────────────────────────┐
│ Site Bindings - EPMWebDAV                                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ Type  │ Host Name      │ Port │ IP Address │ Binding... │    │
│  ├─────────────────────────────────────────────────────────┤    │
│  │ http  │ example.com    │ 80   │ All Unas...│             │    │
│  │ https │ example.com    │ 443  │ All Unas...│ (SNI)       │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Buttons at bottom:                                              │
│  ┌─────────┐  ┌──────────┐  ┌────────────┐  ┌────┐ ┌────────┐  │
│  │ Add...  │  │ Edit...  │  │ Remove     │  │ OK │ │ Cancel │  │
│  └─────────┘  └──────────┘  └────────────┘  └────┘ └────────┘  │
│     ▲            ▲                                                │
│     │            └─ 🟢 CLICK #2: Edit HTTPS binding             │
│     └──── 🟢 CLICK #2a: Add if no HTTPS binding exists          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  If no HTTPS binding exists → Click "Add..." and proceed to Screenshot 3
  If HTTPS binding exists   → Click "Edit..." and proceed to Screenshot 3
```

---

### **Screenshot 3: Edit Site Binding - Configure HTTPS & Select Certificate**

```
┌─────────────────────────────────────────────────────────────────┐
│ Edit Site Binding                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Type:              [https ▼]                                    │
│                     └─ Must be "https"                           │
│                                                                  │
│  IP address:        [All Unassigned ▼]                           │
│                     └─ Leave as default                          │
│                                                                  │
│  Port:              [443 ]                                       │
│                     └─ Standard HTTPS port                       │
│                                                                  │
│  Host name:         [example.com              ]                  │
│                     └─ 🟢 FILL: Your domain name                │
│                                                                  │
│  SSL certificate:   [GoDaddy: example.com (Exp: Jan 15 2026) ▼] │
│                     ▲                                             │
│                     └─ 🟢 CLICK #3: Select your certificate     │
│                        (Shows domain + expiration date)          │
│                                                                  │
│  ☑ Require Server Name Indication (SNI)                         │
│     └─ Modern browsers - usually checked by default              │
│                                                                  │
│                            [OK]  [Cancel]                        │
│                             ▲                                     │
│                             └─ 🟢 CLICK #4: Save configuration  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Type: Make sure it's set to "https"
  2️⃣  Host name: Enter your domain (e.g., example.com)
  3️⃣  SSL certificate: Click dropdown and select your newly imported cert
      Look for your domain name + expiration date
  4️⃣  Click "OK" to save
```

---

### **Screenshot 4: Verify Certificate Selection**

```
┌─────────────────────────────────────────────────────────────────┐
│ Edit Site Binding                                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Type:              https ✓                                      │
│  IP address:        All Unassigned ✓                             │
│  Port:              443 ✓                                        │
│  Host name:         example.com ✓                                │
│                                                                  │
│  SSL certificate:   ┌─ Certificate Dropdown ──────────────────┐  │
│                     │ ✓ GoDaddy: example.com (Jan 15 2026)    │  │
│                     │ ✓ GoDaddy: old-site.com (EXPIRED)       │  │
│                     │ ✓ Self-Signed: local-testing            │  │
│                     └──────────────────────────────────────────┘  │
│                     ▲ MAKE SURE YOU SELECT THE RIGHT ONE:         │
│                       - Domain matches your website               │
│                       - Expiration date is in the FUTURE         │
│                       - NOT expired                              │
│                                                                  │
│  ☑ Require Server Name Indication (SNI)                         │
│                                                                  │
│                            [OK]  [Cancel]                        │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

⚠️  CRITICAL: Verify you select the CORRECT certificate:
    ❌ Wrong: example.com (EXPIRED) 
    ✅ Right: example.com (Jan 15 2026)
```

---

### **Screenshot 5: Restart Website to Apply Changes**

```
┌─────────────────────────────────────────────────────────────────┐
│ Internet Information Services (IIS) Manager                      │
│                                                                  │
│  Connections Panel (LEFT)                                        │
│  ┌─────────────────────┐                                         │
│  │ 📁 UROSI (GALAXY)   │                                         │
│  │   ├─ Sites          │                                         │
│  │   │   ├─ Default    │                                         │
│  │   │   └─ EPMWebDAV  │ ← RIGHT-CLICK HERE                     │
│  │   └─ App Pools      │                                         │
│  └─────────────────────┘                                         │
│                                                                  │
│     Context Menu appears:                                        │
│     ┌───────────────────────┐                                    │
│     │ Edit Site             │                                    │
│     │ Browse (http)         │                                    │
│     │ Browse (https)        │                                    │
│     │ ────────────────      │                                    │
│     │ Manage Website        │                                    │
│     │  ├─ Start             │                                    │
│     │  ├─ Stop              │                                    │
│     │  └─ Restart           │ ← 🟢 CLICK: Restart              │
│     │ ────────────────      │                                    │
│     │ Explore               │                                    │
│     │ Remove                │                                    │
│     └───────────────────────┘                                    │
│                                                                  │
│  After clicking "Restart":                                       │
│  ✓ Website status changes: Stopping → Stopped → Starting → Started
│  ✓ New certificate is now active                                 │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Right-click your website in the left panel
  2️⃣  Click "Manage Website" → "Restart"
  3️⃣  Wait for status to change to "Started" (green)
  4️⃣  Your new SSL certificate is now live!
```

---

## Certificate Import Before Binding - Optional Path

### **Screenshot 1b: Import Certificate (If Not Already Imported)**

```
┌─────────────────────────────────────────────────────────────────┐
│ Internet Information Services (IIS) Manager                      │
│                                                                  │
│  Connections Panel (LEFT)                                        │
│  ┌─────────────────────────┐                                     │
│  │ 📁 UROSI (GALAXY)       │ ← Click server node (top)          │
│  │   ├─ Application Pools  │                                     │
│  │   ├─ Sites              │                                     │
│  │   └─ Server Certificates│                                     │
│  └─────────────────────────┘                                     │
│           ▲                                                       │
│           └─ 🟢 CLICK #1: Click on server name                  │
│                                                                  │
│  Center Panel: Features                                          │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ Server Certificates   ← 🟢 CLICK #2: Double-click this  │   │
│  │ Application Settings                                     │   │
│  │ Machine Key                                              │   │
│  │ [more features]                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Click your server node (e.g., UROSI) in the left panel
  2️⃣  In the center panel, double-click "Server Certificates"
```

---

### **Screenshot 2b: Server Certificates View - Import**

```
┌─────────────────────────────────────────────────────────────────┐
│ Server Certificates                                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Center Panel: Existing Certificates                             │
│  ┌──────────────────────────────────────────────────────────┐    │
│  │ Certificate Name    │ Issued To     │ Expiration Date     │    │
│  ├──────────────────────────────────────────────────────────┤    │
│  │ GoDaddy: app.com    │ app.com       │ Jan 15 2025 ❌      │    │
│  │ GoDaddy: old-app    │ old-app.com   │ Dec 30 2024 ❌ EXP  │    │
│  └──────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Right Panel: Actions                                            │
│  ┌─────────────────────────┐                                     │
│  │ Create Self-Signed...   │                                     │
│  │ 📥 Import...            │ ← 🟢 CLICK #3: Import new cert     │
│  │ Bind...                 │                                     │
│  │ Renew...                │                                     │
│  │ Edit...                 │                                     │
│  │ Delete                  │                                     │
│  │ View...                 │                                     │
│  └─────────────────────────┘                                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Review existing certificates (check for expired ones)
  2️⃣  In the RIGHT panel, click "Import..."
```

---

### **Screenshot 3b: Import Certificate Dialog**

```
┌─────────────────────────────────────────────────────────────────┐
│ Import Certificate                                               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Certificate File:                                               │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ C:\certificates\example_com.pfx                            │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ┌──────────────┐                                                │
│  │ 📁 Browse... │ ← 🟢 CLICK #4: Browse to your .PFX file       │
│  └──────────────┘                                                │
│                                                                  │
│  Password:                                                       │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ ••••••••••••••••                                            │ │
│  └────────────────────────────────────────────────────────────┘ │
│  ← 🟢 ENTER: Your .PFX password (if prompted)                   │
│                                                                  │
│  ☑ Allow this certificate to be exported                        │
│    └─ Usually leave checked                                     │
│                                                                  │
│                        [Import]  [Cancel]                       │
│                           ▲                                      │
│                           └─ 🟢 CLICK #5: Import certificate   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

ACTION:
  1️⃣  Click "Browse..." to locate your .PFX file
  2️⃣  Select the certificate file (e.g., example_com.pfx)
  3️⃣  Click "Open" to select it
  4️⃣  Enter the password (if required)
  5️⃣  Click "Import" to import the certificate
  6️⃣  Certificate should now appear in the Server Certificates list
```

---

## Apache - Terminal Visual Guide

### **Apache Deployment Steps with Terminal Output**

```bash
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 1: Copy Certificate Files
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo cp privatekey.key /etc/apache2/ssl/
$ sudo cp server-cert.pem /etc/apache2/ssl/
$ sudo cp intermediate-ca.pem /etc/apache2/ssl/

✓ Certificates copied to /etc/apache2/ssl/

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 2: Set Correct Permissions (CRITICAL)
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo chmod 600 /etc/apache2/ssl/privatekey.key

$ sudo ls -la /etc/apache2/ssl/
-rw------- 1 root root 1748 Aug 20 14:32 intermediate-ca.pem
-rw------- 1 root root 1234 Aug 20 14:32 privatekey.key        ← Permission 600 ✓
-rw-r--r-- 1 root root 2048 Aug 20 14:32 server-cert.pem

✓ Private key is readable only by root

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 3: Enable SSL Module
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo a2enmod ssl

Enabling module ssl.
To activate the new configuration, you need to run:
  sudo systemctl restart apache2

✓ SSL module enabled

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 4: Edit Apache Configuration
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo nano /etc/apache2/sites-available/default-ssl.conf

# Find and update these lines:
# ────────────────────────────────────────────────────────────────

<VirtualHost *:443>
    ServerName example.com
    
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/server-cert.pem      ← ✓ Update
    SSLCertificateKeyFile /etc/apache2/ssl/privatekey.key    ← ✓ Update
    SSLCertificateChainFile /etc/apache2/ssl/intermediate-ca.pem ← ✓ Update
    
    DocumentRoot /var/www/html
    # ... rest of config
</VirtualHost>

# Save: Ctrl + O (Enter) then Ctrl + X

✓ Configuration file updated

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 5: Test Apache Configuration (BEFORE restarting!)
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo apache2ctl configtest

Syntax OK    ← ✓ Configuration is valid

# ❌ If you see an error:
# AH00112: Warning: DocumentRoot [/var/www/html] does not exist
# ❌ Fix it before restarting

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 6: Restart Apache
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ sudo systemctl restart apache2

$ sudo systemctl status apache2

● apache2.service - The Apache HTTP Server
   Loaded: loaded (...)
   Active: active (running) since Aug 20 14:35:22 UTC 2026  ← ✓ Running

✓ Apache is running with new SSL certificate

# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
# Step 7: Verify Certificate is Live
# ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

$ openssl s_client -connect example.com:443 -showcerts

CONNECTED(00000003)
depth=2 C = US, O = GoDaddy, CN = GoDaddy Root CA
verify return:1
depth=1 CN = GoDaddy Secure Certificate Authority
verify return:1
depth=0 CN = example.com
verify return:1

Verify return code: 0 (ok)    ← ✓ Certificate chain is valid!
```

---

## Browser Verification - Visual Steps

### **Chrome: View Certificate Details**

```
┌─────────────────────────────────────────────────────────────────┐
│ Chrome Browser                                                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Address Bar:                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ 🔒 https://example.com                                   │   │
│  │    ↑                                                      │   │
│  │    └─ 🟢 CLICK: Padlock icon                             │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Popup appears:                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ ✓ Connection is secure                                  │   │
│  │                                                          │   │
│  │ This site is encrypted and authenticated by             │   │
│  │ GoDaddy Secure Certificate Authority                    │   │
│  │                                                          │   │
│  │ [Certificate is valid]  ← 🟢 CLICK: View cert details  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Certificate Details Window:                                     │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ General | Details | Certification Path                  │   │
│  ├──────────────────────────────────────────────────────────┤   │
│  │                                                          │   │
│  │ Subject: CN = example.com                    ✓           │   │
│  │ Issuer: GoDaddy Secure Certificate Auth...   ✓           │   │
│  │ Valid From: Jan 15, 2025                     ✓           │   │
│  │ Valid Until: Jan 15, 2026                    ✓           │   │
│  │ Public Key: RSA (2048 bits)                  ✓           │   │
│  │                                                          │   │
│  │ Status: ✓ This certificate is valid                     │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

✓ SUCCESS: Certificate is valid and trusted
```

---

### **Firefox: View Certificate Details**

```
┌─────────────────────────────────────────────────────────────────┐
│ Firefox Browser                                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Address Bar:                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ 🔒 https://example.com                                   │   │
│  │    ↑                                                      │   │
│  │    └─ 🟢 CLICK: Padlock icon                             │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Popup appears:                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ Connection secure                                       │   │
│  │                                                          │   │
│  │ The owner of example.com has configured their website   │   │
│  │ certificate correctly.                                   │   │
│  │                                                          │   │
│  │ [More Information]  ← 🟢 CLICK: Show full details      │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Page Info Window:                                               │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ General | Security | Privacy | Permissions              │   │
│  ├──────────────────────────────────────────────────────────┤   │
│  │                                                          │   │
│  │ 🔒 Secure Connection                                     │   │
│  │    Verified by: GoDaddy Secure Certificate Authority    │   │
│  │    Certificate is valid                                  │   │
│  │                                                          │   │
│  │ [View Certificate]  ← 🟢 CLICK: See certificate details │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Certificate Viewer:                                             │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ General | Details | Certification Path                  │   │
│  ├──────────────────────────────────────────────────────────┤   │
│  │                                                          │   │
│  │ Subject Name:                                            │   │
│  │   CN = example.com                           ✓           │   │
│  │                                                          │   │
│  │ Issuer Name:                                             │   │
│  │   O = GoDaddy, CN = GoDaddy Secure Auth...  ✓           │   │
│  │                                                          │   │
│  │ Validity:                                                │   │
│  │   Not Before: Jan 15, 2025                  ✓           │   │
│  │   Not After: Jan 15, 2026                   ✓           │   │
│  │                                                          │   │
│  │ Public Key: RSA (2048 bits)                 ✓           │   │
│  │                                                          │   │
│  │ Status: ✓ Certificate is valid              ✓           │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

✓ SUCCESS: Certificate is valid and trusted
```

---

## Troubleshooting - Error Screenshots

### **Error 1: Self-Signed Certificate (ERR_CERT_AUTHORITY_INVALID)**

```
┌─────────────────────────────────────────────────────────────────┐
│ Chrome Browser - ERROR                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Address Bar:                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ ❌ https://example.com                                   │   │
│  │     ↑                                                     │   │
│  │     └─ Red X on padlock (NOT SECURE)                     │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Error Message:                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ 🔴 Your connection is not private                        │   │
│  │                                                          │   │
│  │ Attackers might be trying to steal your information     │   │
│  │ from example.com (e.g., passwords, messages, cards).    │   │
│  │                                                          │   │
│  │ NET::ERR_CERT_AUTHORITY_INVALID                          │   │
│  │                                                          │   │
│  │ [Advanced] [Go Back]                                     │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│ FIX: Certificate not issued by trusted Certificate Authority    │
│      ✓ Ensure certificate is from GoDaddy, DigiCert, etc.      │
│      ✓ NOT self-signed or custom CA                            │
│      ✓ Restart IIS/Apache after deploying correct cert         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### **Error 2: Expired Certificate**

```
┌─────────────────────────────────────────────────────────────────┐
│ Chrome Browser - ERROR                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Address Bar:                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ ❌ https://example.com                                   │   │
│  │     ↑                                                     │   │
│  │     └─ Red X on padlock                                  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Error Message:                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ 🔴 Your connection is not private                        │   │
│  │                                                          │   │
│  │ This site's certificate has expired.                     │   │
│  │                                                          │   │
│  │ Certificate expired on: Jan 15, 2025                     │   │
│  │ Current date: Jan 20, 2025                               │   │
│  │                                                          │   │
│  │ NET::ERR_CERT_DATE_INVALID                               │   │
│  │                                                          │   │
│  │ [Advanced] [Go Back]                                     │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│ FIX: Deploy the newly renewed certificate                       │
│      ✓ Verify new certificate expiration date is in future     │
│      ✓ Follow Steps 1-5 in the IIS/Apache deployment guide     │
│      ✓ Restart website/Apache service                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

### **Error 3: Domain Mismatch**

```
┌─────────────────────────────────────────────────────────────────┐
│ Chrome Browser - ERROR                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  You typed:       https://example.com                            │
│  Certificate is:  https://old-site.com                           │
│                                                                  │
│  Address Bar:                                                    │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ ❌ https://example.com                                   │   │
│  │     ↑                                                     │   │
│  │     └─ Red X on padlock                                  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  Error Message:                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │ 🔴 Your connection is not private                        │   │
│  │                                                          │   │
│  │ The certificate is not valid for: example.com           │   │
│  │ Certificate is for: old-site.com                         │   │
│  │                                                          │   │
│  │ NET::ERR_CERT_COMMON_NAME_INVALID                        │   │
│  │                                                          │   │
│  │ [Advanced] [Go Back]                                     │   │
│  │                                                          │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│ FIX: Domain mismatch - certificate CN doesn't match URL        │
│      ✓ Verify Certificate CN matches your domain               │
│         $ openssl x509 -noout -subject -in cert.pem             │
│      ✓ Verify IIS binding hostname matches certificate CN      │
│      ✓ Verify Apache ServerName matches certificate CN         │
│      ✓ Re-bind/restart after fixing                            │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Quick Verification Checklist

```
After deployment, verify your certificate works:

☐ Open browser → https://your-domain.com
☐ Green lock icon 🔒 appears in address bar
☐ No "Not Secure" warning message
☐ Click lock → "Connection is secure" appears
☐ View certificate details → domain name matches
☐ Certificate expiration date is in the FUTURE
☐ Issuer is a trusted Certificate Authority (GoDaddy, DigiCert, etc.)
☐ SSL Labs test returns A or A+ grade
☐ No mixed content warnings (HTTP resources on HTTPS page)
☐ Website loads normally on HTTPS

If any of the above fails:
  ❌ Issue: Self-signed certificate
  ✓ Solution: Deploy certificate from trusted CA

  ❌ Issue: Expired certificate showing
  ✓ Solution: Deploy newly renewed certificate

  ❌ Issue: Domain name mismatch
  ✓ Solution: Verify certificate CN and binding hostname match

  ❌ Issue: Permission denied (Apache)
  ✓ Solution: Run sudo chmod 600 /etc/apache2/ssl/privatekey.key

  ❌ Issue: Apache won't start
  ✓ Solution: Check error log: sudo tail -f /var/log/apache2/error.log
```

---

For more details, see the main **README.md** file in this repository.
