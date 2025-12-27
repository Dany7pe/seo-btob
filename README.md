# SEO Autopilot 🚀

**The AI-powered Content Operations Platform for SEO.**

SEO Autopilot automates your entire content strategy - from keyword research to article generation and publishing. Powered by Claude AI.

![Next.js](https://img.shields.io/badge/Next.js-15-black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-blue)
![Tailwind](https://img.shields.io/badge/Tailwind-4-38bdf8)
![License](https://img.shields.io/badge/License-Proprietary-red)

## ✨ Features

- **AI-Powered Onboarding** - Zero-config setup that analyzes your website and creates a content strategy
- **Smart Content Generation** - Claude AI writes SEO-optimized articles tailored to your brand voice
- **Autopilot Mode** - Schedule automatic article generation and publishing
- **Multi-CMS Support** - WordPress, Webflow, Shopify, or custom HTML export
- **SEO Optimization** - Built-in SEO scoring, keyword analysis, and optimization suggestions
- **Topic Clustering** - Semantic content architecture for topical authority
- **Content Optimization** - Detect and fix content decay, quick wins, and growth opportunities

## 🛠️ Tech Stack

| Category | Technology |
|----------|------------|
| Framework | Next.js 15 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS v4 + Shadcn UI |
| Authentication | Clerk |
| Database | Supabase (PostgreSQL) + Prisma |
| AI | Anthropic Claude (Sonnet 4) |
| Payments | Stripe |
| Email | Resend |
| Deployment | Vercel |

## 🚀 Quick Start

### Prerequisites

- Node.js 18+
- npm or pnpm
- Supabase account
- Clerk account
- Anthropic API key

### Installation

```bash
# Clone the repository
git clone https://github.com/your-org/seo-autopilot.git
cd seo-autopilot

# Install dependencies
npm install

# Configure environment
cp .env.example .env.local
# Edit .env.local with your credentials

# Setup database
npx prisma db push
npx prisma generate

# Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.

## 📖 Documentation

Full documentation is available in the [`/docs`](./docs) folder:

- [Getting Started](./docs/README.md)
- [Environment Setup](./docs/setup/environment.md)
- [Database Setup](./docs/setup/database.md)
- [WordPress Integration](./docs/guides/wordpress.md)
- [Autopilot Guide](./docs/guides/autopilot.md)
- [API Reference](./docs/api/public-api.md)

## 🔧 Configuration

### Required Environment Variables

```env
# Database
DATABASE_URL="postgresql://..."
DIRECT_URL="postgresql://..."

# Authentication
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_..."
CLERK_SECRET_KEY="sk_..."

# AI
ANTHROPIC_API_KEY="sk-ant-..."

# Payments
STRIPE_SECRET_KEY="sk_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
```

See [Environment Setup](./docs/setup/environment.md) for full configuration guide.

## 📁 Project Structure

```
src/
├── app/                 # Next.js App Router
│   ├── (marketing)/     # Public pages
│   ├── (dashboard)/     # Protected dashboard
│   ├── api/             # API routes
│   └── onboarding/      # Onboarding wizard
├── components/          # React components
├── lib/
│   ├── actions/         # Server Actions
│   ├── integrations/    # External APIs
│   └── utils/           # Utilities
└── middleware.ts        # Auth middleware
```

## 🚀 Deployment

### Vercel (Recommended)

1. Push to GitHub
2. Import project in Vercel
3. Add environment variables
4. Deploy

### Cron Jobs

Configure these cron endpoints in Vercel:

| Endpoint | Schedule | Description |
|----------|----------|-------------|
| `/api/cron/daily-generation` | `0 8 * * *` | Generate daily articles |
| `/api/cron/auto-publish` | `0 9 * * *` | Publish approved articles |
| `/api/cron/auto-optimize` | `0 3 * * 1` | Weekly content optimization |

## 📄 License

Proprietary - All rights reserved.

## 🔗 Links

- [Product Website](https://seoautopilot.com)
- [Documentation](./docs/README.md)
- [Anthropic](https://anthropic.com)
- [Clerk](https://clerk.com)
- [Stripe](https://stripe.com)
# seo-btob
