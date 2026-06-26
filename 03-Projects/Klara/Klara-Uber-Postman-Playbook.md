# Klara — Uber Guest Rides in Postman

## What the Klara notes say

I read these notes in the Jarvis Obsidian vault:

- `03-Projects/Klara/Klara.md`
- `03-Projects/Klara/Klara-Build-Plan.md`

The project is spelled **Klara** in the vault, not Clara.

Uber is planned as a **Phase 2 / post-MVP tool**, not part of the first core build. Phase 1 is phone calls, memory, family WhatsApp summaries, reminders, onboarding, and Stripe. Phase 2 adds high-utility external APIs: ÖBB, Uber, weather, package tracking, and similar services.

The relevant Klara behavior is:

- Klara can order an Uber for a user.
- It must always use an explicit confirmation step before any financially binding action.
- The intended voice flow is: user asks for ride → Klara estimates/clarifies → Klara says something like “Soll ich das Uber wirklich bestellen? Es kostet ungefähr €12.” → user confirms → Klara books.
- Uber Developer Access should be requested early because approval may take weeks.
- In the build plan, “Uber API Booking” is listed after real user validation and after the first simpler tool-calls.

## What Postman is useful for here

Postman should be your **API rehearsal room** before you wire Uber into Klara’s backend.

Use it to:

1. Understand the Uber Guest Rides API request sequence.
2. Store sandbox and production credentials separately.
3. Generate and test access tokens.
4. Run ride-estimate, ride-create, ride-status, modify, cancel, and receipt-style requests manually.
5. Learn the exact request/response payloads your FastAPI backend will need.
6. Build a working “happy path” before adding GPT-Realtime function calling.
7. Test failure cases safely in sandbox: bad address, unavailable ride, cancellation, missing guest data, expired token.

## Recommended Postman setup

### 1. Create a Postman workspace

Create a workspace named something like:

`Klara — Uber Guest Rides`

Inside it, keep:

- Uber Guest Rides API — Sandbox
- Uber Guest Rides API — Production
- Klara Backend API, later, for your own FastAPI endpoints

### 2. Use Uber’s official Postman collection

Start from Uber’s official Postman collection for **Guest Rides API — Sandbox**. The screenshot you were looking at is already the right kind of page.

The collection contains folders like:

- Authorization
- Run & Zones
- Trip Estimate
- Create Trip
- Change Driver State
- List & Get Trips
- Cancel Trip
- Scenarios
- Location Refinement
- Guest Details

For Klara, the important early folders are:

- Authorization
- Run & Zones
- Trip Estimate
- Create Trip
- List & Get Trips
- Cancel Trip

### 3. Create environments

Create two Postman environments:

`Uber Guest Rides — Sandbox`

Variables:

- `base_url` = sandbox Uber API base URL from the collection/docs
- `client_id`
- `client_secret`
- `scope`
- `access_token`
- `run_id` or sandbox run UUID, if required by the sandbox flow
- `request_id`
- test pickup latitude / longitude
- test dropoff latitude / longitude
- test guest first name
- test guest last name
- test guest phone number

`Uber Guest Rides — Production`

Variables:

- `base_url` = production Uber API base URL
- `client_id`
- `client_secret`
- `scope`
- `access_token`
- real organization / guest ride config values from Uber approval

Keep production credentials out of screenshots, notes, and shared exports.

## Authentication setup

Uber Guest Rides uses OAuth-style server-to-server authentication. In the current Uber/Postman docs, the Postman collection says credentials include `client_id` and `client_secret`, which are exchanged for a short-lived access token. Protected Guest Rides calls then use an `Authorization: Bearer <token>` header.

Important scope note:

- Uber’s official developer introduction currently mentions `guest.rides`.
- Uber’s Postman collection and several endpoint pages currently mention `guests.trips`.

Before coding, verify the exact scope in the Uber Developer dashboard or the collection you have access to. If token generation fails, this scope mismatch is one of the first things to check.

