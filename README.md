# DCD Development — Fitness Tariff Landing Page

A responsive landing page built with **Next.js 12**, **Tailwind CSS**, and **React 18**. It displays fitness subscription plans fetched from an external API, with a countdown timer, interactive plan selection, and a purchase flow.

---

## Tech Stack

- [Next.js 12](https://nextjs.org/) — SSR + file-based routing
- [React 18](https://react.dev/) — UI
- [Tailwind CSS 3](https://tailwindcss.com/) — utility-first styling
- [MUI (Material UI)](https://mui.com/) — `useMediaQuery` for responsive logic
- [SCSS](https://sass-lang.com/) — global base styles
- [Express](https://expressjs.com/) — custom server (`server.js`)
- [Nodemon](https://nodemon.io/) — dev auto-restart

---

## Getting Started

### Prerequisites

- Node.js >= 16
- npm >= 8

### Install

```bash
npm install
```

### Run in development

```bash
npm run dev
```

App runs at `http://localhost:3000`.

### Build for production

```bash
npm run build
npm start
```

> `npm start` reads the `$PORT` environment variable. Set it before running, e.g. `PORT=3000 npm start` (Linux/macOS) or `$env:PORT=3000; npm start` (PowerShell).

---

## Project Structure

```
├── components/
│   ├── base/
│   │   ├── Badge.js          # Discount % badge
│   │   ├── Button.js         # CTA button
│   │   ├── CustomImage.js    # Image wrapper
│   │   ├── CustomText.js     # Text wrapper
│   │   ├── ItemCard.js       # Tariff card (desktop)
│   │   └── ItemCardMobile.js # Tariff card (mobile ≤960px)
│   ├── landing/
│   │   └── Main.js           # Main page content
│   └── layout/
│       ├── Header.js         # Fixed top banner with countdown timer
│       └── Footer.js         # Footer
├── lib/
│   └── tariffs.js            # API fetch + fallback data + mapping logic
├── pages/
│   ├── api/
│   │   └── tariffs.js        # Internal API route proxying the tariffs service
│   ├── _app.js
│   ├── _document.js
│   └── index.js              # Entry page (SSR via getServerSideProps)
├── public/
│   └── images/img.png        # Hero image
├── styles/
│   └── globals.scss          # Tailwind directives + global styles
├── server.js                 # Custom Express server
└── tailwind.config.js
```

---

## Data Source

Tariff plans are fetched from:

```
GET https://t-core.fit-hub.pro/Test/GetTariffs
```

| API field    | Description                        |
|--------------|------------------------------------|
| `period`     | Plan duration label                |
| `price`      | Discounted price                   |
| `full_price` | Original price (before discount)   |
| `is_best`    | Featured/default plan flag         |
| `text`       | Plan description                   |

Discount percentage is **calculated client-side** as `(1 - price / full_price) * 100` — it is not provided by the API.

If the API is unreachable, the app falls back to hardcoded plans defined in `lib/tariffs.js`.

---

## Environment Variables

| Variable              | Default                                      | Description                  |
|-----------------------|----------------------------------------------|------------------------------|
| `TARIFFS_SERVICE_URL` | `https://t-core.fit-hub.pro/Test/GetTariffs` | Override the tariffs API URL |
| `PORT`                | `3000`                                       | Port for production server   |

Create a `.env.local` file in the root to set them locally:

```env
TARIFFS_SERVICE_URL=https://your-api.example.com/tariffs
PORT=3000
```

---

## Key Features

- **Countdown timer** in the header — triggers discount expiry when it reaches zero
- **Featured plan** displayed prominently; remaining plans shown in a 3-column grid
- **Responsive** — single-column stacked layout on mobile, side-by-side on desktop
- **Discount animation** — prices animate between discounted and full price when timer ends
- **Purchase flow** — checkbox consent required before buy; shows confirmation on success
- **30-day guarantee** section at the bottom
- **SSR** — tariffs fetched server-side via `getServerSideProps` for fast initial load
