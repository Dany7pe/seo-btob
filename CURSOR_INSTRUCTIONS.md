# INSTRUCTIONS POUR CURSOR

## 🎯 CONTEXTE DU PROJET

Tu développes une **plateforme SaaS B2B de rédaction SEO managée**.

**Modèle Business:**
- L'admin (moi) configure tout pour chaque client (CMS, prompts, accès)
- Les clients ont une interface simplifiée : rédaction + optimisation + calendrier
- Facturation : Setup initial + abonnement mensuel
- Multi-utilisateurs par client (équipes de rédacteurs)

**Stack Technique:**
- Next.js 15 (App Router)
- TypeScript (strict)
- Tailwind CSS + Shadcn UI
- Clerk (authentification multi-tenant)
- Prisma + Supabase (PostgreSQL)
- Anthropic Claude API (génération de contenu)
- Stripe (paiements)
- Resend (emails transactionnels)
- Vercel (déploiement + cron jobs)

---

## 📁 STRUCTURE COMPLÈTE DES FICHIERS

```
seo-btob/
├── .env.example
├── .env.local                    # Ne pas commit
├── .gitignore
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── package.json
├── middleware.ts                 # Auth + routing par rôle
│
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
│
├── src/
│   ├── app/
│   │   ├── globals.css
│   │   ├── layout.tsx            # Root layout
│   │   │
│   │   ├── (marketing)/          # Pages publiques
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx          # Landing page
│   │   │
│   │   ├── (auth)/               # Auth pages (Clerk)
│   │   │   ├── sign-in/[[...sign-in]]/page.tsx
│   │   │   └── sign-up/[[...sign-up]]/page.tsx
│   │   │
│   │   ├── (admin)/              # Interface ADMIN (moi)
│   │   │   ├── layout.tsx        # Sidebar admin + check role
│   │   │   ├── page.tsx          # /admin → Dashboard
│   │   │   │
│   │   │   ├── clients/
│   │   │   │   ├── page.tsx                    # Liste clients
│   │   │   │   ├── new/page.tsx                # Créer client
│   │   │   │   └── [clientId]/
│   │   │   │       ├── page.tsx                # Dashboard client
│   │   │   │       ├── settings/page.tsx       # Config CMS
│   │   │   │       ├── prompts/page.tsx        # Prompts custom
│   │   │   │       ├── users/page.tsx          # Gérer users du client
│   │   │   │       ├── articles/page.tsx       # Articles du client
│   │   │   │       └── usage/page.tsx          # Stats usage/coûts
│   │   │   │
│   │   │   ├── templates/
│   │   │   │   ├── page.tsx                    # Bibliothèque prompts
│   │   │   │   ├── new/page.tsx                # Créer template
│   │   │   │   └── [templateId]/page.tsx       # Éditer template
│   │   │   │
│   │   │   └── billing/
│   │   │       ├── page.tsx                    # Vue revenus
│   │   │       └── costs/page.tsx              # Coûts API
│   │   │
│   │   ├── (dashboard)/          # Interface CLIENT
│   │   │   ├── layout.tsx        # Sidebar client + check accès
│   │   │   ├── page.tsx          # /dashboard → Accueil client
│   │   │   │
│   │   │   ├── articles/
│   │   │   │   ├── page.tsx                    # Liste articles
│   │   │   │   ├── new/page.tsx                # Nouvel article
│   │   │   │   ├── bulk/page.tsx               # Import CSV bulk
│   │   │   │   ├── queue/page.tsx              # File d'attente
│   │   │   │   └── [articleId]/
│   │   │   │       ├── page.tsx                # Preview article
│   │   │   │       ├── edit/page.tsx           # Éditeur
│   │   │   │       └── history/page.tsx        # Versions
│   │   │   │
│   │   │   ├── calendar/
│   │   │   │   └── page.tsx                    # Calendrier éditorial
│   │   │   │
│   │   │   ├── optimize/
│   │   │   │   ├── page.tsx                    # Recommandations
│   │   │   │   └── [articleId]/page.tsx        # Détail opti
│   │   │   │
│   │   │   ├── published/
│   │   │   │   └── page.tsx                    # Articles publiés
│   │   │   │
│   │   │   ├── team/
│   │   │   │   ├── page.tsx                    # Membres équipe
│   │   │   │   └── invite/page.tsx             # Inviter membre
│   │   │   │
│   │   │   └── settings/
│   │   │       └── page.tsx                    # Mon compte
│   │   │
│   │   └── api/
│   │       ├── webhooks/
│   │       │   ├── clerk/route.ts              # Sync users Clerk
│   │       │   └── stripe/route.ts             # Events Stripe
│   │       │
│   │       ├── ai/
│   │       │   ├── generate/route.ts           # Générer 1 article
│   │       │   ├── generate-bulk/route.ts      # Générer en masse
│   │       │   ├── analyze/route.ts            # Analyser SEO
│   │       │   └── suggest/route.ts            # Suggestions
│   │       │
│   │       ├── cms/
│   │       │   ├── test/route.ts               # Tester connexion
│   │       │   └── publish/route.ts            # Publier article
│   │       │
│   │       ├── clients/
│   │       │   ├── route.ts                    # CRUD clients
│   │       │   └── [clientId]/
│   │       │       ├── route.ts
│   │       │       └── invite/route.ts         # Inviter user
│   │       │
│   │       ├── articles/
│   │       │   ├── route.ts                    # CRUD articles
│   │       │   └── [articleId]/
│   │       │       ├── route.ts
│   │       │       ├── versions/route.ts
│   │       │       └── publish/route.ts
│   │       │
│   │       └── cron/
│   │           ├── process-queue/route.ts      # Traiter file génération
│   │           ├── auto-publish/route.ts       # Publier planifiés
│   │           └── weekly-analyze/route.ts     # Analyse SEO hebdo
│   │
│   ├── components/
│   │   ├── admin/
│   │   │   ├── AdminSidebar.tsx
│   │   │   ├── ClientCard.tsx
│   │   │   ├── ClientForm.tsx
│   │   │   ├── ClientsTable.tsx
│   │   │   ├── CMSConfigForm.tsx
│   │   │   ├── PromptEditor.tsx
│   │   │   ├── TemplateCard.tsx
│   │   │   ├── UsageChart.tsx
│   │   │   ├── CostsTable.tsx
│   │   │   └── AlertsBanner.tsx
│   │   │
│   │   ├── dashboard/
│   │   │   ├── DashboardSidebar.tsx
│   │   │   ├── ArticleCard.tsx
│   │   │   ├── ArticleEditor.tsx
│   │   │   ├── ArticlePreview.tsx
│   │   │   ├── BulkUploader.tsx
│   │   │   ├── GenerationQueue.tsx
│   │   │   ├── CalendarView.tsx
│   │   │   ├── SEOScore.tsx
│   │   │   ├── SEOScoreDetails.tsx
│   │   │   ├── OptimizationCard.tsx
│   │   │   ├── PublishButton.tsx
│   │   │   ├── VersionHistory.tsx
│   │   │   ├── TeamMemberCard.tsx
│   │   │   └── InviteForm.tsx
│   │   │
│   │   ├── shared/
│   │   │   ├── Header.tsx
│   │   │   ├── Logo.tsx
│   │   │   ├── LoadingSpinner.tsx
│   │   │   ├── EmptyState.tsx
│   │   │   ├── ConfirmDialog.tsx
│   │   │   ├── Pagination.tsx
│   │   │   ├── SearchInput.tsx
│   │   │   └── StatusBadge.tsx
│   │   │
│   │   └── ui/                   # Shadcn UI (généré)
│   │       ├── button.tsx
│   │       ├── card.tsx
│   │       ├── input.tsx
│   │       ├── textarea.tsx
│   │       ├── select.tsx
│   │       ├── dialog.tsx
│   │       ├── dropdown-menu.tsx
│   │       ├── table.tsx
│   │       ├── tabs.tsx
│   │       ├── toast.tsx
│   │       ├── calendar.tsx
│   │       ├── badge.tsx
│   │       ├── progress.tsx
│   │       ├── avatar.tsx
│   │       └── ...
│   │
│   ├── lib/
│   │   ├── db.ts                 # Client Prisma singleton
│   │   │
│   │   ├── actions/              # Server Actions
│   │   │   ├── clients.ts
│   │   │   ├── articles.ts
│   │   │   ├── prompts.ts
│   │   │   ├── templates.ts
│   │   │   ├── team.ts
│   │   │   └── publish.ts
│   │   │
│   │   ├── services/
│   │   │   ├── ai.ts             # Claude API
│   │   │   ├── seo-analyzer.ts   # Analyse SEO
│   │   │   ├── cms/
│   │   │   │   ├── index.ts      # Factory
│   │   │   │   ├── wordpress.ts
│   │   │   │   ├── webflow.ts
│   │   │   │   └── shopify.ts
│   │   │   ├── email.ts          # Resend
│   │   │   └── billing.ts        # Stripe
│   │   │
│   │   ├── utils/
│   │   │   ├── auth.ts           # Helpers auth/rôles
│   │   │   ├── cn.ts             # classNames helper
│   │   │   ├── constants.ts
│   │   │   ├── encryption.ts     # Chiffrer credentials CMS
│   │   │   ├── csv-parser.ts     # Parser CSV bulk
│   │   │   └── slug.ts           # Générer slugs
│   │   │
│   │   └── validations/
│   │       ├── client.ts         # Zod schemas
│   │       ├── article.ts
│   │       ├── prompt.ts
│   │       └── bulk.ts
│   │
│   ├── hooks/
│   │   ├── use-client.ts
│   │   ├── use-articles.ts
│   │   ├── use-generation-queue.ts
│   │   └── use-debounce.ts
│   │
│   └── types/
│       └── index.ts              # Types globaux
│
└── public/
    ├── logo.svg
    └── ...
```

