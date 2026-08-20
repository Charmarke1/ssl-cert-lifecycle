# Visual Screenshot Guide: SSL Certificate Deployment

This guide provides step-by-step visual instructions showing exactly where to click in IIS Manager, Apache terminal, and browser verification.

---

## IIS Manager Workflow

### Step 1: Navigate to Website Bindings

```
┌──────────────────────────────────────────────────────────────────────┐
│                  Internet Information Services (IIS) Manager          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌──────────────────┐              ┌──────────────────────────────┐  │
│  │   Connections    │              │      EPMWebDAV Home          │  │
│  │   (Left Panel)   │              │                              │  │
│  │                  │              │  Feature Name  │ Description │  │
│  │ 📁 UROSI         │              │ ─────────────────────────────│  │
│  │  ├─ Sites        │              │ .NET Auth...  │ Configure..  │  │
│  │  │  ├─ Default   │              │ .NET Compl... │ Configure..  │  │
│  │  │  └─ EPMWebDAV │◄──┬──────────│ [more features]             │  │
│  │  │     🟢Click#1 │   │          │                              │  │
│  │  ├─ App Pools    │   │          └──────────────────────────────┘  │
│  │  └─ [...]       │   │                                              │
│  │                  │   │          ┌──────────────────────────────┐  │
│  │                  │   │          │      Actions (Right)         │  │
│  └──────────────────┘   │          │                              │  │
│                         │          │ 🔍 Explore                   │  │
│                         └──────────────────────────────────────────┘  │
│                         │ ✏️  Edit Permissions                      │
│                         │                                            │
│                         │ ✏️  Edit Site                              │
│                         │                                            │
│                         │ ✅ Bindings...  🟢Click#2                │
│                         │                                            │
│                         │ ⚙️  Basic Settings...                     │
│                         │                                            │
│                         └──────────────────────────────────────────┘  │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘

🟢 ACTION #1: Click your website (EPMWebDAV) in LEFT panel
🟢 ACTION #2: Click "Bindings..." in RIGHT panel Actions
```

---

### Step 2: View & Add HTTPS Binding

```
┌──────────────────────────────────────────────────────────────────────┐
│                         Site Bindings Dialog                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌────┬──────────────┬──────┬──────────────┬────────────────────┐   │
│  │Type│  Host Name   │ Port │  IP Address  │  Binding Info      │   │
│  ├────┼──────────────┼──────┼──────────────┼────────────────────┤   │
│  │http│example.com   │  80  │All Unass...  │  example.com       │   │
│  │https│example.com  │ 443  │All Unass...  │  example.com(SNI)  │   │
│  └────┴──────────────┴──────┴──────────────┴────────────────────┘   │
│                                                                       │
│  Current Bindings:                                                   │
│  • HTTP on port 80 (existing)                                        │
│  • HTTPS on port 443 (existing)                                      │
│                                                                       │
│  ┌──────────┐  ┌────────┐  ┌────────┐  ┌─────┐  ┌────────┐         │
│  │  Add...  │  │ Edit...│  │ Remove │  │ OK  │  │ Cancel │         │
│  └──────────┘  └────────┘  └────────┘  └─────┘  └────────┘         │
│      ▲            ▲                                                    │
│      │            └─ If HTTPS exists → Click "Edit..."              │
│      └─ If no HTTPS → Click "Add..." to create new binding          │
│                                                                       │
│  🟢 ACTION #3: Click "Edit..." for existing HTTPS binding           │
│               (or "Add..." if you need to create it)                │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

### Step 3: Configure HTTPS & Select Certificate

```
┌──────────────────────────────────────────────────────────────────────┐
│                      Edit Site Binding Dialog                         │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │  Type:                    [https        ▼]                  │    │
│  │                           └─ Must be "https"                │    │
│  │                                                              │    │
│  │  IP address:              [All Unassigned ▼]                │    │
│  │                           └─ Leave as default               │    │
│  │                                                              │    │
│  │  Port:                    [443          ]                   │    │
│  │                           └─ Standard HTTPS port            │    │
│  │                                                              │    │
│  │  Host name:               [example.com             ]        │    │
│  │                           └─ 🟢 Enter your domain here      │    │
│  │                                                              │    │
│  │  SSL certificate:         [GoDaddy: example.com (Jan 15)▼]  │    │
│  │                           └─ 🟢 Click dropdown to select    │    │
│  │                              your certificate               │    │
│  │                                                              │    │
│  │  ☑ Require Server Name Indication (SNI)                    │    │
│  │     └─ For modern browsers - usually checked               │    │
│  │                                                              │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                       │
│              ┌────────────────────┬────────────┐                     │
│              │        OK          │   Cancel   │                     │
│              └────────────────────┴────────────┘                     │
│                      🟢 Click OK to save                              │
│                                                                       │
├──────────────────────────────────────────────────────────────────────┤
│ ⚠️  CRITICAL STEP: Certificate Selection                             │
│                                                                       │
│ Click the SSL certificate dropdown to see options:                   │
│                                                                       │
│  ☑ GoDaddy: example.com (Jan 15 2026)        ← ✓ SELECT THIS ONE    │
│  ☐ GoDaddy: old-site.com (Dec 30 2024)       ← ❌ EXPIRED          │
│  ☐ Self-Signed: local-test                    ← ❌ NOT TRUSTED      │
│                                                                       │
│ Verify:                                                               │
│  ✓ Domain matches your website                                       │
│  ✓ Expiration date is in FUTURE (not expired)                       │
│  ✓ NOT self-signed or expired certificate                           │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

