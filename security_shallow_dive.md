# Security - How to make safe-ish web applications

I tend to believe that a lot of security is about knowing what is actually happening, rather than relying on what you believe should be happening...

One good place to start is the OWASP Top 10 <https://owasp.org/www-project-top-ten/> and The OWASP Testing Project <https://github.com/OWASP/wstg/tree/master/document>

## Basic safeguards

- https & certificate checking enabled by default
- web application firewall (properly configured!)
- DDoS mitigation (e.g. cloudflare)
- dependency management (keep them up to date, and as few as possible) and monitoring (e.g. Snyk)
- declare acceptable inputs to e.g. forms (Rails has the strong params option?) & regexes to guard against sql injection, regex-based ddos etc
- use whatever your framework provides to guard against XSS and CSRF
- learn about CORS (by default it's enforced by the browser, so curl/postman etc can bypass it, but can be disabled for non-browser clients too)
- use request throttling
- keep configuration and code separate (secrets in environment variables or similar)
- logging & monitoring & observability (Honeycomb.io is supposed to be good for this)
- limit user access to the minimum required (including developer users!)
- 2FA

## Additional thoughts

- test your backup and incident response process regularly
- you need a plan/playbook so that everyone knows what to do if a breach is detected
- depending on your industry you may have a legal requirement to report breaches
- graphQL provides a broad attack surface, as everything is linked by default: you must check authentication/authorisation at each step
- regular reminders of security hygiene (password length, 2FA, unscoped 'where' queries etc)
- don't roll your own security/authentication/encryption solution (BCrypt is decent)
