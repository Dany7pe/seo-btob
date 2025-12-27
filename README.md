# SEO Content Platform - B2B Managed Service

**Plateforme de rédaction SEO managée pour agences et entreprises.**

Tu configures. Ils rédigent. Le contenu se publie.

---

## 🎯 Concept B2B

```
┌─────────────────────────────────────────────────────────────┐
│                      TOI (Admin)                            │
│  • Setup client      • Config CMS      • Prompts custom    │
│  • Facturation       • Monitoring      • Support           │
└─────────────────────────────┬───────────────────────────────┘
                              │
      ┌───────────────────────┼───────────────────────┐
      ▼                       ▼                       ▼
┌───────────┐           ┌───────────┐           ┌───────────┐
│ Client A  │           │ Client B  │           │ Client C  │
│ WordPress │           │ Webflow   │           │ Shopify   │
│ Prompts A │           │ Prompts B │           │ Prompts C │
└─────┬─────┘           └─────┬─────┘           └─────┬─────┘
      │                       │                       │
      └───────────────────────┼───────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              INTERFACE CLIENT SIMPLIFIÉE                     │
│                                                              │
│   📝 RÉDACTION                    📈 OPTIMISATION           │
│   ────────────                    ───────────────           │
│   • Articles générés par IA       • Score SEO               │
│   • Éditer / Valider              • Recommandations         │
│   • Planifier publication         • Articles à améliorer    │
│   • Historique                    • Mots-clés manquants     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔄 Ce qui change vs SaaS classique

| Aspect | ❌ SaaS Self-Service | ✅ B2B Managé |
|--------|---------------------|---------------|
| Onboarding | Le client configure tout | **Toi** tu configures |
| Connexion CMS | Self-service | **Toi** tu connectes |
| Prompts IA | Génériques | **Personnalisés par client** |
| Interface | Dashboard complet | **Rédaction + Opti seulement** |
| Pricing | Abonnement fixe | **Setup + Récurrent** |
| Support | Documentation | **Accompagnement** |

---

## 🏗️ Architecture Technique

### Stack Technique

| Catégorie | Technologie | Justification |
|-----------|-------------|---------------|
| Framework | **Next.js 15** (App Router) | Full-stack, SSR, API Routes |
| Language | **TypeScript** | Typage, maintenabilité |
| Styling | **Tailwind CSS + Shadcn UI** | Rapidité, composants prêts |
| Auth | **Clerk** | Multi-tenant ready, simple |
| Database | **Supabase** (PostgreSQL) | Row Level Security, temps réel |
| ORM | **Prisma** | Type-safe, migrations |
| IA | **Anthropic Claude** (Sonnet 4) | Meilleure qualité rédaction |
| Paiements | **Stripe** | Facturation récurrente |
| Deployment | **Vercel** | Deploy + Cron jobs |

### Structure des Fichiers

```
src/
├── app/
│   │
│   ├── (marketing)/                 # Pages publiques
│   │   ├── page.tsx                 # Landing page
│   │   └── layout.tsx
│   │
│   ├── (admin)/                     # 🔐 TON INTERFACE ADMIN
│   │   ├── layout.tsx               # Layout admin (vérifie role=admin)
│   │   ├── page.tsx                 # Dashboard admin
│   │   │
│   │   ├── clients/
│   │   │   ├── page.tsx             # Liste tous les clients
│   │   │   ├── new/page.tsx         # Créer nouveau client
│   │   │   └── [clientId]/
│   │   │       ├── page.tsx         # Détails client
│   │   │       ├── settings/page.tsx    # Config CMS + credentials
│   │   │       ├── prompts/page.tsx     # Prompts personnalisés
│   │   │       ├── articles/page.tsx    # Voir articles du client
│   │   │       └── billing/page.tsx     # Facturation client
│   │   │
│   │   ├── prompts/
│   │   │   ├── page.tsx             # Bibliothèque de prompts
│   │   │   └── [promptId]/page.tsx  # Éditer un prompt
│   │   │
│   │   └── billing/
│   │       └── page.tsx             # Vue globale facturation
│   │
│   ├── (client)/                    # 🔐 INTERFACE CLIENT SIMPLIFIÉE
│   │   ├── layout.tsx               # Layout client (vérifie role=client)
│   │   ├── page.tsx                 # Dashboard client (rédaction)
│   │   │
│   │   ├── articles/
│   │   │   ├── page.tsx             # Liste articles
│   │   │   ├── new/page.tsx         # Générer nouvel article
│   │   │   └── [articleId]/
│   │   │       ├── page.tsx         # Voir/Éditer article
│   │   │       └── edit/page.tsx    # Éditeur complet
│   │   │
│   │   ├── optimize/
│   │   │   ├── page.tsx             # Recommandations SEO
│   │   │   └── [articleId]/page.tsx # Détail optimisation
│   │   │
│   │   └── published/
│   │       └── page.tsx             # Historique publications
│   │
│   ├── api/
│   │   ├── webhooks/
│   │   │   ├── clerk/route.ts       # Sync users
│   │   │   └── stripe/route.ts      # Paiements
│   │   │
│   │   ├── ai/
│   │   │   ├── generate/route.ts    # Générer article
│   │   │   ├── optimize/route.ts    # Analyser SEO
│   │   │   └── suggest/route.ts     # Suggestions amélioration
│   │   │
│   │   ├── cms/
│   │   │   ├── publish/route.ts     # Publier vers CMS
│   │   │   ├── wordpress/route.ts   # Test connexion WP
│   │   │   ├── webflow/route.ts     # Test connexion Webflow
│   │   │   └── shopify/route.ts     # Test connexion Shopify
│   │   │
│   │   └── cron/
│   │       ├── daily-generation/route.ts   # Génération auto
│   │       └── weekly-optimize/route.ts    # Analyse hebdo
│   │
│   └── auth/
│       ├── sign-in/[[...sign-in]]/page.tsx
│       └── sign-up/[[...sign-up]]/page.tsx
│
├── components/
│   ├── admin/                       # Composants admin only
│   │   ├── ClientCard.tsx
│   │   ├── ClientForm.tsx
│   │   ├── PromptEditor.tsx
│   │   ├── CMSConnector.tsx
│   │   └── BillingTable.tsx
│   │
│   ├── client/                      # Composants client only
│   │   ├── ArticleCard.tsx
│   │   ├── ArticleEditor.tsx
│   │   ├── SEOScore.tsx
│   │   ├── OptimizationCard.tsx
│   │   └── PublishButton.tsx
│   │
│   ├── shared/                      # Composants partagés
│   │   ├── Header.tsx
│   │   ├── Sidebar.tsx
│   │   ├── LoadingSpinner.tsx
│   │   └── EmptyState.tsx
│   │
│   └── ui/                          # Shadcn UI components
│       ├── button.tsx
│       ├── card.tsx
│       ├── input.tsx
│       └── ...
│
├── lib/
│   ├── actions/                     # Server Actions
│   │   ├── clients.ts               # CRUD clients
│   │   ├── articles.ts              # CRUD articles
│   │   ├── prompts.ts               # CRUD prompts
│   │   └── publish.ts               # Publication CMS
│   │
│   ├── services/                    # Logique métier
│   │   ├── ai.ts                    # Appels Claude
│   │   ├── seo-analyzer.ts          # Analyse SEO
│   │   ├── cms/
│   │   │   ├── wordpress.ts
│   │   │   ├── webflow.ts
│   │   │   └── shopify.ts
│   │   └── billing.ts               # Logique Stripe
│   │
│   ├── db/
│   │   ├── index.ts                 # Client Prisma
│   │   └── queries/                 # Requêtes réutilisables
│   │       ├── clients.ts
│   │       ├── articles.ts
│   │       └── prompts.ts
│   │
│   └── utils/
│       ├── auth.ts                  # Helpers auth
│       ├── constants.ts
│       └── helpers.ts
│
├── prisma/
│   ├── schema.prisma                # Schéma DB
│   └── seed.ts                      # Données initiales
│
├── middleware.ts                    # Auth + routing par rôle
│
└── types/
    └── index.ts                     # Types globaux