---

## 📊 SCHÉMA PRISMA COMPLET

Le fichier `prisma/schema.prisma` doit contenir exactement ce schéma :

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider  = "postgresql"
  url       = env("DATABASE_URL")
  directUrl = env("DIRECT_URL")
}

// ============================================
// ENUMS
// ============================================

enum UserRole {
  SUPER_ADMIN   // Toi - accès total
  CLIENT_ADMIN  // Admin d'un client
  CLIENT_EDITOR // Rédacteur d'un client
  CLIENT_VIEWER // Lecture seule
}

enum CMSType {
  WORDPRESS
  WEBFLOW
  SHOPIFY
}

enum ArticleStatus {
  DRAFT
  REVIEW
  APPROVED
  SCHEDULED
  PUBLISHED
  NEEDS_UPDATE
}

enum JobStatus {
  PENDING
  PROCESSING
  COMPLETED
  FAILED
}

enum OptimizationType {
  MISSING_KEYWORD
  KEYWORD_DENSITY_LOW
  KEYWORD_DENSITY_HIGH
  TITLE_TOO_SHORT
  TITLE_TOO_LONG
  TITLE_MISSING_KEYWORD
  META_MISSING
  META_TOO_SHORT
  META_TOO_LONG
  CONTENT_TOO_SHORT
  NO_H2
  NO_H3
  NO_INTERNAL_LINKS
  NO_EXTERNAL_LINKS
  NO_IMAGES
  CONTENT_DECAY
  QUICK_WIN
}

