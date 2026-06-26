---
created: '2026-04-10'
status: active
tags:
  - vibe-b
  - startup
  - wien
  - marketplace
type: memory
---
# Vibe B. — Permanent Record

Curated venue marketplace for Vienna. Airbnb for events — rooftops, lofts, studios, gardens.

## Core concept

Intent-based search: user describes event in natural language → system returns exactly 3 curated matches. No filters, no infinite grid. The constraint is the product.

## Positioning

"Don't search for spaces. Tell us what you're planning."

Target: B2B (startup teams, corporates) + B2C (birthdays, private events). Dual audience, single interface.

## Business model

- 12% host-side commission only
- Zero guest fees (vs. Peerspace ~25-30% total take)
- Stripe Connect for split payments + automatic host payouts post-event
- No listing fees, no monthly costs for hosts

## Design system

- Warm Minimalism — boutique hotel aesthetic
- Cormorant Garamond (headings) + DM Sans (body/UI)
- Background `#F4EFE4` · Plum `#2A1728` · Green `#3D6647`
- Cards: 20px radius, 4px hover lift

## Tech stack

- React + Vite (frontend)
- Supabase (Postgres, Auth, Storage, Realtime)
- Stripe Connect (payments + payouts)
- LLM API for matching (prompt → 3 space IDs as JSON)
- Resend (transactional email)
- Beehiiv (newsletter/waitlist)

## Status at last session

- Landing page: MVP-ready in browser, all 10 sections specified
- Cursor prompt: written and saved
- Database: not yet built
- Listings: 0 real venues onboarded
- Payments: not yet integrated

## Critical constraint

Supply before everything. Need 15 confirmed Vienna spaces before testing matching, running ads, or onboarding guests.

**Linked to:** [[Uni/Vibe-B]]
