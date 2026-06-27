# SEAK Solutions — Website

**Live at: [seaksolutions.dev](https://seaksolutions.dev)**

This is the source for my personal portfolio and business front — SeaK Solutions, by Sean Kollmeier. It tells the story of my homelab, my skills, my photography, and my background, and it doubles as a contact point for both job opportunities and freelance IT help (setups, troubleshooting, the kind of work I'd do for a neighbor).

---

## Hosted on Ironhide

This site doesn't live on a cloud provider. It's served from **Ironhide**, the Proxmox homelab server I built from secondhand parts and documented from the ground up.

**→ [github.com/SeanKoal/ironhide-homelab](https://github.com/SeanKoal/ironhide-homelab)**

Specifically, it runs on the Ubuntu Server VM inside Ironhide, served by Nginx, and made publicly reachable through a Cloudflare Tunnel — no open ports on my home router. If you want to see the actual machine behind this site — the hardware, the hypervisor, the networking, the bugs I fought to get it all running — that's the repo to read.

---

## Stack

No framework, no static site generator, no build step. Every page is hand-coded HTML, CSS, and a small amount of vanilla JavaScript, deliberately — this is a portfolio about infrastructure and security, and I wanted the code itself to be something a recruiter or hiring manager could open and actually read.

- **HTML/CSS/JS** — hand-written, no dependencies
- **Fonts** — Space Grotesk (headings), Inter (body), JetBrains Mono (labels/data), via Google Fonts
- **Forms** — contact form submissions handled by [Formspree](https://formspree.io), no custom backend
- **Design language** — dark, pine-green and copper palette; monospace used deliberately as a design choice, not a default, to echo the terminal/config-file world the homelab content lives in

## Architecture

```
Visitor
   │
   ▼
Cloudflare (DNS + TLS termination + HTTPS enforcement)
   │
   ▼
Cloudflare Tunnel (cloudflared, running as a systemd service on the VM)
   │
   ▼
Nginx (Ubuntu Server VM, inside Ironhide)
   │
   ▼
Static files (this repo)
```

No inbound ports are opened on my router. The Ubuntu Server VM makes an outbound connection to Cloudflare; Cloudflare proxies public traffic back through that tunnel. This is the same approach I'd have wanted as a kid running Minecraft servers off port-forwarding — except this time it doesn't expose anything directly to the internet.

## Security

Since this server is genuinely public now, it's hardened accordingly:

- **HTTPS enforced** at the Cloudflare edge (Always Use HTTPS, Automatic HTTPS Rewrites), minimum TLS 1.2
- **UFW firewall** on the VM — only SSH, HTTP, and HTTPS allowed; everything else dropped by default
- **Fail2ban** watching SSH — repeated failed logins trigger a temporary ban
- **Root login over SSH disabled** — only my own user account can authenticate
- **Unattended-upgrades** enabled — security patches apply automatically, not just when I remember to run `apt upgrade`

Still on the list: switching SSH from password auth to key-based auth.

## Structure

```
seak-solutions-website/
├── index.html          (home)
├── homelab.html        (the Ironhide story)
├── skills.html         (what I actually know, with proof)
├── background.html     (how I got here)
├── contact.html         (hiring or IT help)
├── photography.html     (placeholder — coming soon)
└── css/
    ├── style.css        (shared base — nav, buttons, typography, footer)
    ├── home.css
    ├── homelab.css
    ├── skills.css
    ├── background.css
    └── contact.css
```

One shared stylesheet, one small page-specific stylesheet per page — kept separate so a future style change doesn't mean hunting through a single giant CSS file.

## Status

- ✅ All core pages live and linked
- ✅ Public, secure, served over HTTPS via Cloudflare Tunnel
- ✅ Contact form wired to a real inbox
- ✅ Server hardened (firewall, fail2ban, SSH, auto-updates)
- ⏳ Photography page — pending real content (Canon R100 competition work, a small vintage camera collection, and a growing run with film)
- ⏳ SSH key-based authentication

---

*Built by [Sean Kollmeier](https://seaksolutions.dev) — Assistant Store Manager, student, and the person who built the network, the lab, and this site.*
