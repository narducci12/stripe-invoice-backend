# Stripe Invoice Payment Backend

This is a simple Vercel serverless backend for processing invoice payments via Stripe.

## Setup Instructions

### 1. Prerequisites
- Stripe account ([stripe.com](https://stripe.com))
- Vercel account ([vercel.com](https://vercel.com))
- GitHub account (for easy deployment)

### 2. Get Your Stripe Keys
1. Log into Stripe Dashboard
2. Go to **Developers → API Keys**
3. Copy your **Secret Key** (starts with `sk_test_` or `sk_live_`)

### 3. Deploy to Vercel

#### Option A: Deploy via GitHub (Recommended)
1. Create a new GitHub repository
2. Upload these files to the repository
3. Go to [vercel.com](https://vercel.com) and click "Import Project"
4. Select your GitHub repository
5. Add environment variable:
   - Name: `STRIPE_SECRET_KEY`
   - Value: Your Stripe secret key (sk_test_xxx or sk_live_xxx)
6. Click "Deploy"

#### Option B: Deploy via Vercel CLI
1. Install Vercel CLI: `npm install -g vercel`
2. Navigate to this folder: `cd stripe-invoice-backend`
3. Run: `vercel`
4. Follow prompts to deploy
5. Add environment variable in Vercel Dashboard:
   - Go to your project → Settings → Environment Variables
   - Add `STRIPE_SECRET_KEY` with your secret key

### 4. Get Your API URL
After deployment, Vercel will give you a URL like:
`https://your-project-name.vercel.app`

Your API endpoints will be:
- `https://your-project-name.vercel.app/api/create-payment-intent`
- `https://your-project-name.vercel.app/api/verify-payment`

### 5. Update Your Shop App
In your shop management HTML file, update the `STRIPE_CONFIG`:

```javascript
const STRIPE_CONFIG = {
    publishableKey: 'pk_test_YOUR_KEY_HERE', // or pk_live_ for production
    apiUrl: 'https://your-project-name.vercel.app'
};
```

## API Endpoints

### POST /api/create-payment-intent
Creates a Stripe PaymentIntent for processing a payment.

**Request Body:**
```json
{
  "amount": 299.99,
  "invoiceId": "INV-2026-001",
  "customerEmail": "customer@email.com",
  "customerName": "John Smith",
  "description": "Invoice Payment"
}
```

**Response:**
```json
{
  "clientSecret": "pi_xxx_secret_xxx",
  "paymentIntentId": "pi_xxx"
}
```

### POST /api/verify-payment
Verifies a payment was successful.

**Request Body:**
```json
{
  "paymentIntentId": "pi_xxx"
}
```

**Response:**
```json
{
  "status": "succeeded",
  "amount": 299.99,
  "invoiceId": "INV-2026-001",
  "paid": true
}
```

## Testing

Use Stripe's test card numbers:
- **Success:** 4242 4242 4242 4242
- **Decline:** 4000 0000 0000 0002
- **Requires Auth:** 4000 0025 0000 3155

Use any future expiry date and any 3-digit CVC.

## Going Live

1. In Stripe Dashboard, toggle to "Live mode"
2. Copy your live API keys
3. Update Vercel environment variable with live secret key
4. Update your app with the live publishable key

## Support

If you have issues:
1. Check Vercel deployment logs
2. Check Stripe Dashboard → Developers → Logs
3. Ensure environment variable is set correctly