### Step 4: Restart Website to Apply Changes

```
┌──────────────────────────────────────────────────────────────────────┐
│               Right-click Website → Manage → Restart                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌──────────────────┐         Context Menu                          │
│  │   Connections    │         ┌──────────────────────┐              │
│  │                  │         │ Edit Site            │              │
│  │ 📁 UROSI         │         │ Browse (http)        │              │
│  │  ├─ Sites        │         │ Browse (https)       │              │
│  │  │  └─ EPMWebDAV │────────┼─ Manage Website ──┐  │              │
│  │  │  (Right-click)│         │  ├─ Start        │  │              │
│  │  └─ [...]       │         │  ├─ Stop         │  │              │
│  │                  │         │  ├─ Restart  ◄──┼──┤ 🟢 Click here │
│  └──────────────────┘         │  └─ [...]       │  │              │
│                               │ Explore          │  │              │
│                               │ Remove           │  │              │
│                               └──────────────────┘  │              │
│                               └──────────────────────┘              │
│                                                                       │
│  ┌─ Website Status After Restart ────────────────────────────────┐  │
│  │                                                                │  │
│  │  Stopping...  →  Stopped  →  Starting...  →  Started ✓       │  │
│  │                                                                │  │
│  │  🟢 GREEN indicator = Website is running                      │  │
│  │  🟢 New SSL certificate is now ACTIVE                         │  │
│  │                                                                │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  🟢 ACTION #4: Right-click website → Manage Website → Restart       │
│               Wait for status to change to "Started" (green)         │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Apache Configuration Workflow

### Step 1-5: Complete Apache Deployment

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Apache SSL Deployment Pipeline                    │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐              │
│  │   Step 1    │    │   Step 2    │    │   Step 3    │              │
│  │  Copy Files │───▶│ Set Perms   │───▶│ Enable SSL  │              │
│  │             │    │             │    │             │              │
│  │ $ sudo cp   │    │ $ chmod 600 │    │ $ a2enmod   │              │
│  │  *.pem      │    │  key        │    │   ssl       │              │
│  │  *.key      │    │             │    │             │              │
│  │  to /etc... │    │ Verify:     │    │ Output:     │              │
│  │             │    │ $ ls -la    │    │ Enabling    │              │
│  │ ✓ Files in  │    │             │    │ module ssl  │              │
│  │  right dir  │    │ ✓ chmod 600 │    │             │              │
│  │             │    │   on key    │    │ ✓ Module    │              │
│  └─────────────┘    └─────────────┘    │   enabled   │              │
│                                         └─────────────┘              │
│                                                │                     │
│                                                ▼                     │
│                                         ┌─────────────┐              │
│                                         │   Step 4    │              │
│                                         │ Edit Config │              │
│                                         │             │              │
│                                         │ $ nano      │              │
│                                         │ /etc/apache2│              │
│                                         │ /sites-.../ │              │
│                                         │ default-ssl │              │
│                                         │             │              │
│                                         │ Update 3 SSL│              │
│                                         │ directives: │              │
│                                         │ • CertFile  │              │
│                                         │ • KeyFile   │              │
│                                         │ • ChainFile │              │
│                                         │             │              │
│                                         │ ✓ Save with │              │
│                                         │   Ctrl+O    │              │
│                                         │   then Exit │              │
│                                         └─────────────┘              │
│                                                │                     │
│                                                ▼                     │
│                                         ┌─────────────┐              │
│                                         │   Step 5    │              │
│                                         │ Test Config │              │
│                                         │             │              │
│                                         │ $ apache2ctl│              │
│                                         │ configtest  │              │
│                                         │             │              │
│                                         │ Output:     │              │
│                                         │ Syntax OK ✓ │              │
│                                         │             │              │
│                                         │ If error:   │              │
│                                         │ FIX before  │              │
│                                         │ restarting! │              │
│                                         └─────────────┘              │
│                                                │                     │
│                                                ▼                     │
│                                         ┌─────────────┐              │
│                                         │   Step 6    │              │
│                                         │   Restart   │              │
│                                         │             │              │
│                                         │ $ systemctl │              │
│                                         │ restart     │              │
│                                         │ apache2     │              │
│                                         │             │              │
│                                         │ Status:     │              │
│                                         │ active      │              │
│                                         │ (running) ✓ │              │
│                                         │             │              │
│                                         │ ✓ Certificate           │
│                                         │   is LIVE   │              │
│                                         └─────────────┘              │
│                                                │                     │
│                                                ▼                     │
│                                         ┌─────────────┐              │
│                                         │   Step 7    │              │
│                                         │   Verify    │              │
│                                         │             │              │
│                                         │ $ openssl   │              │
│                                         │ s_client    │              │
│                                         │ -connect    │              │
│                                         │ example.com │              │
│                                         │ :443        │              │
│                                         │             │              │
│                                         │ Verify      │              │
│                                         │ return:0 ✓  │              │
│                                         │             │              │
│                                         │ ✓ Certificate           │
│                                         │   chain OK  │              │
│                                         └─────────────┘              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Browser Verification Workflow

### Chrome: View Certificate Details

```
┌──────────────────────────────────────────────────────────────────────┐
│                         Chrome Browser                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Step 1: Address Bar                                                  │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ 🔒  https://example.com                                        │  │
│  │  ▲                                                              │  │
│  │  └─ 🟢 Click padlock icon                                      │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  Step 2: Popup Appears                                                │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ ✓ Connection is secure                                        │  │
│  │                                                                │  │
│  │ This site is encrypted and authenticated by                   │  │
│  │ GoDaddy Secure Certificate Authority                          │  │
│  │                                                                │  │
│  │ [Certificate is valid]  🟢 Click here                         │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  Step 3: Certificate Details Window Opens                             │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │  General  │  Details  │  Certification Path                   │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │                                                                │  │
│  │  Subject (CN): example.com                        ✓ MATCHES   │  │
│  │                                                                │  │
│  │  Issuer: GoDaddy Secure Certificate Authority     ✓ TRUSTED   │  │
│  │                                                                │  │
│  │  Valid From: Jan 15, 2025                         ✓ IN PAST   │  │
│  │  Valid Until: Jan 15, 2026                        ✓ IN FUTURE │  │
│  │                                                                │  │
│  │  Public Key: RSA (2048 bits)                      ✓ STRONG    │  │
│  │                                                                │  │
│  │  Status: ✓ This certificate is valid                         │  │
│  │          ✓ No warnings or errors                             │  │
│  │                                                                │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  ✅ SUCCESS: Certificate is valid, trusted, and active               │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

