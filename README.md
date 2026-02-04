# ForkScores

A web application for rating and reviewing individual menu items at restaurants. Unlike traditional restaurant review platforms, ForkScores focuses on the dishes themselves, allowing users to discover the best dishes in their area.

## Features

### For Users
- Browse and search menu items across restaurants
- Leave detailed reviews with ratings for taste, quality, value, and presentation
- Upload photos with reviews
- Save favorite dishes
- Real-time updates when new reviews are posted

### For Restaurant Owners
- Add and manage restaurants
- Create and organize menu categories and items
- View analytics and ratings
- Respond to customer reviews

### For Administrators
- Dashboard with platform-wide statistics
- User management (roles, verification, moderation)
- Restaurant management (verification, activation)
- Review moderation (flagged content, visibility)
- Menu item management

## Tech Stack

- **Frontend**: React 18 + TypeScript + Vite + Tailwind CSS
- **Backend**: Node.js + Express + TypeScript
- **Database**: PostgreSQL with Prisma ORM
- **Real-time**: Socket.io
- **Authentication**: JWT with refresh tokens

## Project Structure

```
forkscores/
├── backend/           # Node.js/Express API
│   ├── src/
│   │   ├── routes/    # API endpoints
│   │   ├── middleware/# Auth, error handling
│   │   ├── config/    # Database config
│   │   └── socket/    # WebSocket handlers
│   └── prisma/        # Database schema
├── web/               # React frontend
│   └── src/
│       ├── components/# Reusable UI components
│       ├── pages/     # Page components
│       ├── services/  # API client
│       ├── context/   # Auth & Socket contexts
│       └── types/     # TypeScript types
└── README.md
```

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+ (or Docker)
- npm or yarn

### Setup

1. **Clone the repository**
   ```bash
   cd /path/to/forkscores
   ```

2. **Install dependencies**
   ```bash
   # Install backend dependencies
   cd backend
   npm install

   # Install frontend dependencies
   cd ../web
   npm install
   ```

3. **Set up the database**
   ```bash
   # Option A: Using Docker
   docker run -d --name forkscores-postgres \
     -e POSTGRES_USER=postgres \
     -e POSTGRES_PASSWORD=password \
     -e POSTGRES_DB=forkscores \
     -p 5432:5432 postgres:16-alpine

   # Option B: Update DATABASE_URL in .env with your PostgreSQL connection string
   cd backend
   cp .env.example .env

   # Generate Prisma client and run migrations
   npm run db:generate
   npm run db:migrate

   # (Optional) Seed with sample data
   npx tsx prisma/seed.ts
   ```

4. **Start the development servers**
   ```bash
   # Terminal 1 - Backend
   cd backend
   npm run dev

   # Terminal 2 - Frontend
   cd web
   npm run dev
   ```

5. **Open the app**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:3001

## Default Admin Account

After running the seed script:
- **Email**: daniel410.choi@gmail.com
- **Password**: (set in seed script)

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login
- `POST /api/auth/refresh` - Refresh tokens
- `GET /api/auth/me` - Get current user

### Restaurants
- `GET /api/restaurants` - List restaurants
- `GET /api/restaurants/:id` - Get restaurant with menu
- `POST /api/restaurants` - Create restaurant (auth required)
- `PATCH /api/restaurants/:id` - Update restaurant (owner only)
- `GET /api/restaurants/:id/analytics` - Get analytics (owner only)

### Menu Items
- `GET /api/menus/items` - Search menu items
- `GET /api/menus/items/:id` - Get menu item details
- `POST /api/menus/items/:id/favorite` - Toggle favorite (auth required)
- `POST /api/menus/categories` - Create category (owner only)
- `POST /api/menus/items` - Create menu item (owner only)

### Reviews
- `GET /api/reviews/item/:menuItemId` - Get reviews for item
- `POST /api/reviews` - Create review (auth required)
- `POST /api/reviews/:id/helpful` - Vote helpful (auth required)
- `POST /api/reviews/:id/respond` - Owner response (owner only)

### Admin (requires ADMIN role)
- `GET /api/admin/stats` - Platform statistics
- `GET /api/admin/users` - List users
- `PATCH /api/admin/users/:id` - Update user
- `GET /api/admin/restaurants` - List all restaurants
- `GET /api/admin/reviews` - List all reviews
- `GET /api/admin/menu-items` - List all menu items

## Environment Variables

### Backend (.env)
```
NODE_ENV=development
PORT=3001
DATABASE_URL=postgresql://user:password@localhost:5432/forkscores
JWT_SECRET=your-secret-key
JWT_REFRESH_SECRET=your-refresh-secret
FRONTEND_URL=http://localhost:5173
```

## License

MIT
