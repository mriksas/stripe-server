# PowerToGo Backend

> The Node.js API behind the PowerToGo Android power-bank rental prototype.

![Node.js](https://img.shields.io/badge/Node.js-Express-339933?logo=nodedotjs&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Mongoose-47A248?logo=mongodb&logoColor=white)
![Stripe](https://img.shields.io/badge/Stripe-Checkout%20%2B%20Webhooks-635BFF?logo=stripe&logoColor=white)

## Overview

The service manages users, stations, power-bank inventory, payments, rental state, reminders, and administrator actions. Stripe Checkout creates the payment session; a verified webhook moves the selected power bank into the rented state.

```mermaid
flowchart LR
    A[Android client] --> B[Express API]
    B --> C[(MongoDB)]
    B --> D[Stripe Checkout]
    D -->|signed webhook| B
    B --> E[Email reminders]
    F[Scheduled overdue check] --> B
```

## Main capabilities

- register users and query role/ban status;
- seed stations and power-bank inventory;
- check availability by station;
- create Stripe Checkout sessions;
- verify Stripe webhooks and activate rentals;
- list payment history and active rentals;
- return power banks;
- list, ban, and unban users;
- send scheduled overdue reminders;
- generate QR images for station metadata.

## Environment

Create a local `.env` with the variables used by `server.js`:

```env
PORT=4242
MONGODB_URI=
STRIPE_SECRET_KEY=
STRIPE_WEBHOOK_SECRET=
MAIL_FROM=
MAIL_PASSWORD=
```

## Run locally

The source imports the following packages. If starting from this repository as-is, create a package manifest and install them:

```bash
npm init -y
npm install express cors mongoose stripe body-parser node-cron nodemailer dotenv qrcode
node server.js
```

Generate station QR codes with:

```bash
node generateQR.js
```

## Important development settings

- `YOUR_DOMAIN` currently contains a LAN development address and should become an environment variable.
- The reminder schedule and overdue threshold are shortened for testing.
- Stripe CLI can forward local events to `/webhook` during development.

## Security notice

Never commit live credentials. The repository currently tracks an `.env` file; any real values it has ever contained should be rotated, removed from Git history, and replaced with an `.env.example` before public deployment.