### Firefox: View Certificate Details

```
┌──────────────────────────────────────────────────────────────────────┐
│                        Firefox Browser                               │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Step 1: Address Bar                                                  │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ 🔒  https://example.com                                        │  │
│  │  ▲                                                              │  │
│  │  └─ 🟢 Click padlock icon                                      │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  Step 2: Popup Appears                                                │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ Connection secure                                              │  │
│  │                                                                │  │
│  │ The owner of example.com has configured their                 │  │
│  │ website certificate correctly.                                │  │
│  │                                                                │  │
│  │ [More Information]  🟢 Click here                             │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  Step 3: Page Info Window Opens                                       │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │  General  │  Security  │  Privacy  │  Permissions              │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │                                                                │  │
│  │  🔒 Secure Connection                                         │  │
│  │     Verified by: GoDaddy Secure Certificate Authority         │  │
│  │     Certificate is valid ✓                                    │  │
│  │                                                                │  │
│  │  [View Certificate]  🟢 Click here for details               │  │
│  │                                                                │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  Step 4: Certificate Viewer Opens                                     │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │  General  │  Details  │  Certification Path                   │  │
│  ├────────────────────────────────────────────────────────────────┤  │
│  │                                                                │  │
│  │  Subject Name:                                                │  │
│  │    CN = example.com                              ✓ MATCHES   │  │
│  │                                                                │  │
│  │  Issuer Name:                                                 │  │
│  │    O = GoDaddy                                   ✓ TRUSTED   │  │
│  │    CN = GoDaddy Secure Certificate Authority                 │  │
│  │                                                                │  │
│  │  Validity:                                                    │  │
│  │    Not Before: Jan 15, 2025                      ✓ IN PAST   │  │
│  │    Not After: Jan 15, 2026                       ✓ IN FUTURE │  │
│  │                                                                │  │
│  │  Public Key: RSA (2048 bits)                     ✓ STRONG    │  │
│  │                                                                │  │
│  │  Status: ✓ Certificate is valid                             │  │
│  │          ✓ No warnings or errors                             │  │
│  │                                                                │  │
│  └────────────────────────────────────────────────────────────────┘  │
│                                                                       │
│  ✅ SUCCESS: Certificate is valid, trusted, and active               │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Error Detection & Troubleshooting

### ❌ Error 1: Self-Signed Certificate (ERR_CERT_AUTHORITY_INVALID)

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Chrome Error Message                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Address Bar: ❌ https://example.com                                │
│              (Red X on padlock - NOT SECURE)                         │
│                                                                       │
│  Error Message:                                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  🔴 Your connection is not private                          │    │
│  │                                                             │    │
│  │  Attackers might be trying to steal your information       │    │
│  │  from example.com (passwords, messages, cards).            │    │
│  │                                                             │    │
│  │  NET::ERR_CERT_AUTHORITY_INVALID                           │    │
│  │                                                             │    │
│  │  [Advanced]  [Go Back]                                     │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                       │
│  ⚠️  PROBLEM: Certificate is self-signed or not from trusted CA      │
│                                                                       │
│  ✓  FIX:                                                              │
│      1. Ensure certificate is from GoDaddy, DigiCert, Let's Encrypt  │
│      2. NOT self-signed or custom certificate authority             │
│      3. Re-deploy correct certificate in IIS/Apache                 │
│      4. Restart website/Apache service                              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

### ❌ Error 2: Expired Certificate

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Chrome Error Message                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Address Bar: ❌ https://example.com                                │
│              (Red X on padlock - NOT SECURE)                         │
│                                                                       │
│  Error Message:                                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  🔴 Your connection is not private                          │    │
│  │                                                             │    │
│  │  This site's certificate has expired.                      │    │
│  │                                                             │    │
│  │  Certificate expired on:  Jan 15, 2025                      │    │
│  │  Current date:           Jan 20, 2025                       │    │
│  │                                                             │    │
│  │  NET::ERR_CERT_DATE_INVALID                                │    │
│  │                                                             │    │
│  │  [Advanced]  [Go Back]                                     │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                       │
│  ⚠️  PROBLEM: Certificate has expired (validity date is in past)    │
│                                                                       │
│  ✓  FIX:                                                              │
│      1. Obtain newly renewed certificate from CA                     │
│      2. Convert PFX to PEM (if needed)                               │
│      3. Follow full deployment steps in README.md                    │
│      4. Deploy new certificate in IIS/Apache                        │
│      5. Restart website/Apache service                              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

### ❌ Error 3: Domain Name Mismatch

```
┌──────────────────────────────────────────────────────────────────────┐
│                    Chrome Error Message                              │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  URL Accessed:         https://example.com                           │
│  Certificate For:      https://old-site.com                          │
│                                                                       │
│  Address Bar: ❌ https://example.com                                │
│              (Red X on padlock - NOT SECURE)                         │
│                                                                       │
│  Error Message:                                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                                                             │    │
│  │  🔴 Your connection is not private                          │    │
│  │                                                             │    │
│  │  The certificate is not valid for: example.com             │    │
│  │                                                             │    │
│  │  The certificate is for: old-site.com                      │    │
│  │                                                             │    │
│  │  NET::ERR_CERT_COMMON_NAME_INVALID                         │    │
│  │                                                             │    │
│  │  [Advanced]  [Go Back]                                     │    │
│  │                                                             │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                       │
│  ⚠️  PROBLEM: Certificate CN (domain) doesn't match URL              │
│                                                                       │
│  ✓  FIX:                                                              │
│      1. Check certificate Common Name (CN):                          │
│         $ openssl x509 -noout -subject -in cert.pem                  │
│                                                                       │
│      2. Verify IIS binding hostname matches CN:                      │
│         → Open IIS Manager → Bindings → Host name field              │
│                                                                       │
│      3. Verify Apache ServerName matches CN:                         │
│         → grep ServerName /etc/apache2/sites-available/...           │
│                                                                       │
│      4. If mismatch: Get new certificate for correct domain          │
│         OR update binding/config to match certificate                │
│                                                                       │
│      5. Restart website/Apache service                              │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Verification Checklist

