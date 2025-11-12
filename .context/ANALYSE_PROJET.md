# 📚 Analyse Complète du Projet ARM STACKER

> Document d'analyse pour webdesigner / Front-end Developer en apprentissage  
> Date: 2025

---

## 🎯 Vue d'ensemble

**ARM STACKER** est une plateforme e-commerce spécialisée dans la vente de produits audio (samples, kits de batterie, etc.). Le projet est structuré en **monorepo** avec une architecture séparant clairement le front-end (React) et le back-end (Express/Node.js).

### Type de projet
- **E-commerce B2C** pour produits audio numériques
- **Stack moderne** : React + TypeScript + Vite (front) / Express + Prisma + PostgreSQL (back)
- **Paiements** : Intégration Stripe avec support Apple Pay / Google Pay
- **Architecture** : Monorepo avec workspaces npm

---

## 🏗️ Architecture du Projet

### Structure Monorepo

```
armed/
├── apps/
│   ├── web/          # Application React (Front-end)
│   └── api/           # API Express (Back-end)
├── packages/
│   ├── types/         # Types TypeScript partagés
│   └── config/        # Configuration partagée (validation env)
└── package.json       # Workspace root
```

### Pourquoi un monorepo ?
- **Code partagé** : Types et configuration réutilisables entre front et back
- **Développement synchronisé** : Modifications front/back dans le même repo
- **Build unifié** : Scripts npm au niveau root pour orchestrer les builds

---

## 🛠️ Stack Technique Complète

### Front-end (`apps/web`)

#### Core
- **React 18.2.0** - Bibliothèque UI
- **TypeScript 5.6.3** - Typage statique
- **Vite 5.4.0** - Build tool et dev server (ultra-rapide)

#### Routing
- **React Router DOM 7.9.5** - Navigation SPA

#### State Management
- **Zustand 5.0.8** - Store léger pour le panier (alternative à Redux)

#### Styling
- **Tailwind CSS 4.1.17** - Framework CSS utility-first
- **@tailwindcss/vite** - Plugin Vite pour Tailwind
- **PostCSS 8.5.6** - Traitement CSS
- **Autoprefixer 10.4.21** - Préfixes navigateurs

#### Paiements
- **@stripe/react-stripe-js 5.3.0** - Composants React Stripe
- **@stripe/stripe-js 8.4.0** - SDK Stripe client

#### Dev Tools
- **ESLint 9.36.0** - Linter JavaScript/TypeScript
- **TypeScript ESLint 8.45.0** - Règles TypeScript pour ESLint

### Back-end (`apps/api`)

#### Core
- **Express 4.19.0** - Framework web Node.js
- **TypeScript 5.6.3** - Typage statique
- **tsx 4.19.0** - Exécution TypeScript directe (alternative à ts-node)

#### Base de données
- **Prisma 5.19.0** - ORM moderne pour PostgreSQL
- **PostgreSQL** - Base de données relationnelle (via DATABASE_URL)

#### Validation & Sécurité
- **Zod 3.25.76** - Validation de schémas runtime (alternative à Joi/Yup)
- **CORS 2.8.5** - Gestion CORS pour les requêtes cross-origin

#### Paiements
- **Stripe 19.3.0** - SDK Stripe serveur

#### Utilitaires
- **dotenv 16.4.5** - Variables d'environnement

### Packages partagés

#### `@arm/types`
Types TypeScript partagés entre front et back :
- `Product`, `Order`, `OrderItem`, `License`
- `Currency` (EUR, USD, GBP)

#### `@arm/config`
Validation des variables d'environnement avec Zod :
- `NODE_ENV`, `PORT`, `DATABASE_URL`, `API_BASE`, `JWT_SECRET`

---

## 📁 Structure Détaillée des Dossiers

### Front-end (`apps/web/src/`)

