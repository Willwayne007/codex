# Ela Perdeu Interesse?

Modern mobile-first MVP for analyzing romantic conversations from WhatsApp, Instagram and Tinder.

The app lets a visitor paste a conversation and receive:

- Interest score
- Attraction level
- Emotional tone
- Confidence mistakes
- Response quality
- Probability of date
- Best next message
- Flirting improvement tips

## Stack

- Next.js 15 App Router
- TypeScript
- TailwindCSS
- OpenAI API
- Supabase
- Stripe-ready mock checkout
- Vercel-ready deployment

## Local setup

1. Install dependencies:

```bash
npm install
```

2. Copy environment variables:

```bash
cp .env.example .env.local
```

3. Fill the keys:

```bash
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
OPENAI_API_KEY=your_openai_key
OPENAI_MODEL=gpt-4o-mini
NEXT_PUBLIC_STRIPE_PRICE_ID=price_xxx
```

4. Run the Supabase SQL in `supabase/schema.sql`.

5. Start development:

```bash
npm run dev
```

Open `http://localhost:3000`.

## OpenAI behavior

The API route lives at `app/api/analyze/route.ts`.

In development, if `OPENAI_API_KEY` is missing, the app returns a polished fallback result so the UI can still be tested instantly. In production, the API returns a clear server configuration error when `OPENAI_API_KEY` is missing.

With the key present, it calls the configured `OPENAI_MODEL` and requests strict JSON with:

- `interestScore`
- `detectedSignals`
- `mainMistake`
- `recommendedAction`
- `suggestedMessage`
- `summary`

The API includes an IP-based free usage limit of 3 analyses per 24 hours. For very high traffic or multi-region deployment, replace the in-memory limiter in `lib/rate-limit.ts` with a shared store such as Redis, Upstash or Supabase RPC.

The frontend includes analytics placeholders for:

- `analysis_started`
- `analysis_completed`
- `paywall_viewed`
- `checkout_clicked`

## Supabase behavior

Supabase stores:

- `profiles`: future optional Google login users
- `analyses`: conversation previews and analysis result JSON
- `subscriptions`: Stripe-ready subscription records

Free usage is tracked by `anonymous_id`. The MVP allows one free analysis per anonymous visitor unless `plan` is `premium`.

## Checkout

`app/api/checkout/route.ts` is intentionally mocked. Replace it with Stripe Checkout Session creation when you are ready to charge:

- Read `NEXT_PUBLIC_STRIPE_PRICE_ID`
- Create a Stripe customer or reuse one
- Create a subscription checkout session
- Return `session.url`
- Handle webhooks to update `subscriptions`

## Vercel deployment

1. Push this project to GitHub.
2. Import the repo in Vercel.
3. Add the environment variables from `.env.example`.
4. Set `NEXT_PUBLIC_APP_URL` to the production URL.
5. Deploy.

## SEO targets

The app metadata is optimized for:

- dating AI
- relationship AI
- conversation analyzer
- tinder assistant
- whatsapp AI analysis
- analisador de conversa
- ela perdeu interesse

## Product notes

The landing page is built for paid social traffic: clear emotional promise, instant paste flow, viral share, example conversations, fake live activity counter, mobile-first result cards and a low-friction premium upgrade path.
