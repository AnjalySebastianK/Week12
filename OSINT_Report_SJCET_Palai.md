# Open Source Intelligence Report - sjcetpalai.ac.in

**Author:** Anjaly Sebastian  
**Date:** 28 October 2025  
**Target:** sjcetpalai.ac.in  

---

## Summary
This report summarizes passive OSINT reconnaissance performed against **sjcetpalai.ac.in**. All collection was passive (WHOIS lookups, DNS footprinting, Subdomain enumeration, Certificate Transparency logs, Shodan, Google Dorks and robots/header inspection). No active scans, exploitation, or credential testing was performed.

**Key takeaways (top-level)** for sjcetpalai.ac.in:

- Root domain resolves to multiple IPs, mainly hosted by Contabo (e.g., 75.119.129.151).
- 30+ public subdomains discovered, covering library, admissions, boot, mail, testing, and integrations with GitHub and Vercel.
- Certificate Transparency logs show valid Let's Encrypt SSL certificates; reveal presence of subdomains like login.sjcetpalai.ac.in.
- Main website runs on Apache (Debian Linux); WordPress, Bootstrap, and JQuery detected.
- Email handled by Google Workspace, MX records point to Google; security headers like X-Frame-Options: DENY enable basic protection.
- Several third-party services and cloud assets are linked, increasing the attack surface if abandoned or misconfigured.
- Many subdomains, external providers, and integrations mean asset monitoring and patching are critical for ongoing security.
---
## Methodology (detailed)
1. **WHOIS lookup:** Queried WHOIS to confirm registrar (ERNET India), DNS provider, and noted registrant privacy controls. 
2. **DNS Enumeration (DIG & Amass):** Used DIG and Amass to collect A, AAAA, MX, NS, CNAME records. Enumerated the root domain, mail server (Google), and over 30 subdomains. 
3. **Certificate Transparency (crt.sh):** Queried crt.sh for all certificates issued to the target, revealing additional subdomains and validating the use of Let's Encrypt SSL/TLS certificates. 
4. **Web Scanning (WhatWeb):** Detected core technologies (Apache, Debian, WordPress, Bootstrap, jQuery) and documented security-relevant HTTP headers. 
5. **Internet Exposure (Shodan):** Checked Shodan for open ports, banner leaks, and exposure metadata for all related public IP addresses. 
6. **Third-party & Service Mapping:** Identified dependencies on GitHub, Vercel, Google, and other providers via CNAME and SSL data. 
7. **robots.txt Inspection:** Downloaded and analyzed robots.txt to note site owner’s indexing preferences and identify any directories intended to be hidden from search engines—but potentially interesting for attackers. 
8. **HTTP Header Analysis:** Recorded server response headers (e.g., X-Frame-Options, X-UA-Compatible, content-security-policy) to assess presence of basic security mechanisms and identify risky defaults. 
9. **Google Dorks Search:** Conducted focused Google searches to uncover exposed files, hidden/test directories, login/admin panels, config leaks, sensitive documents (PDF, .env, .xls, .sql, .phpinfo, backups) and potential misconfigurations indexed by Google. 
10. **Screenshot & Documentation:** Archived all commands, screenshots, web scan outputs, and crt.sh records systematically for report evidence and traceability. 
---

## Recon Commands & How to Reproduce

> Run these from a terminal (replacing host/domain as needed). All are passive.

### WHOIS
```bash 
whois sjcetpalai.ac.in
whois -h whois.nixiregistry.in sjcetpalai.ac.in
``` 
### DNS
```bash
dig sjcetpalai.ac.in ANY
amass enum -d sjcetpalai.ac.in
whatweb sjcetpalai.ac.in
```

### Certificate Transparency
Open:
```
https://crt.sh/?q=sjcetpalai.ac.in
```

### Shodan
```bash
https://www.shodan.io/host/75.119.129.151
```
### Google Dorks (examples)
```
site:sjcetpalai.ac.in
site:sjcetpalai.ac.in filetype:pdf
site:sjcetpalai.ac.in ext:env
site:sjcetpalai.ac.in inurl:login
site:sjcetpalai.ac.in "password"
site:sjcetpalai.ac.in filetype:xls OR filetype:xlsx
site:sjcetpalai.ac.in intitle:index.of
```
### Robots/Headers
```bash
curl -I https://sjcetpalai.ac.in
curl -s https://sjcetpalai.ac.in/robots.txt
```
---

## Findings

