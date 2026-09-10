# Northhh — Portfolio

Personal portfolio for **Benedict Patrick (Northhh)** — full stack developer, beatmaker, and cybersecurity practitioner. Built and maintained entirely from a mobile phone, no laptop or desktop involved at any stage.

Live: [northhh-dev.vercel.app](https://northhh-dev.vercel.app)

## What this is

A static portfolio site with a live, self-authored content system layered on top — **Northhh Labs** — so individual projects can have full dynamic case studies without hand-editing `index.html` every time.

## Structure

```
index.html          → main portfolio page (project cards, about, contact)
case-study.html      → public, dynamic case-study viewer (?p=<slug>)
admin.html           → private admin panel to add/edit/publish case studies
epk.html             → press kit page
og-image.png         → social preview image
```

## Stack

- **Frontend:** single-file HTML/CSS/JS, no build step
- **Hosting:** Vercel
- **Case-study data:** Firebase Firestore (project: `northhh-dev-72fad`)
- **Auth:** Firebase Auth (email/password), gates `admin.html` to a single admin UID
- **Screenshots/assets:** Cloudinary (not Firebase Storage — avoids the Blaze billing requirement)

## How the case-study system works

1. `admin.html` — sign in, fill out a project (title, problem, what-was-built, stack, decisions, challenges, screenshot URLs, live/GitHub links), toggle **Published**, save. Writes straight to Firestore.
2. Each project card on `index.html` links to `case-study.html?p=<slug>`, which reads that project's Firestore document and renders it.
3. Firestore security rules only expose documents where `published: true` to the public; the admin account can read/write everything, including drafts.

Adding a new project's case-study link to a card is one line:

```html
<a href="/case-study.html?p=your-slug" class="proj-link">VIEW CASE STUDY ↗</a>
```

## Design

Dusk/terminal aesthetic — `Big Shoulders Display` for headings, `JetBrains Mono` for body and UI text. The landing page itself has no Firebase dependency, so it stays fast and resilient; only the case-study and admin pages talk to Firestore.

## Deployment

Push to `main` — Vercel auto-deploys. No environment variables or build step required; Firebase config is embedded client-side (protected by Firestore security rules, not by hiding the config).
