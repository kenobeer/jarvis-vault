## 2026-04-10 · Vibe B. — Kickoff Session

**Summary:** Full product definition session for Vibe B., a curated Vienna venue marketplace. Evolved from an earlier "Event-Stack" concept (full-service events OS) to a focused two-sided marketplace for bookable spaces. Designed the complete landing page — 10 sections — and mapped the technical MVP roadmap from landing page to working web app.

**Decisions:**
- Business model locked: 12% host commission, zero guest fees, Stripe Connect
- Design system locked: Warm Minimalism, Cormorant Garamond + DM Sans, plum + green palette
- Landing page structure finalized: 10 sections covering full customer journey from arrival to email capture
- Tech stack decided: React + Vite + Supabase + Stripe Connect + LLM matching via API
- MVP phasing: 5 phases, ~3 months total to working product
- Cursor prompt written for full landing page build

**Open threads:**
- No real Vienna venues onboarded yet — this is the actual bottleneck
- LLM matching engine not built — keyword fallback first, then API
- Stripe Connect not integrated
- No auth system yet
- Need to decide: build host onboarding flow before or after first 15 manual listings?

## 2026-04-17 · Vibe B. — Pitch Prep Session

**Summary:** Full investor pitch preparation for Friday. Built the complete investment proposal (€35K convertible note, Cap €800K, 20% discount), a stichpunktartiger one-pager for investors, an 8-slide 5-minute pitch structure with slide logic, and the final English scripts for each slide. Investors researched: Markus Aumer (ex-Round2, Aumer Advisory, WU-Alumni), Matthias Hofreiter (Gateway Ventures Wien), Benjamin Delic, Thomas Plank. Context: Angel/Pre-Seed round in the WU ecosystem, not institutional.

**Decisions:**
- Instrument: Wandeldarlehen (Convertible Note) — avoids valuation discussion at this stage
- Ask: €35.000 — covers FlexCo founding, legal docs, supply acquisition, tech infra, working capital
- Pitch language: English
- Opening slide: visual hook (rooftop photo), no intro slide, no agenda slide
- Core moat argument: inventory, not AI — "other platforms can copy the interface, not the supply"
- Closing line: "I'm looking forward to the hard questions" — signals preparation to Aumer specifically
- Pitch script saved to: [[03-AI/Vibe-B/pitch-script-EN]]

**Open threads:**
- No supply-side LOIs yet — critical to get at least one informal yes before Friday
- Team role division not yet formalized — investors will ask
- No traction/bookings — lean into capital efficiency and in-house build as counter
