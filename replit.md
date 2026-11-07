# SK3LE WATERMARK BOT

A Telegram Mini App and bot storefront for SK3LE digital watermarks, voiceovers, edit audios, and profile pictures, with Telegram Stars checkout.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the shared API server (port 5000)
- `pnpm --filter @workspace/sk3le-watermark-bot run dev` — run the storefront
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `BOT_TOKEN` is optional for local storefront use; set it to activate the Telegraf bot and Telegram Stars checkout.
- `ADMIN_PIN` enables browser-preview owner access. Without it, the Mini App verifies @sk3lefake through signed Telegram WebApp init data.
- Set `MINI_APP_URL` to the deployed HTTPS storefront URL so `/start` opens the Mini App.
- `OWNER_TELEGRAM_ID` is optional; otherwise @sk3lefake should start the bot once so it can save the private chat ID for payment notices.
- `SESSION_SECRET` signs admin sessions and invoice payloads.

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- Store UI: `artifacts/sk3le-watermark-bot/src/`
- Store API, Telegram bot, and uploaded media: `artifacts/api-server/src/` and `artifacts/api-server/public/`
- Product catalog: `artifacts/api-server/public/products.json`
- API contract: `lib/api-spec/openapi.yaml`

## Architecture decisions

- Product metadata is stored as JSON and uploaded bytes are written to `public/uploads`, matching the requested file layout.
- Admin sessions are signed server-side; the browser PIN is optional, while Telegram admin access requires validated WebApp init data.
- The server starts normally without `BOT_TOKEN`; Telegram-only checkout remains unavailable until configured.

## Product

- EN/RU storefront with media previews and category filtering.
- Owner-only product upload and deletion.
- Telegram Stars invoice creation and payment notifications.

## User preferences

- Keep the visual theme dark purple with glowing violet accents and an English/Russian toggle.

## Gotchas

- Payment notifications require the bot to know the owner's private chat ID; @sk3lefake must send `/start` once unless `OWNER_TELEGRAM_ID` is configured.
- Uploaded files and `products.json` are on the app filesystem as requested; use durable object storage before relying on them across rebuilds or replicas.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