```
src/
├── components/          # Composants React réutilisables
│   ├── catalog/        # Composants catalogue (ErrorState, SkeletonCard)
│   ├── layout/         # Layout (Navbar, AppContainer)
│   ├── product/        # Composants produit (AudioPlayer, WaveformCanvas, LicenseSelector)
│   └── ui/             # Composants UI génériques (Button, Card, Input)
├── hooks/              # Hooks React personnalisés
│   ├── useProducts.ts  # Fetch liste produits avec pagination
│   ├── useProduct.ts   # Fetch produit unique par slug
│   └── useAudioPlayer.ts # Gestion lecteur audio
├── pages/              # Pages/écrans de l'application
│   ├── Home.tsx
│   ├── Catalog.tsx
│   ├── Product.tsx
│   ├── Cart.tsx
│   ├── Checkout.tsx
│   ├── CheckoutConfirmation.tsx
│   ├── CheckoutStripe.tsx
│   ├── Account.tsx
│   └── Legal.tsx
├── store/              # State management (Zustand)
│   └── cart.ts         # Store panier avec persistance localStorage
├── utils/              # Utilitaires
│   ├── api.ts          # Helpers pour construire URLs API
│   ├── format.ts       # Formatage (prix, dates, etc.)
│   └── waveformMock.ts # Génération waveform mock
├── validation/         # Schémas de validation (Zod)
│   └── checkout.ts
├── App.tsx             # Composant racine avec routing
├── main.tsx            # Point d'entrée React
├── index.css           # Styles globaux + import Tailwind
└── theme.css           # Variables de thème
```

### Back-end (`apps/api/src/`)

```
src/
├── routes/             # Routes Express (endpoints API)
│   ├── products.ts     # GET /products (liste paginée)
│   ├── productDetails.ts # GET /products/:slug
│   ├── checkout.ts     # POST /checkout/init (validation panier)
│   ├── payments.ts     # POST /payments/init (création PaymentIntent Stripe)
│   └── stripeWebhook.ts # POST /stripe/webhook (événements Stripe)
├── services/           # Services métier
│   └── stripe.ts      # Wrapper Stripe (createPaymentIntent)
├── prismaClient.ts    # Instance Prisma Client
├── index.ts           # Point d'entrée API (setup Express)
└── server.ts          # (ancien fichier, maintenant index.ts)
```

### Base de données (`apps/api/prisma/`)

```
prisma/
├── schema.prisma       # Schéma Prisma (modèles, relations)
├── migrations/         # Migrations SQL
└── seed.ts            # Script de seed (données de test)
```

---

## 🎨 Design System & Styling

### Thème
- **Mode sombre** par défaut (`color-scheme: dark`)
- **Couleurs principales** :
  - Background : `#1D1D1D` (neutral-900)
  - Text : `#F3F3F3` (neutral-100)
  - Accent : Violet (`violet-600`, `violet-400`)
  - Borders : `neutral-700`, `neutral-500`

### Tailwind CSS
- **Configuration** : Via `@tailwindcss/vite` (plugin Vite)
- **Approche** : Utility-first (classes inline)
- **Responsive** : Breakpoints par défaut (sm, md, lg, xl)
- **Customisation** : Variables CSS dans `theme.css` si besoin

### Composants UI
- **Button** : Variantes (primary violet, secondary neutral)
- **Card** : Conteneurs avec bordures arrondies
- **Input** : Champs de formulaire avec style dark
- **SkeletonCard** : Loading state pour le catalogue

---

## 🔄 Flux de Données

### 1. Affichage Catalogue
```
Catalog.tsx
  → useProducts({ page, pageSize, sort, order })
    → fetch('http://localhost:4000/products?...')
      → API: getProducts()
        → Prisma: product.findMany()
          → Response JSON { data, meta }
```

### 2. Ajout au Panier
```
Product.tsx
  → useCart.addItem({ productId, slug, title, priceCents, licenseType, qty })
    → Zustand store (cart.ts)
      → localStorage.setItem('arm_cart_v1', ...)
        → Re-render Navbar (badge quantité)
```

### 3. Checkout & Paiement
```
Checkout.tsx
  → POST /payments/init (items, totalCents, email)
    → API: initPayment()
      → Stripe: createPaymentIntent()
        → Response: { clientSecret }
          → Stripe Elements (CardElement / PaymentRequestButton)
            → confirmCardPayment(clientSecret)
              → Webhook Stripe → Order créé en DB
                → Redirect /checkout/confirmation
```

---