In Postman:

1. Open the collection.
2. Select the sandbox environment.
3. Go to the Authorization tab or the “Generate Access Token” request.
4. Enter `client_id`, `client_secret`, and the required scope.
5. Click “Get Access Token”.
6. Click “Use Token”.
7. Run a simple protected request to confirm the token works.

## Sandbox request sequence for Klara

For Klara’s use case, test this sequence manually:

### A. Start sandbox run / zones if required

The sandbox docs say the `/run` call can return a `run_id`, which is then sent in later sandbox calls as a header like `x-uber-sandbox-runuuid`.

Use this to simulate trip states without creating real rides.

### B. Estimate a trip

This is the most important request for Klara’s confirmation flow.

Test input:

- pickup location
- destination location
- guest details if required
- product / ride type if required

Klara needs the response to say something user-friendly, for example:

“Die Fahrt dauert ungefähr 18 Minuten und kostet ungefähr 12 bis 15 Euro. Soll ich sie wirklich bestellen?”

### C. Create a trip

Only after explicit confirmation.

In the real Klara backend, this should never happen directly from a fuzzy voice intent. It should happen only after your backend has stored a pending ride quote and the user confirms it.

### D. Get trip status

Use this to learn how to report back:

- ride requested
- driver assigned
- driver arriving
- trip active
- completed
- canceled / failed

### E. Cancel trip

Test cancellation paths and timing.

Klara needs a safe phrase like:

“Die Fahrt ist bestellt. Wenn du möchtest, kann ich sie jetzt stornieren.”

### F. Error cases

Test these in Postman before coding:

- token expired
- invalid scope
- missing guest phone number
- invalid pickup/dropoff
- no vehicle available
- duplicate request
- cancellation too late
- sandbox run ID missing

## How this maps into Klara’s backend

Eventually, Postman tests turn into FastAPI functions:

- `estimate_uber_ride(pickup, dropoff, guest)`
- `create_uber_ride(quote_id_or_payload, confirmed_by_user)`
- `get_uber_ride_status(request_id)`
- `cancel_uber_ride(request_id)`

The Realtime model should not call Uber directly. It should call your backend tool functions. The backend should enforce the confirmation rule even if the model makes a mistake.

Recommended safety architecture:

1. Voice model detects ride intent.
2. Backend calls estimate only.
3. Backend stores a pending ride with price/ETA/request draft.
4. Klara asks for explicit confirmation.
5. User says yes.
6. Backend checks `confirmed_by_user = true`.
7. Backend creates the ride.
8. Klara reports status by voice and optionally WhatsApp.

## Missing details from the notes

The Klara notes do not yet specify these required API details:

- Whether Uber Developer Access has already been approved.
- Exact Uber API product: Guest Rides appears likely, but confirm this in the Uber Developer dashboard.
- Exact scope: `guest.rides` vs `guests.trips` needs verification against the approved app/collection.
- Sandbox and production base URLs to use in your account.
- Client ID and client secret.
- Whether Uber requires callback/webhook URLs for your approved integration.
- Whether Klara will pay via your Uber for Business organization or another billing setup.
- What guest phone number format Uber requires for DACH users.
- Which ride products are allowed in Austria / DACH for Guest Rides.
- Whether you need scheduled rides or only immediate rides.
- How to handle cancellation fees and user consent.
- How to handle accessibility needs for older users.
- Whether family members should receive WhatsApp ride confirmations.

## Practical next move

Do this next:

1. Apply for or check Uber Developer / Guest Rides access.
2. Fork/import the official Uber Guest Rides Sandbox Postman collection.
3. Create the sandbox environment with placeholder variables.
4. Generate an access token.
5. Run the sandbox scenario flow end-to-end.
6. Save one successful estimate/create/status/cancel sequence.
7. Only then implement the matching FastAPI wrapper functions.

Postman is not the final product. It is the place where you learn and prove the Uber workflow before connecting it to Klara’s voice agent.