```

---

## 📊 Modèle de Données

### Schéma Prisma Complet

```prisma
// prisma/schema.prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

// ============================================
// USERS & AUTH
// ============================================

enum UserRole {
  ADMIN      // Toi - accès total
  CLIENT     // Client final - accès limité
}

model User {
  id            String    @id @default(cuid())
  clerkId       String    @unique
  email         String    @unique
  name          String?
  role          UserRole  @default(CLIENT)

  // Relations
  client        Client?   @relation("ClientUser")  // Si role=CLIENT

  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// ============================================
// CLIENTS (Tes clients B2B)
// ============================================

model Client {
  id            String    @id @default(cuid())
  name          String                          // Nom entreprise
  slug          String    @unique               // URL-friendly
  website       String                          // URL du site

  // User associé
  userId        String    @unique
  user          User      @relation("ClientUser", fields: [userId], references: [id])

  // Configuration CMS
  cmsType       CMSType
  cmsConfig     Json                            // Credentials chiffrés

  // Configuration IA
  prompts       Prompt[]
  brandVoice    String?   @db.Text              // Ton de la marque
  targetAudience String?  @db.Text              // Audience cible
  keywords      String[]                        // Mots-clés principaux

  // Articles
  articles      Article[]

  // Facturation
  subscription  Subscription?

  // Metadata
  isActive      Boolean   @default(true)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

enum CMSType {
  WORDPRESS
  WEBFLOW
  SHOPIFY
  CUSTOM      // Export HTML
}

// ============================================
// PROMPTS (Templates par client)
// ============================================

model Prompt {
  id            String    @id @default(cuid())
  name          String                          // "Article Blog", "Fiche Produit"
  description   String?

  // Le prompt lui-même
  systemPrompt  String    @db.Text              // Instructions système
  userPrompt    String    @db.Text              // Template avec {{variables}}

  // Variables disponibles
  variables     Json      @default("[]")        // ["titre", "mot_cle", "longueur"]

  // Client associé (null = prompt global/template)
  clientId      String?
  client        Client?   @relation(fields: [clientId], references: [id])

  // Metadata
  isDefault     Boolean   @default(false)       // Prompt par défaut du client
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// ============================================
// ARTICLES
// ============================================

model Article {
  id            String        @id @default(cuid())

  // Contenu
  title         String
  slug          String
  content       String        @db.Text
  excerpt       String?       @db.Text
  metaTitle     String?
  metaDescription String?     @db.Text

  // SEO
  seoScore      Int?                            // 0-100
  seoAnalysis   Json?                           // Détail de l'analyse
  keywords      String[]                        // Mots-clés ciblés

  // Status
  status        ArticleStatus @default(DRAFT)

  // Relations
  clientId      String
  client        Client        @relation(fields: [clientId], references: [id])

  // Publication
  publishedAt   DateTime?
  cmsPostId     String?                         // ID dans le CMS externe
  cmsPostUrl    String?                         // URL publiée

  // Optimisation
  optimizations Optimization[]

  // Metadata
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt

  @@unique([clientId, slug])
}

enum ArticleStatus {
  DRAFT           // Brouillon généré
  REVIEW          // En attente de validation client
  APPROVED        // Validé, prêt à publier
  SCHEDULED       // Planifié
  PUBLISHED       // Publié sur le CMS
  NEEDS_UPDATE    // Nécessite mise à jour (content decay)
}

// ============================================
// OPTIMISATIONS & RECOMMANDATIONS
// ============================================

model Optimization {
  id            String            @id @default(cuid())

  // Article concerné
  articleId     String
  article       Article           @relation(fields: [articleId], references: [id])

  // Type et détails
  type          OptimizationType
  priority      Priority          @default(MEDIUM)
  title         String                          // "Ajouter le mot-clé principal"
  description   String            @db.Text      // Explication détaillée
  suggestion    String?           @db.Text      // Texte suggéré

  // Status
  status        OptimizationStatus @default(PENDING)

  // Metadata
  createdAt     DateTime          @default(now())
  resolvedAt    DateTime?
}

enum OptimizationType {
  MISSING_KEYWORD     // Mot-clé manquant
  TITLE_TOO_SHORT     // Titre trop court
  TITLE_TOO_LONG      // Titre trop long
  META_MISSING        // Meta description manquante
  META_TOO_SHORT      // Meta trop courte
  META_TOO_LONG       // Meta trop longue
  CONTENT_TOO_SHORT   // Contenu trop court
  NO_HEADINGS         // Pas de H2/H3
  NO_INTERNAL_LINKS   // Pas de liens internes
  NO_EXTERNAL_LINKS   // Pas de liens externes
  CONTENT_DECAY       // Contenu obsolète
  QUICK_WIN           // Amélioration rapide possible
}

enum Priority {
  LOW
  MEDIUM
  HIGH
  CRITICAL
}

enum OptimizationStatus {
  PENDING       // En attente
  IN_PROGRESS   // En cours
  DONE          // Résolu
  IGNORED       // Ignoré par le client
}

// ============================================
// FACTURATION
// ============================================

model Subscription {
  id                  String    @id @default(cuid())

  // Client
  clientId            String    @unique
  client              Client    @relation(fields: [clientId], references: [id])

  // Stripe
  stripeCustomerId    String    @unique
  stripeSubscriptionId String?  @unique
  stripePriceId       String?

  // Status
  status              SubscriptionStatus @default(ACTIVE)

  // Limites
  articlesPerMonth    Int       @default(10)
  articlesUsed        Int       @default(0)

  // Dates
  currentPeriodStart  DateTime?
  currentPeriodEnd    DateTime?
  canceledAt          DateTime?

  createdAt           DateTime  @default(now())
  updatedAt           DateTime  @updatedAt
}

enum SubscriptionStatus {
  ACTIVE
  PAST_DUE
  CANCELED
  PAUSED
}
```

---

## 🔐 Middleware & Routing par Rôle

```typescript
// middleware.ts

import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'
import { NextResponse } from 'next/server'

const isAdminRoute = createRouteMatcher(['/admin(.*)'])
const isClientRoute = createRouteMatcher(['/dashboard(.*)'])
const isPublicRoute = createRouteMatcher(['/', '/sign-in(.*)', '/sign-up(.*)'])

export default clerkMiddleware(async (auth, req) => {
  const { userId, sessionClaims } = await auth()

  // Routes publiques
  if (isPublicRoute(req)) {
    return NextResponse.next()
  }

  // Non authentifié
  if (!userId) {
    return NextResponse.redirect(new URL('/sign-in', req.url))
  }

  const role = sessionClaims?.metadata?.role as string

  // Route admin - vérifier rôle admin
  if (isAdminRoute(req)) {
    if (role !== 'ADMIN') {
      return NextResponse.redirect(new URL('/dashboard', req.url))
    }
  }

  // Route client - vérifier rôle client ou admin
  if (isClientRoute(req)) {
    if (role !== 'CLIENT' && role !== 'ADMIN') {
      return NextResponse.redirect(new URL('/sign-in', req.url))
    }
  }

  return NextResponse.next()
})

export const config = {
  matcher: ['/((?!.*\\..*|_next).*)', '/', '/(api|trpc)(.*)'],
}
```

---

## 🤖 Service IA (Claude)

```typescript
// lib/services/ai.ts

import Anthropic from '@anthropic-ai/sdk'
import { Prompt, Client } from '@prisma/client'

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY!,
})

interface GenerateArticleInput {
  client: Client & { prompts: Prompt[] }
  topic: string
  keyword: string
  length?: 'short' | 'medium' | 'long'
}

export async function generateArticle({
  client,
  topic,
  keyword,
  length = 'medium',
}: GenerateArticleInput) {
  // Trouver le prompt par défaut du client
  const prompt = client.prompts.find(p => p.isDefault) || client.prompts[0]

  if (!prompt) {
    throw new Error('Aucun prompt configuré pour ce client')
  }

  // Construire le system prompt
  const systemPrompt = `${prompt.systemPrompt}

CONTEXTE CLIENT:
- Entreprise: ${client.name}
- Site web: ${client.website}
- Ton de la marque: ${client.brandVoice || 'Professionnel et accessible'}
- Audience cible: ${client.targetAudience || 'Professionnels B2B'}
- Mots-clés principaux: ${client.keywords.join(', ')}
`

  // Remplacer les variables dans le user prompt
  const userPrompt = prompt.userPrompt
    .replace('{{topic}}', topic)
    .replace('{{keyword}}', keyword)
    .replace('{{length}}', length === 'short' ? '800' : length === 'medium' ? '1500' : '2500')

  const response = await anthropic.messages.create({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 4096,
    system: systemPrompt,
    messages: [
      { role: 'user', content: userPrompt }
    ],
  })

  const content = response.content[0]
  if (content.type !== 'text') {
    throw new Error('Réponse inattendue de Claude')
  }

  return parseArticleResponse(content.text)
}

function parseArticleResponse(text: string) {
  // Parser la réponse structurée de Claude
  // Format attendu: titre, meta, contenu séparés
  return {
    title: extractSection(text, 'TITLE'),
    metaDescription: extractSection(text, 'META'),
    content: extractSection(text, 'CONTENT'),
    excerpt: extractSection(text, 'EXCERPT'),
  }
}

// Analyse SEO
export async function analyzeArticleSEO(
  content: string,
  targetKeyword: string
) {
  const response = await anthropic.messages.create({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 2048,
    system: `Tu es un expert SEO. Analyse l'article fourni et retourne un JSON avec:
- score: nombre de 0 à 100
- issues: tableau d'objets {type, priority, title, description, suggestion}

Types possibles: MISSING_KEYWORD, TITLE_TOO_SHORT, META_MISSING, CONTENT_TOO_SHORT, NO_HEADINGS, etc.
Priorités: LOW, MEDIUM, HIGH, CRITICAL`,
    messages: [
      {
        role: 'user',
        content: `Mot-clé cible: ${targetKeyword}\n\nArticle:\n${content}`
      }
    ],
  })

  const text = response.content[0]
  if (text.type !== 'text') throw new Error('Erreur analyse')

  return JSON.parse(text.text)
}
```

---

## 📝 Prompts Templates

### Prompt par Défaut (Article Blog)

```typescript
// À créer via l'interface admin pour chaque client

const defaultBlogPrompt = {
  name: "Article de Blog",
  description: "Génère un article de blog optimisé SEO",

  systemPrompt: `Tu es un rédacteur SEO expert. Tu écris des articles de blog optimisés pour le référencement naturel.

RÈGLES:
1. Utilise le mot-clé principal naturellement (densité 1-2%)
2. Structure avec H2 et H3 pertinents
3. Paragraphes courts (3-4 phrases max)
4. Inclus une introduction engageante
5. Termine par une conclusion avec CTA
6. Ton: professionnel mais accessible
7. Évite le jargon technique inutile

FORMAT DE RÉPONSE:
TITLE: [Titre optimisé avec mot-clé, 50-60 caractères]
META: [Meta description engageante, 150-160 caractères]
EXCERPT: [Résumé de 2-3 phrases]
CONTENT:
[Contenu de l'article en Markdown]`,

  userPrompt: `Écris un article de blog sur le sujet suivant:

SUJET: {{topic}}
MOT-CLÉ PRINCIPAL: {{keyword}}
LONGUEUR: environ {{length}} mots

L'article doit être informatif, engageant et optimisé pour le SEO.`,

  variables: ["topic", "keyword", "length"]
}
```

---

## 🔄 Workflow Complet

### 1. Onboarding Client (Admin)

```
Admin se connecte
    │
    ├─→ /admin/clients/new
    │   ├─ Nom entreprise
    │   ├─ URL du site
    │   ├─ Email du contact
    │   └─ CMS utilisé
    │
    ├─→ Créer compte Clerk pour le client
    │   └─ Role = CLIENT
    │
    ├─→ /admin/clients/[id]/settings
    │   ├─ Configurer connexion CMS
    │   │   ├─ WordPress: URL + App Password
    │   │   ├─ Webflow: API Token + Site ID
    │   │   └─ Shopify: API Key + Store URL
    │   └─ Tester la connexion
    │
    ├─→ /admin/clients/[id]/prompts
    │   ├─ Créer prompts personnalisés
    │   ├─ Définir ton de la marque
    │   ├─ Définir audience cible
    │   └─ Mots-clés principaux
    │
    └─→ Client reçoit email avec accès
```

### 2. Génération Article (Client)

```
Client se connecte
    │
    ├─→ /dashboard (Interface simplifiée)
    │
    ├─→ /dashboard/articles/new
    │   ├─ Entrer sujet
    │   ├─ Entrer mot-clé cible
    │   ├─ Choisir longueur
    │   └─ [Générer]
    │
    ├─→ IA génère l'article
    │   ├─ Utilise les prompts configurés par Admin
    │   ├─ Applique ton de la marque
    │   └─ Optimise pour le mot-clé
    │
    ├─→ /dashboard/articles/[id]
    │   ├─ Preview de l'article
    │   ├─ Score SEO affiché
    │   ├─ [Modifier] → Éditeur
    │   ├─ [Approuver] → Prêt à publier
    │   └─ [Planifier] → Choisir date
    │
    └─→ Publication
        ├─ Automatique (cron) ou manuelle
        └─ Vers le CMS configuré par Admin
```

### 3. Optimisation (Client)

```
Client va sur /dashboard/optimize
    │
    ├─→ Liste des recommandations
    │   ├─ Triées par priorité
    │   ├─ Groupées par article
    │   └─ Filtrables par type
    │
    ├─→ Clic sur une recommandation
    │   ├─ Détail du problème
    │   ├─ Suggestion de correction
    │   └─ [Appliquer] / [Ignorer]
    │
    └─→ Appliquer correction
        ├─ Ouvre l'éditeur avec suggestion
        ├─ Client valide
        └─ Score SEO recalculé
```

---

## ⚙️ Variables d'Environnement

```env
# .env.example

# ============================================
# DATABASE
# ============================================
DATABASE_URL="postgresql://postgres:[password]@db.[project].supabase.co:5432/postgres"
DIRECT_URL="postgresql://postgres:[password]@db.[project].supabase.co:5432/postgres"

# ============================================
# AUTHENTICATION (Clerk)
# ============================================
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY="pk_test_..."
CLERK_SECRET_KEY="sk_test_..."
NEXT_PUBLIC_CLERK_SIGN_IN_URL="/sign-in"
NEXT_PUBLIC_CLERK_SIGN_UP_URL="/sign-up"
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL="/dashboard"
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL="/dashboard"

# ============================================
# AI (Anthropic)
# ============================================
ANTHROPIC_API_KEY="sk-ant-..."

# ============================================
# PAYMENTS (Stripe)
# ============================================
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."

# ============================================
# CMS ENCRYPTION
# ============================================
CMS_ENCRYPTION_KEY="32-character-random-string-here"

# ============================================
# APP
# ============================================
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# ============================================
# CRON SECRET (Vercel)
# ============================================
CRON_SECRET="random-secret-for-cron-auth"
```

---

## 🚀 Cron Jobs

| Endpoint | Schedule | Description |
|----------|----------|-------------|
| `/api/cron/daily-generation` | `0 8 * * *` | Génère articles planifiés |
| `/api/cron/auto-publish` | `0 9 * * *` | Publie articles approuvés |
| `/api/cron/weekly-optimize` | `0 3 * * 1` | Analyse SEO hebdomadaire |

```typescript
// app/api/cron/weekly-optimize/route.ts

import { NextRequest, NextResponse } from 'next/server'
import { prisma } from '@/lib/db'
import { analyzeArticleSEO } from '@/lib/services/ai'

export async function GET(req: NextRequest) {
  // Vérifier le secret
  const authHeader = req.headers.get('authorization')
  if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  // Récupérer tous les articles publiés
  const articles = await prisma.article.findMany({
    where: { status: 'PUBLISHED' },
    include: { client: true }
  })

  for (const article of articles) {
    // Analyser chaque article
    const analysis = await analyzeArticleSEO(
      article.content,
      article.keywords[0] || ''
    )

    // Mettre à jour le score et créer les optimisations
    await prisma.article.update({
      where: { id: article.id },
      data: {
        seoScore: analysis.score,
        seoAnalysis: analysis,
      }
    })

    // Créer les nouvelles recommandations
    for (const issue of analysis.issues) {
      await prisma.optimization.create({
        data: {
          articleId: article.id,
          type: issue.type,
          priority: issue.priority,
          title: issue.title,
          description: issue.description,
          suggestion: issue.suggestion,
        }
      })
    }
  }

  return NextResponse.json({
    success: true,
    analyzed: articles.length
  })
}
```

---

## 📦 Ce qui est SUPPRIMÉ vs SaaS original

| Fonctionnalité | Raison suppression |
|----------------|-------------------|
| ❌ Onboarding wizard client | Toi tu configures |
| ❌ Auto-connexion CMS par client | Toi tu connectes |
| ❌ Configuration prompts par client | Toi tu personnalises |
| ❌ Topic clustering complexe | Simplifie l'interface |
| ❌ Content decay detection auto | Remplacé par analyse hebdo |
| ❌ Multi-site par client | 1 client = 1 site |
| ❌ Export HTML custom | Focus sur CMS intégrés |
| ❌ Keyword research intégré | Toi tu définis les KW |

## ✅ Ce qui est AJOUTÉ pour B2B

| Fonctionnalité | Raison ajout |
|----------------|--------------|
| ✅ Interface Admin séparée | Gestion de tous tes clients |
| ✅ Gestion des prompts par client | Personnalisation maximale |
| ✅ Configuration CMS par Admin | Setup complet par toi |
| ✅ Rôles User (Admin/Client) | Séparation des accès |
| ✅ Facturation par client | Suivi business |
| ✅ Dashboard client simplifié | Juste rédaction + opti |
| ✅ Bibliothèque de prompts | Réutilisation entre clients |

---

## 🎯 MVP - Ordre d'Implémentation

### Phase 1: Fondations (Semaine 1)
- [ ] Setup Next.js + TypeScript + Tailwind
- [ ] Setup Prisma + Supabase
- [ ] Setup Clerk avec rôles
- [ ] Middleware de routing
- [ ] Layout Admin + Layout Client

### Phase 2: Admin Interface (Semaine 2)
- [ ] CRUD Clients
- [ ] Configuration CMS (WordPress d'abord)
- [ ] CRUD Prompts
- [ ] Test connexion CMS

### Phase 3: Client Interface (Semaine 3)
- [ ] Dashboard client
- [ ] Génération articles (Claude)
- [ ] Éditeur d'articles
- [ ] Publication vers WordPress

### Phase 4: Optimisation (Semaine 4)
- [ ] Analyse SEO
- [ ] Recommandations
- [ ] Score SEO
- [ ] Cron jobs

### Phase 5: Polish (Semaine 5)
- [ ] Facturation Stripe
- [ ] Webflow + Shopify
- [ ] Emails (Resend)
- [ ] Tests + Deploy

---

## 📄 License

Proprietary - All rights reserved.