## 🗄️ Modèle de Données (Prisma)

### Modèles principaux

#### `Product`
```prisma
- id: String (UUID)
- slug: String (unique) - URL-friendly identifier
- title: String
- priceCents: Int
- currency: String (default: "EUR")
- tags: String[]
- bpm: Int? (optionnel)
- key: String? (optionnel)
- description: String?
- coverUrl: String? (image de couverture)
- previewUrl: String? (fichier audio preview)
- durationSec: Int?
- waveformData: Json? (données waveform normalisées)
- createdAt, updatedAt: DateTime
```

#### `Order`
```prisma
- id: String (UUID)
- buyerEmail: String
- totalCents: Int
- currency: String
- status: OrderStatus (PENDING, PAID, SHIPPED, CANCELLED)
- items: OrderItem[] (relation)
- createdAt, updatedAt: DateTime
```

#### `OrderItem`
```prisma
- id: String (UUID)
- orderId: String (FK → Order)
- productId: String (FK → Product)
- priceCents: Int (prix capturé au moment de l'achat)
- currency: String
- licenseType: LicenseType (STANDARD, EXTENDED)
```

#### `ProductLicense`
```prisma
- id: String (UUID)
- productId: String (FK → Product)
- type: LicenseType (STANDARD, EXTENDED)
- priceCents: Int
- currency: String
```

### Relations
- `Product` → `OrderItem[]` (1-to-many)
- `Product` → `ProductLicense[]` (1-to-many)
- `Order` → `OrderItem[]` (1-to-many)
- `OrderItem` → `Product` (many-to-1)
- `OrderItem` → `Order` (many-to-1)

---

## 🔌 API Endpoints

### Produits
- `GET /products` - Liste paginée
  - Query params: `page`, `pageSize`, `sort` (createdAt|price), `order` (asc|desc)
  - Response: `{ data: Product[], meta: { page, pageSize, total, totalPages } }`

- `GET /products/:slug` - Détails produit
  - Response: `{ id, slug, title, priceCents, licenses: ProductLicense[], ... }`

### Checkout
- `POST /checkout/init` - Validation panier (mock)
  - Body: `{ email, country, items: CartItem[] }`
  - Response: `{ ok, confirmationId, totalCents, currency }`

### Paiements
- `POST /payments/init` - Création PaymentIntent Stripe
  - Body: `{ amountCents, currency, email?, items: CartItem[] }`
  - Response: `{ clientSecret: string }`

- `POST /stripe/webhook` - Webhook Stripe (événements paiement)
  - Crée `Order` en DB quand paiement réussi

### Health
- `GET /health` - Health check (ping DB)

---

## 🛒 Gestion du Panier (Zustand)

### Store (`store/cart.ts`)

**State** :
```typescript
{
  items: CartItem[],
  addItem: (payload) => void,
  removeItem: (id) => void,
  setQty: (id, qty) => void,
  setLicense: (id, licenseType, priceCents) => void,
  clear: () => void,
  totalCents: () => number,
  totalQty: () => number
}
```

**Persistance** :
- Sauvegarde automatique dans `localStorage` (`arm_cart_v1`)
- Hydratation au chargement de l'app
- Fusion intelligente : même produit + même licence = incrémente qty

**Usage** :
```typescript
const addItem = useCart((s) => s.addItem);
const totalQty = useCart((s) => s.totalQty()); // Selector (re-render si change)
```

---

## 🎵 Fonctionnalités Audio

### AudioPlayer
- Lecteur audio avec contrôles (play/pause, seek)
- Waveform visuelle (WaveformCanvas)
- Preview audio depuis `previewUrl` (fichier MP3/WAV)

### Waveform
- Données : `waveformData` (JSON array de nombres 0..1)
- Affichage : Canvas avec barres verticales
- Fallback : `generateWaveformMock()` si pas de données

---

## 💳 Intégration Stripe

### Configuration
- **Clés** : `VITE_STRIPE_PUBLISHABLE_KEY` (front) / `STRIPE_SECRET_KEY` (back)
- **HTTPS requis** : Pour Apple Pay / Google Pay (mkcert en dev)