enum Priority {
  LOW
  MEDIUM
  HIGH
  CRITICAL
}

enum OptimizationStatus {
  PENDING
  IN_PROGRESS
  DONE
  IGNORED
}

enum SubscriptionStatus {
  TRIALING
  ACTIVE
  PAST_DUE
  CANCELED
  PAUSED
}

enum UsageType {
  GENERATION
  OPTIMIZATION
  ANALYSIS
}

enum InviteStatus {
  PENDING
  ACCEPTED
  EXPIRED
}

// ============================================
// USERS & AUTH
// ============================================

model User {
  id            String    @id @default(cuid())
  clerkId       String    @unique
  email         String    @unique
  firstName     String?
  lastName      String?
  imageUrl      String?
  role          UserRole  @default(CLIENT_EDITOR)

  // Relations
  clientUsers   ClientUser[]
  articlesCreated Article[] @relation("ArticleCreator")
  invitesSent   Invite[]  @relation("InviteSender")

  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// ============================================
// CLIENTS (Entreprises B2B)
// ============================================

model Client {
  id            String    @id @default(cuid())
  name          String
  slug          String    @unique
  website       String
  logo          String?

  // Configuration CMS (credentials chiffrés)
  cmsType       CMSType?
  cmsConfig     Json?

  // Configuration IA
  brandVoice    String?   @db.Text
  targetAudience String?  @db.Text
  keywords      String[]

  // Relations
  users         ClientUser[]
  prompts       Prompt[]
  articles      Article[]
  generationJobs GenerationJob[]
  subscription  Subscription?
  usageLogs     UsageLog[]
  invites       Invite[]

  // Metadata
  isActive      Boolean   @default(true)
  onboardedAt   DateTime?
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// Relation Many-to-Many User <-> Client avec rôle
model ClientUser {
  id            String    @id @default(cuid())

  userId        String
  user          User      @relation(fields: [userId], references: [id], onDelete: Cascade)

  clientId      String
  client        Client    @relation(fields: [clientId], references: [id], onDelete: Cascade)

  role          UserRole  @default(CLIENT_EDITOR)

  createdAt     DateTime  @default(now())

  @@unique([userId, clientId])
}

// ============================================
// INVITATIONS
// ============================================

model Invite {
  id            String       @id @default(cuid())
  email         String
  role          UserRole     @default(CLIENT_EDITOR)
  token         String       @unique @default(cuid())
  status        InviteStatus @default(PENDING)

  clientId      String
  client        Client       @relation(fields: [clientId], references: [id], onDelete: Cascade)

  invitedById   String
  invitedBy     User         @relation("InviteSender", fields: [invitedById], references: [id])

  expiresAt     DateTime
  acceptedAt    DateTime?
  createdAt     DateTime     @default(now())
}

// ============================================
// TEMPLATES PROMPTS (Globaux, réutilisables)
// ============================================

model PromptTemplate {
  id            String    @id @default(cuid())
  name          String
  description   String?
  category      String    // "blog", "ecommerce", "legal", "realestate"

  systemPrompt  String    @db.Text
  userPrompt    String    @db.Text
  variables     Json      @default("[]")

  // Stats
  usageCount    Int       @default(0)

  isActive      Boolean   @default(true)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// ============================================
// PROMPTS (Par client, basés sur templates ou custom)
// ============================================

model Prompt {
  id            String    @id @default(cuid())
  name          String
  description   String?

  systemPrompt  String    @db.Text
  userPrompt    String    @db.Text
  variables     Json      @default("[]")

  // Client associé
  clientId      String
  client        Client    @relation(fields: [clientId], references: [id], onDelete: Cascade)

  // Template source (optionnel)
  templateId    String?

  isDefault     Boolean   @default(false)
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

// ============================================
// ARTICLES
// ============================================

model Article {
  id              String        @id @default(cuid())

  // Contenu
  title           String
  slug            String
  content         String        @db.Text
  excerpt         String?       @db.Text
  metaTitle       String?
  metaDescription String?       @db.Text
  featuredImage   String?

  // SEO
  targetKeyword   String
  secondaryKeywords String[]
  seoScore        Int?
  seoAnalysis     Json?

  // Status & Planning
  status          ArticleStatus @default(DRAFT)
  scheduledAt     DateTime?
  publishedAt     DateTime?

  // CMS
  cmsPostId       String?
  cmsPostUrl      String?

  // Relations
  clientId        String
  client          Client        @relation(fields: [clientId], references: [id], onDelete: Cascade)

  createdById     String
  createdBy       User          @relation("ArticleCreator", fields: [createdById], references: [id])

  generationJobId String?
  generationJob   GenerationJob? @relation(fields: [generationJobId], references: [id])

  versions        ArticleVersion[]
  optimizations   Optimization[]

  createdAt       DateTime      @default(now())
  updatedAt       DateTime      @updatedAt

  @@unique([clientId, slug])
}

// ============================================
// VERSIONS D'ARTICLES
// ============================================

model ArticleVersion {
  id            String    @id @default(cuid())
  version       Int

  title         String
  content       String    @db.Text
  metaTitle     String?
  metaDescription String? @db.Text

  articleId     String
  article       Article   @relation(fields: [articleId], references: [id], onDelete: Cascade)

  createdById   String
  createdAt     DateTime  @default(now())

  @@unique([articleId, version])
}

// ============================================
// GÉNÉRATION EN MASSE (BULK)
// ============================================

model GenerationJob {
  id              String    @id @default(cuid())

  clientId        String
  client          Client    @relation(fields: [clientId], references: [id], onDelete: Cascade)

  // Données d'entrée
  keywords        Json      // [{topic, keyword, length}]
  totalCount      Int

  // Progression
  status          JobStatus @default(PENDING)
  completedCount  Int       @default(0)
  failedCount     Int       @default(0)
  errorMessages   Json?

  // Résultats
  articles        Article[]

  createdAt       DateTime  @default(now())
  startedAt       DateTime?
  completedAt     DateTime?
}

// ============================================
// OPTIMISATIONS & RECOMMANDATIONS
// ============================================

model Optimization {
  id            String            @id @default(cuid())

  articleId     String
  article       Article           @relation(fields: [articleId], references: [id], onDelete: Cascade)

  type          OptimizationType
  priority      Priority          @default(MEDIUM)
  title         String
  description   String            @db.Text
  suggestion    String?           @db.Text

  status        OptimizationStatus @default(PENDING)

  createdAt     DateTime          @default(now())
  resolvedAt    DateTime?
}

// ============================================
// ABONNEMENTS & FACTURATION
// ============================================

model Subscription {
  id                    String             @id @default(cuid())

  clientId              String             @unique
  client                Client             @relation(fields: [clientId], references: [id], onDelete: Cascade)

  // Stripe
  stripeCustomerId      String             @unique
  stripeSubscriptionId  String?            @unique
  stripePriceId         String?

  // Plan
  plan                  String             @default("starter") // starter, pro, agency
  status                SubscriptionStatus @default(ACTIVE)

  // Limites
  articlesPerMonth      Int                @default(10)
  articlesUsed          Int                @default(0)
  usersLimit            Int                @default(1)

  // Frais de setup
  setupFee              Int?               // En centimes
  setupPaidAt           DateTime?

  // Période
  currentPeriodStart    DateTime?
  currentPeriodEnd      DateTime?
  canceledAt            DateTime?

  createdAt             DateTime           @default(now())
  updatedAt             DateTime           @updatedAt
}

// ============================================
// TRACKING USAGE & COÛTS
// ============================================

model UsageLog {
  id            String    @id @default(cuid())

  clientId      String
  client        Client    @relation(fields: [clientId], references: [id], onDelete: Cascade)

  type          UsageType
  inputTokens   Int       @default(0)
  outputTokens  Int       @default(0)
  cost          Float     // En euros

  articleId     String?
  metadata      Json?

  createdAt     DateTime  @default(now())
}
```

---

## 🔐 MIDDLEWARE D'AUTHENTIFICATION

Fichier `middleware.ts` à la racine :

```typescript
import { clerkMiddleware, createRouteMatcher } from '@clerk/nextjs/server'
import { NextResponse } from 'next/server'

const isPublicRoute = createRouteMatcher([
  '/',
  '/sign-in(.*)',
  '/sign-up(.*)',
  '/api/webhooks(.*)',
])

const isAdminRoute = createRouteMatcher(['/admin(.*)'])
const isDashboardRoute = createRouteMatcher(['/dashboard(.*)'])

export default clerkMiddleware(async (auth, req) => {
  // Routes publiques
  if (isPublicRoute(req)) {
    return NextResponse.next()
  }

  const { userId, sessionClaims } = await auth()

  // Non authentifié
  if (!userId) {
    const signInUrl = new URL('/sign-in', req.url)
    signInUrl.searchParams.set('redirect_url', req.url)
    return NextResponse.redirect(signInUrl)
  }

  const role = sessionClaims?.metadata?.role as string | undefined

  // Route admin - vérifier SUPER_ADMIN
  if (isAdminRoute(req)) {
    if (role !== 'SUPER_ADMIN') {
      return NextResponse.redirect(new URL('/dashboard', req.url))
    }
  }

  // Route dashboard - vérifier accès client
  if (isDashboardRoute(req)) {
    // Les SUPER_ADMIN peuvent aussi accéder au dashboard
    if (role === 'SUPER_ADMIN') {
      return NextResponse.next()
    }
    // Les autres rôles CLIENT_* ont accès
    if (!role?.startsWith('CLIENT_')) {
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

## 🤖 SERVICE IA (CLAUDE)

Fichier `src/lib/services/ai.ts` :

```typescript
import Anthropic from '@anthropic-ai/sdk'
import { prisma } from '@/lib/db'

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY!,
})

// Tarifs Claude Sonnet (à ajuster)
const COST_PER_1K_INPUT_TOKENS = 0.003  // $0.003
const COST_PER_1K_OUTPUT_TOKENS = 0.015 // $0.015

interface GenerateArticleInput {
  clientId: string
  topic: string
  keyword: string
  length: 'short' | 'medium' | 'long'
  promptId?: string
}

interface GenerateResult {
  title: string
  slug: string
  content: string
  excerpt: string
  metaTitle: string
  metaDescription: string
  tokensUsed: { input: number; output: number }
  cost: number
}

export async function generateArticle(input: GenerateArticleInput): Promise<GenerateResult> {
  // Récupérer le client et son prompt
  const client = await prisma.client.findUnique({
    where: { id: input.clientId },
    include: {
      prompts: {
        where: input.promptId
          ? { id: input.promptId }
          : { isDefault: true },
        take: 1,
      },
    },
  })

  if (!client) throw new Error('Client non trouvé')

  const prompt = client.prompts[0]
  if (!prompt) throw new Error('Aucun prompt configuré pour ce client')

  // Construire les prompts
  const wordCount = input.length === 'short' ? 800 : input.length === 'medium' ? 1500 : 2500

  const systemPrompt = `${prompt.systemPrompt}

CONTEXTE CLIENT:
- Entreprise: ${client.name}
- Site web: ${client.website}
- Ton de la marque: ${client.brandVoice || 'Professionnel et accessible'}
- Audience cible: ${client.targetAudience || 'Professionnels'}
- Mots-clés du secteur: ${client.keywords.join(', ') || 'Non définis'}

FORMAT DE RÉPONSE OBLIGATOIRE (respecte exactement ce format):
---TITLE---
[Titre optimisé SEO, 50-60 caractères, contenant le mot-clé]
---META_TITLE---
[Meta title, peut être identique au titre ou légèrement différent]
---META_DESCRIPTION---
[Meta description engageante, 150-160 caractères, avec mot-clé]
---EXCERPT---
[Résumé de 2-3 phrases pour les aperçus]
---CONTENT---
[Contenu complet de l'article en Markdown]`

  const userPrompt = prompt.userPrompt
    .replace(/\{\{topic\}\}/g, input.topic)
    .replace(/\{\{keyword\}\}/g, input.keyword)
    .replace(/\{\{length\}\}/g, String(wordCount))

  // Appel API Claude
  const response = await anthropic.messages.create({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 4096,
    system: systemPrompt,
    messages: [{ role: 'user', content: userPrompt }],
  })

  const textContent = response.content[0]
  if (textContent.type !== 'text') {
    throw new Error('Réponse inattendue de Claude')
  }

  // Parser la réponse
  const parsed = parseArticleResponse(textContent.text)

  // Calculer les coûts
  const inputTokens = response.usage.input_tokens
  const outputTokens = response.usage.output_tokens
  const cost = (inputTokens / 1000) * COST_PER_1K_INPUT_TOKENS +
               (outputTokens / 1000) * COST_PER_1K_OUTPUT_TOKENS

  // Logger l'usage
  await prisma.usageLog.create({
    data: {
      clientId: input.clientId,
      type: 'GENERATION',
      inputTokens,
      outputTokens,
      cost,
      metadata: { topic: input.topic, keyword: input.keyword },
    },
  })

  return {
    ...parsed,
    tokensUsed: { input: inputTokens, output: outputTokens },
    cost,
  }
}

function parseArticleResponse(text: string): Omit<GenerateResult, 'tokensUsed' | 'cost'> {
  const extractSection = (marker: string): string => {
    const regex = new RegExp(`---${marker}---\\s*([\\s\\S]*?)(?=---[A-Z_]+---|$)`)
    const match = text.match(regex)
    return match ? match[1].trim() : ''
  }

  const title = extractSection('TITLE')

  return {
    title,
    slug: generateSlug(title),
    metaTitle: extractSection('META_TITLE') || title,
    metaDescription: extractSection('META_DESCRIPTION'),
    excerpt: extractSection('EXCERPT'),
    content: extractSection('CONTENT'),
  }
}

function generateSlug(title: string): string {
  return title
    .toLowerCase()
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '')
    .replace(/[^a-z0-9]+/g, '-')
    .replace(/^-|-$/g, '')
    .slice(0, 100)
}

// Analyse SEO
export async function analyzeSEO(content: string, targetKeyword: string): Promise<{
  score: number
  issues: Array<{
    type: string
    priority: string
    title: string
    description: string
    suggestion?: string
  }>
}> {
  const response = await anthropic.messages.create({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 2048,
    system: `Tu es un expert SEO. Analyse l'article et retourne UNIQUEMENT un JSON valide (pas de texte avant ou après).

Format:
{
  "score": <number 0-100>,
  "issues": [
    {
      "type": "<MISSING_KEYWORD|TITLE_TOO_SHORT|META_MISSING|CONTENT_TOO_SHORT|NO_H2|etc>",
      "priority": "<LOW|MEDIUM|HIGH|CRITICAL>",
      "title": "<titre court>",
      "description": "<explication>",
      "suggestion": "<suggestion de correction>"
    }
  ]
}`,
    messages: [{
      role: 'user',
      content: `Mot-clé cible: ${targetKeyword}\n\nArticle:\n${content}`,
    }],
  })

  const textContent = response.content[0]
  if (textContent.type !== 'text') throw new Error('Erreur analyse')

  return JSON.parse(textContent.text)
}
```

---

## 📧 SERVICE EMAIL (RESEND)

Fichier `src/lib/services/email.ts` :

```typescript
import { Resend } from 'resend'

const resend = new Resend(process.env.RESEND_API_KEY)

export async function sendOnboardingEmail(params: {
  to: string
  clientName: string
  loginUrl: string
  tempPassword?: string
}) {
  await resend.emails.send({
    from: 'SEO Platform <onboarding@votredomaine.com>',
    to: params.to,
    subject: `Bienvenue sur la plateforme SEO - ${params.clientName}`,
    html: `
      <h1>Bienvenue ${params.clientName} !</h1>
      <p>Votre espace de rédaction SEO est prêt.</p>
      <p><a href="${params.loginUrl}">Connectez-vous ici</a></p>
      ${params.tempPassword ? `<p>Mot de passe temporaire: ${params.tempPassword}</p>` : ''}
      <p>À bientôt !</p>
    `,
  })
}

export async function sendInviteEmail(params: {
  to: string
  inviterName: string
  clientName: string
  inviteUrl: string
}) {
  await resend.emails.send({
    from: 'SEO Platform <invites@votredomaine.com>',
    to: params.to,
    subject: `Invitation à rejoindre ${params.clientName}`,
    html: `
      <h1>Vous êtes invité !</h1>
      <p>${params.inviterName} vous invite à rejoindre l'équipe ${params.clientName}.</p>
      <p><a href="${params.inviteUrl}">Accepter l'invitation</a></p>
      <p>Ce lien expire dans 7 jours.</p>
    `,
  })
}

export async function sendQuotaWarningEmail(params: {
  to: string
  clientName: string
  used: number
  limit: number
}) {
  await resend.emails.send({
    from: 'SEO Platform <alerts@votredomaine.com>',
    to: params.to,
    subject: `Quota bientôt atteint - ${params.clientName}`,
    html: `
      <h1>Attention : Quota bientôt atteint</h1>
      <p>Vous avez utilisé ${params.used}/${params.limit} articles ce mois-ci.</p>
      <p>Contactez-nous pour augmenter votre limite.</p>
    `,
  })
}
```

---

## 🔗 SERVICES CMS

Fichier `src/lib/services/cms/wordpress.ts` :

```typescript
interface WordPressConfig {
  siteUrl: string
  username: string
  applicationPassword: string
}

interface PublishResult {
  success: boolean
  postId?: string
  postUrl?: string
  error?: string
}

export async function testConnection(config: WordPressConfig): Promise<boolean> {
  try {
    const response = await fetch(`${config.siteUrl}/wp-json/wp/v2/users/me`, {
      headers: {
        Authorization: `Basic ${Buffer.from(`${config.username}:${config.applicationPassword}`).toString('base64')}`,
      },
    })
    return response.ok
  } catch {
    return false
  }
}

export async function publishPost(
  config: WordPressConfig,
  article: {
    title: string
    content: string
    excerpt?: string
    slug: string
    status?: 'publish' | 'draft'
  }
): Promise<PublishResult> {
  try {
    const response = await fetch(`${config.siteUrl}/wp-json/wp/v2/posts`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Basic ${Buffer.from(`${config.username}:${config.applicationPassword}`).toString('base64')}`,
      },
      body: JSON.stringify({
        title: article.title,
        content: article.content,
        excerpt: article.excerpt,
        slug: article.slug,
        status: article.status || 'publish',
      }),
    })

    if (!response.ok) {
      const error = await response.text()
      return { success: false, error }
    }

    const data = await response.json()
    return {
      success: true,
      postId: String(data.id),
      postUrl: data.link,
    }
  } catch (error) {
    return { success: false, error: String(error) }
  }
}
```

---

## 📅 CRON JOBS (Vercel)

Fichier `vercel.json` :

```json
{
  "crons": [
    {
      "path": "/api/cron/process-queue",
      "schedule": "*/5 * * * *"
    },
    {
      "path": "/api/cron/auto-publish",
      "schedule": "0 9 * * *"
    },
    {
      "path": "/api/cron/weekly-analyze",
      "schedule": "0 3 * * 1"
    }
  ]
}
```

Exemple route cron `src/app/api/cron/auto-publish/route.ts` :

```typescript
import { NextRequest, NextResponse } from 'next/server'
import { prisma } from '@/lib/db'
import { publishToCMS } from '@/lib/services/cms'

export async function GET(req: NextRequest) {
  // Vérifier le secret
  const authHeader = req.headers.get('authorization')
  if (authHeader !== `Bearer ${process.env.CRON_SECRET}`) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  // Récupérer les articles planifiés pour maintenant ou avant
  const articles = await prisma.article.findMany({
    where: {
      status: 'SCHEDULED',
      scheduledAt: { lte: new Date() },
    },
    include: { client: true },
  })

  const results = []

  for (const article of articles) {
    try {
      const result = await publishToCMS(article.client, article)

      await prisma.article.update({
        where: { id: article.id },
        data: {
          status: 'PUBLISHED',
          publishedAt: new Date(),
          cmsPostId: result.postId,
          cmsPostUrl: result.postUrl,
        },
      })

      results.push({ id: article.id, success: true })
    } catch (error) {
      results.push({ id: article.id, success: false, error: String(error) })
    }
  }

  return NextResponse.json({ processed: results.length, results })
}
```

---

## 🎨 COMPOSANTS UI ESSENTIELS

### Score SEO (`src/components/dashboard/SEOScore.tsx`)

```tsx
interface SEOScoreProps {
  score: number
  size?: 'sm' | 'md' | 'lg'
}

export function SEOScore({ score, size = 'md' }: SEOScoreProps) {
  const getColor = () => {
    if (score >= 80) return 'text-green-500'
    if (score >= 60) return 'text-yellow-500'
    return 'text-red-500'
  }

  const getLabel = () => {
    if (score >= 80) return 'Excellent'
    if (score >= 60) return 'Bon'
    if (score >= 40) return 'À améliorer'
    return 'Faible'
  }

  const sizeClasses = {
    sm: 'w-12 h-12 text-lg',
    md: 'w-20 h-20 text-2xl',
    lg: 'w-28 h-28 text-4xl',
  }

  return (
    <div className="flex flex-col items-center gap-2">
      <div className={`${sizeClasses[size]} rounded-full border-4 ${getColor()} border-current flex items-center justify-center font-bold`}>
        {score}
      </div>
      <span className={`text-sm font-medium ${getColor()}`}>{getLabel()}</span>
    </div>
  )
}
```

### Carte Article (`src/components/dashboard/ArticleCard.tsx`)

```tsx
import { Card, CardHeader, CardTitle, CardContent, CardFooter } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import { SEOScore } from './SEOScore'
import Link from 'next/link'

interface ArticleCardProps {
  article: {
    id: string
    title: string
    status: string
    seoScore?: number | null
    targetKeyword: string
    createdAt: Date
    scheduledAt?: Date | null
    publishedAt?: Date | null
  }
}

export function ArticleCard({ article }: ArticleCardProps) {
  const statusColors: Record<string, string> = {
    DRAFT: 'bg-gray-500',
    REVIEW: 'bg-yellow-500',
    APPROVED: 'bg-blue-500',
    SCHEDULED: 'bg-purple-500',
    PUBLISHED: 'bg-green-500',
    NEEDS_UPDATE: 'bg-orange-500',
  }

  return (
    <Card>
      <CardHeader className="flex flex-row items-start justify-between">
        <div>
          <CardTitle className="text-lg">{article.title}</CardTitle>
          <p className="text-sm text-muted-foreground mt-1">
            Mot-clé: {article.targetKeyword}
          </p>
        </div>
        {article.seoScore && <SEOScore score={article.seoScore} size="sm" />}
      </CardHeader>
      <CardContent>
        <div className="flex items-center gap-2">
          <Badge className={statusColors[article.status]}>
            {article.status}
          </Badge>
          <span className="text-sm text-muted-foreground">
            {article.publishedAt
              ? `Publié le ${new Date(article.publishedAt).toLocaleDateString()}`
              : article.scheduledAt
              ? `Planifié pour le ${new Date(article.scheduledAt).toLocaleDateString()}`
              : `Créé le ${new Date(article.createdAt).toLocaleDateString()}`
            }
          </span>
        </div>
      </CardContent>
      <CardFooter className="gap-2">
        <Button variant="outline" size="sm" asChild>
          <Link href={`/dashboard/articles/${article.id}`}>Voir</Link>
        </Button>
        <Button variant="outline" size="sm" asChild>
          <Link href={`/dashboard/articles/${article.id}/edit`}>Éditer</Link>
        </Button>
      </CardFooter>
    </Card>
  )
}
```

---

## 🚀 ORDRE D'IMPLÉMENTATION

### Phase 1: Setup (Jour 1-2)
1. `npx create-next-app@latest seo-btob --typescript --tailwind --app`
2. Installer dépendances (voir package.json)
3. Setup Prisma + Supabase
4. Setup Clerk
5. Configurer middleware.ts
6. Installer Shadcn UI (`npx shadcn-ui@latest init`)
7. Ajouter composants UI de base

### Phase 2: Admin - Clients (Jour 3-5)
1. Layout admin avec sidebar
2. Dashboard admin (stats globales)
3. CRUD Clients
4. Configuration CMS par client
5. CRUD Prompts par client
6. Gestion users par client

### Phase 3: Admin - Templates (Jour 6-7)
1. CRUD Templates prompts globaux
2. Catégorisation
3. Duplication vers clients

### Phase 4: Client - Rédaction (Jour 8-12)
1. Layout dashboard client
2. Dashboard client
3. Liste articles
4. Génération article (1 seul)
5. Preview + Score SEO
6. Éditeur article
7. Publication vers CMS

### Phase 5: Client - Bulk (Jour 13-15)
1. Page import CSV
2. Parser CSV
3. Création GenerationJob
4. File d'attente UI
5. Cron traitement queue
6. Notifications progression

### Phase 6: Client - Calendrier (Jour 16-18)
1. Vue calendrier (mois)
2. Drag & drop articles
3. Planification
4. Cron auto-publish

### Phase 7: Client - Optimisation (Jour 19-21)
1. Analyse SEO (service)
2. Liste recommandations
3. Détail optimisation
4. Appliquer suggestion
5. Cron analyse hebdo

### Phase 8: Client - Équipe (Jour 22-24)
1. Liste membres
2. Invitations
3. Gestion rôles
4. Webhook Clerk sync

### Phase 9: Facturation (Jour 25-27)
1. Setup Stripe
2. Webhook Stripe
3. Gestion abonnements
4. Limites quotas
5. Vue admin revenus/coûts

### Phase 10: Emails & Polish (Jour 28-30)
1. Emails onboarding
2. Emails invitations
3. Emails alertes quota
4. Tests
5. Deploy Vercel

---

## ⚠️ POINTS D'ATTENTION

1. **Sécurité CMS credentials**: Toujours chiffrer avec `CMS_ENCRYPTION_KEY`
2. **Rate limiting Claude API**: Max 60 req/min, implémenter queue
3. **Isolation multi-tenant**: Toujours filtrer par `clientId`
4. **Clerk metadata**: Stocker le `role` dans `publicMetadata`
5. **Versioning articles**: Créer une version avant chaque modification
6. **Gestion erreurs**: Try/catch sur tous les appels API externes
7. **Logs usage**: Logger tous les appels Claude pour facturation

---

## 📝 COMMANDES UTILES

```bash
# Prisma
npx prisma generate          # Générer le client
npx prisma db push           # Pousser le schéma
npx prisma studio            # Interface visuelle DB
npx prisma db seed           # Seed data

# Shadcn UI
npx shadcn-ui@latest add button card input ...

# Dev
npm run dev                  # Lancer en dev
npm run build                # Build prod
npm run lint                 # Linter
```

---

C'est tout ! Avec ce fichier, Cursor a tout ce qu'il faut pour coder le projet complet.