```
┌──────────────────────────────────────────────────────────────────────┐
│         After Deployment: Verify Your SSL Certificate Works           │
├──────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Open https://your-domain.com in browser:                            │
│                                                                       │
│  ☐ Green/Grey padlock 🔒 in address bar                             │
│  ☐ No "Not Secure" warning message                                  │
│  ☐ Click lock → "Connection is secure" text                         │
│  ☐ View certificate → Domain name matches URL                       │
│  ☐ Certificate expiration date is in FUTURE                         │
│  ☐ Issuer is trusted CA (GoDaddy, DigiCert, Let's Encrypt)          │
│  ☐ No red warnings or "Invalid" messages                            │
│  ☐ Website content loads normally                                   │
│  ☐ No mixed content warnings (HTTP resources on HTTPS page)         │
│  ☐ SSL Labs test (ssllabs.com) returns A or A+ grade                │
│                                                                       │
│  ✅ ALL CHECKED? Your certificate deployment is successful!          │
│                                                                       │
│  ❌ ANY FAILED? Use error section above to diagnose and fix          │
│                                                                       │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference Links

- **[Microsoft Learn: Configure SSL in IIS Manager](https://learn.microsoft.com/en-us/iis/manage/configuring-security/configuring-ssl-in-iis-manager/)**
- **[Apache mod_ssl Documentation](https://httpd.apache.org/docs/2.4/mod/mod_ssl.html)**
- **[SSL Labs Qualys Test](https://www.ssllabs.com/ssltest/)** — Test your certificate grade
- **[Chrome Certificate Verification](https://support.google.com/chrome/answer/95617)** — Official guide
- **[Firefox Certificate Verification](https://support.mozilla.org/en-US/kb/connection-security-error)** — Official guide

---

*For detailed deployment instructions, see [README.md](./README.md) in this repository.*