### Flow
1. **Init PaymentIntent** : `POST /payments/init` → `clientSecret`
2. **Stripe Elements** :
   - `CardElement` : Saisie carte classique
   - `PaymentRequestButtonElement` : Apple Pay / Google Pay
3. **Confirmation** : `stripe.confirmCardPayment(clientSecret, ...)`
4. **Webhook** : Événement `payment_intent.succeeded` → création `Order`

### Support Apple Pay / Google Pay
- Nécessite HTTPS (mkcert pour dev local)
- Détection automatique via `paymentRequest.canMakePayment()`
- Affichage conditionnel du bouton

---

## 🚀 Scripts & Commandes

### Root (`package.json`)
```bash
npm run build        # Build packages partagés
npm run dev:web      # Démarrer front (port 5173)
npm run dev:api      # Démarrer back (port 4000)
```

### Front (`apps/web/package.json`)
```bash
npm run dev          # Vite dev server (HTTPS si certificats mkcert)
npm run build        # Build production
npm run preview      # Preview build
```

### Back (`apps/api/package.json`)
```bash
npm run dev          # tsx src/index.ts (hot reload)
npm run build        # tsc compilation
npm run prisma:migrate   # Prisma migrate dev
npm run prisma:generate  # Prisma generate (client)
npm run prisma:seed      # Seed DB
npm run db:test          # Test connexion DB
```

---

## 🔧 Configuration & Setup

### Variables d'environnement

#### Front (`apps/web/.env`)
```env
VITE_API_URL=http://localhost:4000
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_...
```

#### Back (`apps/api/.env`)
```env
PORT=4000
DATABASE_URL=postgresql://user:password@localhost:5432/armed
STRIPE_SECRET_KEY=sk_test_...
NODE_ENV=development
```

### HTTPS en développement
Pour activer Apple Pay / Google Pay en local :
1. Installer mkcert : `brew install mkcert`
2. Installer CA : `mkcert -install`
3. Générer certificats : `cd apps/web && mkcert localhost`
4. Redémarrer Vite

Vite détecte automatiquement les certificats (`localhost+2.pem` / `localhost+2-key.pem`).

### Base de données
1. PostgreSQL en cours d'exécution
2. `DATABASE_URL` configurée dans `.env`
3. `npm run prisma:migrate` (créer tables)
4. `npm run prisma:generate` (générer client)
5. `npm run prisma:seed` (optionnel, données de test)

---

## 📝 Patterns & Bonnes Pratiques

### Front-end

#### Hooks personnalisés
- `useProducts`, `useProduct` : Fetch data avec loading/error states
- `useAudioPlayer` : Logique lecteur audio réutilisable

#### State Management
- **Zustand** : Store simple pour panier (pas besoin de Redux)
- **Selectors** : `useCart((s) => s.totalQty())` pour optimiser re-renders

#### Composants
- **Composition** : Composants petits et réutilisables
- **Props typées** : TypeScript pour toutes les props
- **Loading states** : SkeletonCard pour UX fluide

#### Routing
- **React Router** : Navigation SPA
- **Routes protégées** : (à implémenter si besoin auth)

### Back-end

#### Validation
- **Zod** : Validation runtime des payloads
- **Schémas réutilisables** : Même logique front/back possible

#### Base de données
- **Prisma** : Type-safe queries
- **Migrations** : Versioning du schéma
- **Relations** : Gestion automatique des FK

#### API
- **REST** : Endpoints standards (GET, POST)
- **Error handling** : Try/catch avec messages clairs
- **CORS** : Configuré pour dev/prod

---

## 🎯 Points d'Entrée pour Webdesigner / Front Dev

### 1. Modifier le Design
- **Couleurs** : `apps/web/src/index.css` (variables CSS) + classes Tailwind
- **Composants UI** : `apps/web/src/components/ui/` (Button, Card, Input)
- **Layout** : `apps/web/src/components/layout/` (Navbar, AppContainer)
- **Pages** : `apps/web/src/pages/` (structure des écrans)

### 2. Ajouter une Page
1. Créer composant dans `pages/`
2. Ajouter route dans `App.tsx` : `<Route path="/nouvelle-page" element={<NouvellePage />} />`
3. Ajouter lien dans `Navbar.tsx` si besoin