1. **WHOIS** 
- Domain Name: sjcetpalai.ac.in
- Registrar: ERNET India
- Registar URL: https://www.ernet.in/
- Domain Status: ok (https://icann.org/epp#ok)
- Domain Creation Date: 2006-03-29T09:15:12.996Z
- Last Updated Date: 2024-03-16T08:29:09.975Z
- Registry Expiry Date: 2033-03-29T09:15:12.996Z
- Registrant, Admin, and Tech contact details (Name, Organization, Address, Phone, Email, Fax) are redacted for privacy.
- Notes: Most personal and organizational details are protected by privacy settings and redacted. Registration, update, and expiry dates are visible. Technical/ownership confirmation can be sought directly via the registrar.

2. **DNS**
- MX Record (Mail Exchange): 5 alt1.aspmx.l.google.com.
- NS Record (Name Servers): ns1.contabo.net, ns2.contabo.net, ns3.contabo.net
- A Record (IPv4 address): 75.119.129.151
- SOA Record (Start of Authority):  ns1.contabo.net hostmaster.contabo.de 2025101301 3600 7200 2419200 10800
- Notes: The domain uses Google for email. Name service is managed by Contabo.net. The site resolves to IPv4 75.119.129.151. SOA record indicates administrative DNS details managed via Contabo.net. No suspicious DNS records appear based on this output—the DNS setup is typical for academic domains.

3. **Certificate Transparency**
- crt.sh ID: 20126231361
- Type: Precertificate (certificate to be finalized)
- Timestamps: Logged on August 5, 2025, by Google, Sectigo, and Geomys CT log operators.
- Log URLs: Google, Sectigo, Geomys
- OCSP/CRL Providers: Google, Sectigo, Microsoft, Mozilla.
- Status: Not Revoked across all providers (certificate considered valid).
- Notes: The certificate for login.sjcetpalai.ac.in is logged in public certificate transparency records, improving accountability and allowing anyone to audit SSL/TLS deployments. The certificate is valid, not revoked, and issued by Let's Encrypt, a respected CA. The short validity (about 90 days) is normal for Let's Encrypt certificates, indicating best practices. Public key info and certificate fingerprints enable cryptographic verification. No detected revocation, expiry is in November 2025.

4. **Amass Subdomain & Asset Enumeration**
- Multiple Subdomains Identified:
  ```
  admap, melms, gms, minor, lib, opac, boot, login, gnsmart, elective and more

  ```
- A Record (IPv4): Multiple IP addresses including 75.119.155.105, 117.254.186.102, 164.52.206.88, 109.205.183.47, 75.129.255.105, 158.220.104.179, 75.119.145.61.
- AAAA Record (IPv6): Numerous IPv6 addresses such as 2a02:c206:2130:9755::1, 2a02:c206:2130:9755::1, 2a02:c205:0:8911::1.
- MX Record: Email is handled via Google (alt1.aspmx.l.google.com, aspmx.l.google.com). 
- CNAME Record: Many subdomains point to external services like vercel (cname.vercel-dns.com), github (tedxsjcet.github.io), and Google (aspmx.l.google.com).
- Third-party Associations: Subdomains connected to external platforms like GitHub, Google, and Vercel, indicating integrations and possible attack vectors via third-party services.
- Multiple IP Ranges & Providers: Academic infrastructure hosted across different IP space and providers (Contabo, NetMagic, Amazon, Google).
- Notes: The report outlines the variety and breadth of the organization’s digital assets, highlighting points for future vulnerability assessment. Potential entry points for attackers if any subdomain is unpatched or misconfigured.  CNAMEs pointing to external services can be risky if left abandoned or misconfigured (takeover scenarios). AAAA records show modern infrastructure support but should also be monitored for vulnerabilities.

5. **Whatweb**
- Running on Apache 2.4.56.
- Operating System is Debian Linux.
- Hosting IP is 75.119.129.151.
- The server is located in the United States (US).
- The site automatically redirects from http://sjcetpalai.ac.in to https://sjcetpalai.ac.in (uses HTTPS for security).
- Web Technologies Detected: Apache web server, Bootstrap CSS framework, jQuery 6.11.1 JavaScript library, EventOn 2.6.3 (probably for event/calendar features), WordPress 6.2.8 (content management system for building websites and blogs), LayerSlider 6.11.1 (used for website sliders and animation).
- Uses security headers like X-Frame-Options: DENY (protects from clickjacking).
- X-UA-Compatible: IE=edge (render compatibility for browsers).
- Notes: The website runs WordPress (often targeted by attackers if plugins/themes are outdated). Security headers are present, which is good practice. Uses common open-source tech (Apache, Debian, jQuery, Bootstrap)—must be updated to avoid known vulnerabilities. Plugins and website themes should also be kept up to date.

6. **Google Dorks**
- Used targeted Google searches such as `site:sjcetpalai.ac.in filetype:xlsx` to uncover publicly accessible files.
- Discovered an exposed Excel (.xlsx) file containing student names and email addresses.
- This highlights a real-world data leak, confirming that confidential student information is discoverable and downloadable by anyone on the internet.
- Such leaked files pose privacy risks and could be used for phishing, spam, or social engineering attacks.

7. **Shodan**
- Target IP: 75.119.129.151
- Last Seen: 2025-10-27
- Location: Düsseldorf, Germany
- Hosting/ISP: Contabo GmbH
- Organization: Wavetech Systems, LLC
- Detected Hostname: vmi1309755.contaboserver.net
- Domains Associated: contaboserver.net
- ASN: AS51167
- Operating System: Debian GNU/Linux
- Open Ports: 22, 80, 443, 10000

8. **robots.txt and headers**
- User-agent: * is a wildcard, meaning these rules apply to all web crawlers/bots (Googlebot, Bingbot, etc.).
- Disallow: Crawlers are told not to access anything under /wp-admin/.
- Allow: Even though /wp-admin/ is disallowed, admin-ajax.php is allowed.
- Sitemap: https://sjcetpalai.ac.in/wp-sitemap.xml

## Legal & Ethical Notice
This report contains **only passive**, publicly available data. Do not perform any intrusive testing or unauthorized access. Unauthorized exploitation of systems or data is illegal.

---
