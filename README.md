# 100+ Bug Bounty Tips

A structured, readable version of the **bounty_tips_100+** slide deck — **116 slides** turned into a searchable Markdown reference, grouped by vulnerability class.

> ⚠️ **For education and authorised testing only.** Every technique here should only be used against systems you own or have explicit written permission to test.

## What's inside

- **97 unique tips**, 19 duplicate slides merged
- **15 categories** — from recon and subdomain enumeration to auth bypass, injection, SSRF, cloud, mobile, and business logic
- Every slide transcribed with its payloads, commands, HTTP requests, and links preserved
- A short **Why it matters** note per tip

## Read it

👉 **[Open the full collection: `tips.md`](tips.md)**

## Categories

| Category | Tips | Jump |
|----------|-----:|------|
| Methodology & Mindset | 9 | [Browse](tips.md#methodology--mindset) |
| Recon & OSINT | 15 | [Browse](tips.md#recon--osint) |
| Subdomains & DNS | 3 | [Browse](tips.md#subdomains--dns) |
| Scanning & Tooling | 8 | [Browse](tips.md#scanning--tooling) |
| Authentication & Session | 13 | [Browse](tips.md#authentication--session) |
| Authorization & IDOR | 6 | [Browse](tips.md#authorization--idor) |
| Injection | 18 | [Browse](tips.md#injection) |
| SSRF & Network | 4 | [Browse](tips.md#ssrf--network) |
| File Upload & Traversal | 4 | [Browse](tips.md#file-upload--traversal) |
| API & GraphQL | 2 | [Browse](tips.md#api--graphql) |
| Cloud & Storage | 1 | [Browse](tips.md#cloud--storage) |
| Client-Side | 7 | [Browse](tips.md#client-side) |
| Business Logic | 3 | [Browse](tips.md#business-logic) |
| Caching & Smuggling | 2 | [Browse](tips.md#caching--smuggling) |
| Mobile | 2 | [Browse](tips.md#mobile) |

## Highlights

- SSRF Search & Destroy mind map (bypasses, DNS tricks, cloud metadata, gopher)
- Reflected XSS approach map — from reflection discovery to filter bypass
- AWS penetration testing checklist (41 checks)
- Endpoint/403 bypass methodology with 11 worked path mutations
- Password-reset and 2FA bypass playbooks
- Patrik's bug bounty tool map and Burp extension lists
- Google/GitHub dork collections and recon one-liners

## Using it

```bash
git clone <your-repo-url>
cd <repo>
# open tips.md in any Markdown viewer, or grep it:
grep -i 'ssrf' tips.md
```

## Structure

```text
.
├── README.md   # this file: overview + category index
└── tips.md     # the full reference, grouped by category
```

## Credits & notes

- Source deck: `bounty_tips_100+.pdf`; individual slides credit their authors where shown (e.g. `@SalahHasoneh1`, `@hackerScrolls`, `@trbughunters`, `@offensivekrish`, `@kamranahmedse`).
- Slide numbers are preserved in each tip's metadata so you can trace every entry back to the original deck.
- Payloads and commands are reproduced as shown on the slides, including a few that are truncated in the original. Unreadable text is flagged with inline `<!-- unclear -->` comments rather than guessed.