### 3. Modifier le Catalogue
- **Affichage** : `pages/Catalog.tsx`
- **Card produit** : Modifier le JSX dans la map
- **Pagination** : Logique déjà implémentée
- **Tri** : Dropdowns déjà fonctionnels

### 4. Personnaliser le Panier
- **Store** : `store/cart.ts` (logique)
- **Affichage** : `pages/Cart.tsx` (UI)
- **Badge** : `components/layout/Navbar.tsx`

### 5. Styling avec Tailwind
```tsx
// Classes utilitaires
<div className="rounded-lg border border-neutral-700 p-4 bg-neutral-900">
  <h1 className="text-2xl font-semibold text-violet-400">Titre</h1>
</div>

// Responsive
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
```

### 6. Ajouter un Composant
1. Créer fichier dans `components/ui/` ou `components/[categorie]/`
2. Exporter composant avec props typées
3. Importer et utiliser dans pages/composants

---

## 🔍 Fichiers Clés à Connaître

### Front
- `App.tsx` - Routing principal
- `store/cart.ts` - Logique panier
- `hooks/useProducts.ts` - Fetch produits
- `pages/Product.tsx` - Page produit (exemple complet)
- `components/product/AudioPlayer.tsx` - Lecteur audio

### Back
- `src/index.ts` - Setup Express + routes
- `prisma/schema.prisma` - Modèle de données
- `routes/products.ts` - Endpoint produits
- `routes/payments.ts` - Endpoint Stripe

---

## 🐛 Debugging

### Front
- **Console browser** : Erreurs React, logs fetch
- **React DevTools** : Inspecter composants, props, state
- **Network tab** : Vérifier requêtes API

### Back
- **Console terminal** : Logs Express, erreurs Prisma
- **Prisma Studio** : `npx prisma studio` (GUI DB)
- **Stripe Dashboard** : Voir PaymentIntents, webhooks

---

## 📚 Ressources & Documentation

### Technologies
- [React](https://react.dev/)
- [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Zustand](https://zustand-demo.pmnd.rs/)
- [React Router](https://reactrouter.com/)
- [Prisma](https://www.prisma.io/docs)
- [Stripe](https://stripe.com/docs)
- [Zod](https://zod.dev/)

### Outils
- [mkcert](https://github.com/FiloSottile/mkcert) - Certificats HTTPS locaux
- [PostgreSQL](https://www.postgresql.org/docs/)

---

## 🎓 Concepts à Maîtriser

### Front-end
- **React Hooks** : useState, useEffect, custom hooks
- **TypeScript** : Types, interfaces, génériques
- **Tailwind CSS** : Utility classes, responsive
- **State Management** : Zustand (ou Redux si migration)
- **Routing** : React Router (nested routes, params)

### Back-end
- **REST API** : GET, POST, status codes
- **Prisma** : Queries, relations, migrations
- **Validation** : Zod schemas
- **Webhooks** : Stripe events

### Général
- **Monorepo** : Workspaces npm
- **Environment variables** : .env files
- **HTTPS** : Certificats, mixed content
- **Database** : Relations, migrations, seeds

---

## 🚧 Améliorations Possibles

### Front
- [ ] Tests unitaires (Vitest)
- [ ] Tests E2E (Playwright)
- [ ] Optimisation images (lazy loading)
- [ ] PWA (service worker, offline)
- [ ] Internationalisation (i18n)
- [ ] Dark/Light mode toggle

### Back
- [ ] Authentification (JWT, sessions)
- [ ] Rate limiting
- [ ] Logging structuré (Winston, Pino)
- [ ] Tests API (Jest, Supertest)
- [ ] Documentation API (Swagger/OpenAPI)

### Infrastructure
- [ ] CI/CD (GitHub Actions)
- [ ] Docker (containerisation)
- [ ] Monitoring (Sentry, LogRocket)

---

## 📞 Support & Questions

Pour toute question sur le projet :
1. Consulter ce document
2. Explorer le code (bien commenté)
3. Vérifier la documentation des librairies
4. Demander à l'équipe

---

**Bon développement ! 🚀**

