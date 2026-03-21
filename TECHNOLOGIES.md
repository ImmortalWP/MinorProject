# HamroPhone — Technology Stack & Tools Documentation

> **Project:** HamroPhone — Nepal's Premium Second-Hand Phone Marketplace  
> **Last Updated:** March 18, 2026

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Frontend Technologies](#2-frontend-technologies)
3. [Backend Technologies](#3-backend-technologies)
4. [Database](#4-database)
5. [Authentication & Security](#5-authentication--security)
6. [Email Service](#6-email-service)
7. [AI / Machine Learning Model](#7-ai--machine-learning-model)
8. [Styling & Design](#8-styling--design)
9. [Build & Dev Tooling](#9-build--dev-tooling)
10. [Project Structure](#10-project-structure)
11. [Environment Variables](#11-environment-variables)
12. [API Endpoints](#12-api-endpoints)
13. [Data Models](#13-data-models)
14. [Python Dependencies](#14-python-dependencies)
15. [Node.js Dependencies](#15-nodejs-dependencies)

---

## 1. Project Overview

**HamroPhone** is a full-stack web application that serves as a marketplace for buying and selling second-hand mobile phones in Nepal. It features an integrated **AI-powered price prediction model** that automatically suggests fair market prices based on phone specifications and condition.

### Key Features

- **Browse & Buy** used phones with a beautiful, dark-themed UI
- **Sell Phones** with an AI model that calculates the best market price
- **User Authentication** with email verification
- **Image Upload** for device photos
- **Profile Management** with personal listings
- **Responsive Design** across desktop and mobile

---

## 2. Frontend Technologies

| Technology | Version | Purpose |
|---|---|---|
| **React** | `19.2.3` | Core UI library for building component-based user interfaces |
| **Next.js** | `16.1.6` | Full-stack React framework — provides file-based routing (App Router), server-side rendering (SSR), API routes, and optimized builds |
| **TypeScript** | `^5` | Statically typed superset of JavaScript for type-safe code |
| **Framer Motion** | `^12.35.2` | Animation library for React — used for page transitions, hover effects, scroll animations, and micro-interactions |
| **Lucide React** | `^0.577.0` | Modern icon library — provides all icons used across the app (e.g. `Smartphone`, `Battery`, `Camera`, `Loader2`, `User`, `Menu`, `LogOut`, etc.) |

### Frontend Architecture

- **App Router** (Next.js 16): Uses the `app/` directory structure for file-based routing
- **Client Components** (`"use client"`): Used for interactive pages like Sell, Login, Signup, Profile, Buy
- **Server Components**: Used for layouts, metadata, and static pages
- **Google Fonts**: `Outfit` font loaded via `next/font/google` — set as the global sans-serif font via CSS variable `--font-outfit`

### Pages

| Route | File | Description |
|---|---|---|
| `/` | `app/page.tsx` | Home page — Hero, Brands Carousel, Mission, Reviews, Footer |
| `/buy` | `app/buy/page.tsx` | Browse and filter phone listings |
| `/sell` | `app/sell/page.tsx` | Sell your phone form with AI price prediction |
| `/login` | `app/login/page.tsx` | User login page |
| `/signup` | `app/signup/page.tsx` | User registration page |
| `/profile` | `app/profile/page.tsx` | User profile with personal listings |

### UI Components

| Component | Path | Description |
|---|---|---|
| `Navbar` | `components/Navbar.tsx` | Fixed responsive navbar with auth-aware login/profile state, mobile hamburger menu, scroll-based blur effect |
| `Hero` | `components/Hero/index.tsx` | Landing page hero section |
| `BrandsCarousel` | `components/BrandsCarousel/index.tsx` | Scrollable brand logos carousel |
| `PhoneCard` | `components/PhoneCard/index.tsx` | Reusable phone listing card component |
| `Mission` | `components/Mission/index.tsx` | Mission/about section |
| `ReviewsCarousel` | `components/ReviewsCarousel/index.tsx` | Customer reviews carousel |
| `Footer` | `components/Footer/index.tsx` | Site footer |

---

## 3. Backend Technologies

| Technology | Version | Purpose |
|---|---|---|
| **Next.js API Routes** | `16.1.6` | Server-side API endpoints built within the Next.js App Router (`app/api/`) |
| **Node.js** | Runtime | Server runtime for executing API routes |

### API Architecture

The backend uses **Next.js Route Handlers** (files named `route.ts` inside the `app/api/` directory). Each endpoint exports functions for specific HTTP methods (`GET`, `POST`, `PUT`, `DELETE`, `PATCH`).

---

## 4. Database

| Technology | Version | Purpose |
|---|---|---|
| **MongoDB** | Cloud (Atlas) | NoSQL document database for storing users, phone listings, and all application data |
| **Mongoose** | `^9.2.4` | ODM (Object Data Modeling) library for MongoDB — provides schema definitions, validation, type-safe models, and query building |

### Database Connection

- Connection is managed in `lib/mongodb.ts`
- Uses a **global cache pattern** to prevent reconnection during Next.js hot reloads in development
- MongoDB connection string is stored in `.env.local` as `MONGODB_URI`
- Database hosted on **MongoDB Atlas** (cloud-hosted cluster)

---

## 5. Authentication & Security

| Technology | Version | Purpose |
|---|---|---|
| **JSON Web Tokens (JWT)** | `^9.0.3` via `jsonwebtoken` | Stateless authentication — tokens are signed with a secret and stored in HTTP-only cookies |
| **bcryptjs** | `^3.0.3` | Password hashing — securely hashes user passwords before storing in the database |

### Auth Flow

1. **Registration** (`/api/auth/register`) — User signs up → password is hashed with bcryptjs → verification email is sent
2. **Email Verification** (`/api/auth/verify-email`) — User clicks email link → account is marked as verified
3. **Login** (`/api/auth/login`) — Email + password checked → JWT token issued → stored as HTTP-only cookie
4. **Session Check** (`/api/auth/me`) — Reads JWT from cookie → returns authenticated user data
5. **Logout** (`/api/auth/logout`) — Clears the JWT cookie
6. **Password Change** (`/api/auth/change-password`) — Authenticated users can update their password

### Type Definitions

- `@types/bcryptjs` (`^2.4.6`) — TypeScript types for bcryptjs
- `@types/jsonwebtoken` (`^9.0.10`) — TypeScript types for jsonwebtoken

---

## 6. Email Service

| Technology | Version | Purpose |
|---|---|---|
| **Nodemailer** | `^8.0.2` | Email sending library for Node.js |
| **Gmail SMTP** | — | SMTP provider for sending transactional emails |

### Email Features

- **Verification Emails**: Sends beautifully styled HTML emails with a "Verify My Account" button when users register
- Configured in `lib/email.ts`
- Uses Gmail App Passwords for SMTP authentication
- Email template includes branded HTML with inline CSS styling
- Verification links expire after 24 hours

### Type Definitions

- `@types/nodemailer` (`^7.0.11`) — TypeScript types for nodemailer

---

## 7. AI / Machine Learning Model

The AI module lives in the `ai_model/` directory and is a complete machine learning pipeline for predicting used phone prices in the Nepal market.

### Python Libraries Used

| Library | Version | Purpose |
|---|---|---|
| **pandas** | `>=2.0.0` | Data manipulation and analysis — reading CSV datasets, creating DataFrames |
| **NumPy** | `>=1.24.0` | Numerical computing — array operations, mathematical functions |
| **scikit-learn** | `>=1.3.0` | Machine learning framework — model training, evaluation, preprocessing, hyperparameter tuning |
| **Matplotlib** | `>=3.7.0` | Data visualization — generating comparison charts and plots |
| **Seaborn** | `>=0.12.0` | Statistical visualization — enhanced color palettes and plot styling |
| **joblib** | `>=1.3.0` | Model serialization — saving and loading trained ML models as `.pkl` files |

### ML Models Trained & Compared

The `train_model.py` script trains and compares **5 different regression models** using GridSearchCV with 5-fold cross-validation:

| # | Model | Type | Description |
|---|---|---|---|
| 1 | **Linear Regression** | Baseline | Simple linear model as the baseline benchmark |
| 2 | **Ridge Regression** | L2 Regularized | Linear regression with L2 regularization to prevent overfitting |
| 3 | **Random Forest Regressor** | Ensemble | Decision tree ensemble for robust predictions |
| 4 | **Gradient Boosting Regressor** | Ensemble | Sequential boosting for high-accuracy predictions |
| 5 | **Support Vector Regressor (SVR)** | Kernel-based | Support vector machine adapted for regression |

### ML Pipeline (scikit-learn)

- **ColumnTransformer**: Applies different preprocessing to numerical vs. categorical features
- **Numerical Pipeline**: `SimpleImputer` (median) → `StandardScaler`
- **Categorical Pipeline**: `SimpleImputer` (most_frequent) → `OneHotEncoder`
- **GridSearchCV**: Hyperparameter tuning with 5-fold cross-validation
- **Evaluation Metrics**: R² Score, Mean Absolute Error (MAE), Root Mean Squared Error (RMSE)

### ML Files

| File | Purpose |
|---|---|
| `train_model.py` | Trains all 5 models, compares them, saves the best one |
| `predict.py` | Standalone prediction script |
| `predict_api.py` | API-compatible prediction script — called from Next.js backend via `child_process` |
| `generate_dataset.py` | Generates the synthetic Nepal used phone dataset |
| `explore_data.py` | Exploratory data analysis script |
| `nepal_used_phones.csv` | Training dataset (~1000 records of Nepal used phone data) |
| `best_model.pkl` | Serialized best-performing ML model |
| `price_scaler.pkl` | Serialized model metadata |
| `requirements.txt` | Python package dependencies |
| `plots/` | Generated comparison charts (model comparison, actual vs predicted, residuals) |
| `results/` | CSV results of model comparison |

### Features Used for Price Prediction

| Feature | Type | Description |
|---|---|---|
| `device_brand` | Categorical | Phone brand (e.g. Samsung, Apple, Xiaomi) |
| `model` | Categorical | Phone model name |
| `ram` | Numerical | RAM in GB |
| `internal_memory` | Numerical | Internal storage in GB |
| `days_used` | Numerical | Number of days the phone has been used |
| `battery_health` | Numerical | Battery health percentage (0-1.0) |
| `screen_condition` | Numerical | Screen condition (1 = Perfect, 0 = Not Perfect) |
| `new_price_npr` | Numerical | Original new price in NPR (Nepali Rupees) |

### How AI Integration Works

1. User fills the sell form on the frontend
2. The Next.js API route (`/api/phones`) receives the form data
3. The API spawns a **Python child process** (`predict_api.py`) with the phone data as a JSON argument (base64 encoded)
4. The Python script loads the trained model (`best_model.pkl`), processes the features, and predicts the price
5. The predicted price is returned as JSON and saved with the phone listing

---

## 8. Styling & Design

| Technology | Version | Purpose |
|---|---|---|
| **Tailwind CSS** | `^4` | Utility-first CSS framework for rapid UI development |
| **@tailwindcss/postcss** | `^4` | PostCSS plugin for Tailwind CSS v4 processing |
| **PostCSS** | — | CSS transformation tool — configured in `postcss.config.mjs` |

### Design System

- **Color Scheme**: Dark theme with black (`#000000`) background and white (`#ffffff`) foreground
- **Accent Color**: Cyan/electric blue (`#00E5FF`) — used for buttons, icons, highlights, and interactive elements
- **Font**: `Outfit` from Google Fonts — set as the default sans-serif
- **Design Patterns Used**:
  - Glassmorphism (translucent backgrounds with `backdrop-blur`)
  - Gradient text (`bg-clip-text text-transparent bg-gradient-to-r`)
  - Glow effects (`hover:shadow-[0_0_30px_rgba(0,229,255,0.4)]`)
  - Rounded corners (`rounded-2xl`, `rounded-3xl`, `rounded-full`)
  - Subtle borders (`border-white/10`)
  - Custom scrollbar hiding (`.no-scrollbar`)
  - Radial gradient vignette effect

---

## 9. Build & Dev Tooling

| Tool | Version | Purpose |
|---|---|---|
| **TypeScript** | `^5` | Static type checking at build time |
| **ESLint** | `^9` | JavaScript/TypeScript linter for code quality |
| **eslint-config-next** | `16.1.6` | Next.js-specific ESLint rules (Core Web Vitals + TypeScript) |
| **PostCSS** | — | CSS transformation pipeline |
| **npm** | — | Package manager |
| **Git** | — | Version control |

### TypeScript Configuration

- **Target**: ES2017
- **Module**: ESNext with Bundler resolution
- **Strict mode**: Enabled
- **JSX**: react-jsx
- **Path aliases**: `@/*` → `./*` (root-relative imports)
- **Incremental builds**: Enabled for faster rebuilds

### npm Scripts

| Script | Command | Purpose |
|---|---|---|
| `dev` | `next dev` | Start development server with hot reload |
| `build` | `next build` | Create production build |
| `start` | `next start` | Start production server |
| `lint` | `eslint` | Run ESLint on the codebase |

---

## 10. Project Structure

```
HamroPhone/
├── app/                          # Next.js App Router
│   ├── api/                      # Backend API routes
│   │   ├── auth/                 # Authentication endpoints
│   │   │   ├── change-password/route.ts
│   │   │   ├── login/route.ts
│   │   │   ├── logout/route.ts
│   │   │   ├── me/route.ts
│   │   │   ├── register/route.ts
│   │   │   └── verify-email/route.ts
│   │   ├── phones/               # Phone listing endpoints
│   │   │   ├── [id]/route.ts     # Single phone operations
│   │   │   ├── my-listings/route.ts
│   │   │   ├── route.ts          # CRUD for phone listings
│   │   │   └── seed/route.ts     # Database seeding
│   │   └── upload/route.ts       # Image upload endpoint
│   ├── buy/page.tsx              # Buy page
│   ├── login/page.tsx            # Login page
│   ├── signup/page.tsx           # Signup page
│   ├── sell/page.tsx             # Sell page (with AI price prediction)
│   ├── profile/page.tsx          # User profile page
│   ├── globals.css               # Global styles & Tailwind config
│   ├── layout.tsx                # Root layout (font, metadata)
│   ├── page.tsx                  # Home page
│   └── favicon.ico
│
├── components/                   # Reusable React components
│   ├── BrandsCarousel/index.tsx
│   ├── Footer/index.tsx
│   ├── Hero/index.tsx
│   ├── Mission/index.tsx
│   ├── Navbar.tsx
│   ├── PhoneCard/index.tsx
│   └── ReviewsCarousel/index.tsx
│
├── lib/                          # Utility/helper modules
│   ├── mongodb.ts                # MongoDB connection with caching
│   └── email.ts                  # Nodemailer email service
│
├── models/                       # Mongoose data models
│   ├── Phone.ts                  # Phone listing schema
│   └── User.ts                   # User account schema
│
├── ai_model/                     # Machine Learning module (Python)
│   ├── train_model.py            # Model training & comparison
│   ├── predict.py                # Standalone prediction
│   ├── predict_api.py            # API-callable prediction
│   ├── generate_dataset.py       # Dataset generation
│   ├── explore_data.py           # EDA script
│   ├── nepal_used_phones.csv     # Dataset
│   ├── best_model.pkl            # Trained model
│   ├── price_scaler.pkl          # Model metadata
│   ├── requirements.txt          # Python dependencies
│   ├── plots/                    # Generated charts
│   └── results/                  # CSV results
│
├── public/                       # Static assets
│   ├── brand_models.json         # Brand → Models mapping data
│   ├── images/                   # Static images
│   └── uploads/                  # User-uploaded phone images
│
├── .env.local                    # Environment variables
├── .gitignore
├── eslint.config.mjs             # ESLint configuration
├── next.config.ts                # Next.js configuration
├── postcss.config.mjs            # PostCSS configuration
├── tsconfig.json                 # TypeScript configuration
├── package.json                  # Node.js dependencies
└── package-lock.json
```

---

## 11. Environment Variables

| Variable | Purpose |
|---|---|
| `MONGODB_URI` | MongoDB Atlas connection string |
| `JWT_SECRET` | Secret key for signing JWT tokens |
| `EMAIL_USER` | Gmail address for sending emails |
| `EMAIL_PASS` | Gmail App Password for SMTP authentication |
| `NEXT_PUBLIC_BASE_URL` | Base URL of the application (used in email verification links) |

---

## 12. API Endpoints

### Authentication (`/api/auth/`)

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/auth/register` | Register a new user account |
| `GET` | `/api/auth/verify-email?token=...` | Verify email address |
| `POST` | `/api/auth/login` | Login with email & password |
| `POST` | `/api/auth/logout` | Logout (clear cookie) |
| `GET` | `/api/auth/me` | Get current authenticated user |
| `POST` | `/api/auth/change-password` | Change user password |

### Phone Listings (`/api/phones/`)

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/phones` | Get all available phone listings |
| `POST` | `/api/phones` | Create a new listing (with AI price prediction) |
| `GET` | `/api/phones/[id]` | Get a specific phone listing |
| `PUT/PATCH/DELETE` | `/api/phones/[id]` | Update or delete a listing |
| `GET` | `/api/phones/my-listings` | Get current user's listings |
| `POST` | `/api/phones/seed` | Seed the database with sample data |

### File Upload (`/api/upload/`)

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/api/upload` | Upload a device photo (saved to `public/uploads/`) |

---

## 13. Data Models

### User Model (`models/User.ts`)

| Field | Type | Description |
|---|---|---|
| `name` | String | User's full name |
| `email` | String | Unique email address (lowercase, trimmed) |
| `phone` | String | Phone number |
| `passwordHash` | String | Bcrypt-hashed password |
| `isVerified` | Boolean | Whether email is verified (default: false) |
| `verificationToken` | String | Email verification token |
| `verificationExpires` | Date | Token expiration time |
| `createdAt` | Date | Auto-generated timestamp |

### Phone Model (`models/Phone.ts`)

| Field | Type | Description |
|---|---|---|
| `brand` | String | Phone brand (e.g., Samsung, Apple) |
| `phoneModel` | String | Model name (e.g., Galaxy S23) |
| `ram` | String | RAM specification (e.g., "8GB") |
| `storage` | String | Storage specification (e.g., "128GB") |
| `daysUsed` | Number | Number of days the phone has been used |
| `batteryHealth` | Number | Battery health percentage (0-100) |
| `screenCondition` | Enum | "Perfect", "Minor Scratches", or "Cracked" |
| `description` | String | Optional description |
| `price` | Number | AI-predicted selling price in NPR |
| `photo` | String | URL path to uploaded device photo |
| `available` | Boolean | Whether listing is active (default: true) |
| `sellerId` | ObjectId | Reference to User who created the listing |
| `createdAt` | Date | Auto-generated timestamp |

---

## 14. Python Dependencies

Listed in `ai_model/requirements.txt`:

```
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
matplotlib>=3.7.0
seaborn>=0.12.0
joblib>=1.3.0
```

---

## 15. Node.js Dependencies

### Production Dependencies

| Package | Version | Purpose |
|---|---|---|
| `next` | `16.1.6` | React framework (App Router, SSR, API routes) |
| `react` | `19.2.3` | UI component library |
| `react-dom` | `19.2.3` | React DOM renderer |
| `mongoose` | `^9.2.4` | MongoDB ODM |
| `bcryptjs` | `^3.0.3` | Password hashing |
| `jsonwebtoken` | `^9.0.3` | JWT authentication |
| `nodemailer` | `^8.0.2` | Email sending |
| `framer-motion` | `^12.35.2` | Animations & transitions |
| `lucide-react` | `^0.577.0` | Icon library |

### Dev Dependencies

| Package | Version | Purpose |
|---|---|---|
| `typescript` | `^5` | TypeScript compiler |
| `tailwindcss` | `^4` | CSS framework |
| `@tailwindcss/postcss` | `^4` | PostCSS plugin for Tailwind |
| `eslint` | `^9` | Code linter |
| `eslint-config-next` | `16.1.6` | Next.js ESLint rules |
| `@types/node` | `^20` | Node.js type definitions |
| `@types/react` | `^19` | React type definitions |
| `@types/react-dom` | `^19` | React DOM type definitions |
| `@types/bcryptjs` | `^2.4.6` | bcryptjs type definitions |
| `@types/jsonwebtoken` | `^9.0.10` | jsonwebtoken type definitions |
| `@types/nodemailer` | `^7.0.11` | nodemailer type definitions |

---

## Summary

| Layer | Technologies |
|---|---|
| **Frontend** | React 19, Next.js 16 (App Router), TypeScript 5, Framer Motion, Lucide React |
| **Styling** | Tailwind CSS v4, PostCSS, Google Fonts (Outfit) |
| **Backend** | Next.js API Routes (Node.js) |
| **Database** | MongoDB Atlas + Mongoose 9 |
| **Auth** | JWT (jsonwebtoken) + bcryptjs |
| **Email** | Nodemailer + Gmail SMTP |
| **AI/ML** | Python, scikit-learn, pandas, NumPy, joblib, Matplotlib, Seaborn |
| **Dev Tools** | TypeScript, ESLint, Git, npm |
