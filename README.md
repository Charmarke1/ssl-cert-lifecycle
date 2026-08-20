# SSL/TLS Certificate Lifecycle Management

A case study documenting how I manage the full SSL/TLS certificate lifecycle in production —
from a renewed certificate landing in my queue to a verified, browser-trusted deployment —
based on hands-on experience supporting a multi-server ERP platform in a technical support role.

![SSL/TLS Certificate Renewal & Deployment Pipeline](./ssl-cert-pipeline.png)

> **Note:** This write-up generalizes real production work into a public-safe case study.
> Client names, hostnames, and environment-specific details have been removed or genericized.

---

## Background

Production environments I supported ran several components that each depend on their own
valid SSL/TLS certificate: an application server, an infrastructure server, an internal
gateway service, a content management service, and a database/reporting server. Certificates
across all of them expire on independent schedules, get reissued by different certificate
authorities, and sometimes need to move between formats depending on which service is
consuming them. Managing that lifecycle without causing an outage is the actual job.

## The Process

**1. Certificate renewed** — A certificate authority (in my case, most often GoDaddy or
Starfield) issues a renewed certificate as a PFX bundle.

**2. Convert to PEM** — Web servers like IIS and Apache/Tomcat don't consume PFX directly in
the way I needed for validation, so I converted the bundle to PEM format, carefully extracting
the private key, server certificate, and intermediate certificates as separate components.

**3. Validate the chain** — Before anything touches production, I validate the complete trust
chain — private key → server certificate → intermediate certificate(s) → root CA — checking
for mismatches, expired intermediates, or an incomplete chain that would pass locally but fail
in a real browser.

**4. Deploy the binding** — Once validated, I update the actual binding: IIS SSL bindings
(including thumbprint configuration) for Windows-hosted services, or the PEM files and
configuration on Apache/Tomcat, followed by a service restart.

**5. Verify live** — Final step is confirming a clean handshake with no browser trust warnings
and no regressions on dependent services.

## Common Failure Modes & How I Diagnosed Them

| Failure | What it looks like | Root cause |
|---|---|---|
| PKIX path building error | TLS handshake fails post-renewal even though the cert "looks" valid | Intermediate certificate missing or installed in the wrong store |
| KeyUsage incompatibility | Certificate is trusted but rejected for the specific service using it | Certificate issued with extensions that don't match how the service uses it |
| Incomplete chain | Works in some browsers/tools, fails in others | Root or intermediate CA not included in the deployed bundle |
| Host header / thumbprint mismatch | Deployment completes but the wrong certificate is served | Binding updated with the correct thumbprint but wrong host header, or vice versa |

Diagnosing these reliably comes down to validating the chain manually (I used tools like
OpenSSL and a plain text editor to inspect each certificate in the bundle) rather than trusting
that "renewed" means "correctly configured."

## Skills Demonstrated

`SSL/TLS` `PFX → PEM Conversion` `Certificate Chain Validation` `IIS` `Apache / Tomcat`
`Windows Server` `Root & Intermediate CA Management` `Production Troubleshooting`

---

*Part of my [technical support & deployment operations portfolio](https://github.com/Charmarke1/charmarke1).*
