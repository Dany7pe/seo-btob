# SEO Content Platform - B2B Managed Service

**Plateforme de rédaction SEO managée pour agences et entreprises.**

Tu configures. Ils rédigent. Le contenu se publie.

---

## 🎯 Concept

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           TOI (SUPER ADMIN)                              │
│                                                                          │
│  📊 Vue globale        💰 Coûts API         📧 Emails auto              │
│  • 127 clients         • Suivi marge        • Onboarding                │
│  • 3,420 articles      • Par client         • Rappels quota             │
│                                                                          │
│  🎨 Templates Prompts (réutilisables entre clients)                     │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    ▼               ▼               ▼
              ┌───────────┐   ┌───────────┐   ┌───────────┐
              │ Client A  │   │ Client B  │   │ Client C  │
              │ Multi-user│   │ Multi-user│   │ Multi-user│
              └───────────┘   └───────────┘   └───────────┘
                    │               │               │
                    └───────────────┼───────────────┘
                                    ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      INTERFACE CLIENT SIMPLIFIÉE                         │
│                                                                          │
│  📝 RÉDACTION         📅 CALENDRIER        📈 OPTIMISATION              │
│  • Nouvel article     • Vue mois           • Score SEO                  │
│  • BULK (CSV)         • Drag & drop        • Recommandations            │
│  • File d'attente     • Planification      • Quick wins                 │
│  • Versions           • Auto-publish                                    │
│                                                                          │
│  👥 ÉQUIPE            📚 PUBLIÉS                                        │
│  • Inviter membres    • Historique                                      │
│  • Rôles              • Vers CMS                                        │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Stack Technique

| Catégorie | Technologie |
|-----------|-------------|
| Framework | Next.js 15 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS + Shadcn UI |
| Auth | Clerk (multi-tenant) |
| Database | Supabase (PostgreSQL) + Prisma |
| IA | Anthropic Claude (Sonnet 4) |
| Paiements | Stripe |
| Emails | Resend |
| Deployment | Vercel |

---

## 📁 Structure du Projet

Voir `CURSOR_INSTRUCTIONS.md` pour les détails complets.

---

## 🚀 Installation

```bash
# Cloner
git clone <repo>
cd seo-btob

# Installer
npm install

# Configurer
cp .env.example .env.local
# Éditer .env.local avec vos clés

# Database
npx prisma generate
npx prisma db push

# Lancer
npm run dev
```

---

## 📄 License

Proprietary - All rights reserved.
