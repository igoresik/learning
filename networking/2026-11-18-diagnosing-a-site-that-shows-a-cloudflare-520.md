<sub><a href="../README.md">Back to the notebook</a></sub>

# Diagnosing a site that shows Cloudflare error 520, and working out who controls what

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-networking-dark.svg"><img src="../assets/tag-networking.svg" alt="Networking" height="22"></picture></a>&nbsp; **Date:** 2026-11-18

## What I built or changed

- A public website I help with started showing "Web server is returning an unknown error" (Cloudflare error 520) on its main address, while another address on the same domain kept working.
- Checked it from the command line with `curl -I` on each address, and looked up the DNS records with `nslookup` (address, name servers, MX and SPF records).
- Used that to explain the problem to the owner and to the person who looks after the hosting, and to ask them the specific questions that would settle it: which server the records point to, whether it is running, and whether a firewall blocks Cloudflare.
- Looked at my own Cloudflare accounts through the command-line tool and the API to confirm the domain is not in either of them, so I can't change its DNS myself.
- Spotted that the domain's mail is hosted separately from the website (MX records point to a hosted mail provider), so a planned move of the mailboxes must not touch those records without checking.
- Drafted the follow-up emails to the hosting contact and the mail consultant, polite and specific, and rewrote them after review.

## What I learned

- A 520 means Cloudflare reached out but the origin server dropped or garbled the connection. Cloudflare itself is working, so the fix is at the origin: a stopped server, a wrong record, or a firewall blocking Cloudflare's addresses.
- `curl -I` on every address tells you quickly whether the problem is the whole domain or one name, and `nslookup` shows whether a domain goes through Cloudflare (Cloudflare addresses and name servers).
- A website and a mailbox on the same domain can live on different providers. The MX and SPF records show where mail goes, and changing them moves the mail.
- Having a login to a tool does not mean the domain is in it. Listing the zones in an account shows what you can really change.
- Most of the work in an outage like this is working out who owns each piece and asking them clearly. A precise question gets a precise answer faster than "it's down".
- Don't guess a start time for an outage. I wrote "since yesterday" because the earliest proof was a screenshot from the first day.

## Tools used

- `curl`, `nslookup`
- Cloudflare dashboard, API and command-line tool (read-only checks)
- Gmail

## Link to the project repo

- Not a code project, nothing to link.
