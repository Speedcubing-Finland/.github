# Speedcubing Finland Website

Official website and member management system for Speedcubing Finland organization.

## 🚀 Project Overview

This full-stack application manages member registrations, displays upcoming WCA competitions in Finland, and provides an admin panel for managing member applications.

### Features

- 📋 **Public Member Registration** - Anyone can submit membership applications
- 🔐 **Secure Admin Panel** - JWT-based authentication for admin operations
- 🏆 **Competition Listings** - Automatic WCA competition data integration
- 👥 **Member Management** - Review and approve/reject membership applications
- 📊 **CSV Comparison Tool** - Compare WCA competition participant lists with member database
- 📱 **Responsive Design** - Mobile-friendly interface built with React and Tailwind CSS

## 🏗️ Tech Stack

### Frontend
- **React 18.3** - UI framework
- **Vite 6.0** - Build tool and dev server
- **React Router 7.1** - Client-side routing
- **Tailwind CSS 3.4** - Utility-first CSS framework
- **Papa Parse 5.5** - CSV parsing for member comparison

### Backend
- **Node.js** - Runtime environment
- **Express 4.21** - Web framework
- **MySQL 2** - Database connection
- **JWT (jsonwebtoken 9.0)** - Authentication tokens
- **BCrypt.js 3.0** - Password hashing
- **CORS 2.8** - Cross-origin resource sharing

### Deployment
- **Frontend**: Hostinger (or similar static hosting)
- **Backend**: Render.com (auto-deploy from GitHub)
- **Database**: Hostinger MySQL

## 📂 Project Structure

```
Speedcubing_Finland/
├── backend/           # Express.js API server
│   ├── src/
│   │   ├── routes/
│   │   │   ├── admin.js      # Protected admin routes (JWT required)
│   │   │   └── public.js     # Public routes (no auth)
│   │   ├── middleware/
│   │   │   └── auth.js       # JWT verification middleware
│   │   ├── db.js             # MySQL connection pool
│   │   └── index.js          # App entry point
│   ├── .env                  # Environment variables (NOT in git)
│   ├── .env.example          # Template for environment setup
│   └── package.json
│
└── frontend/          # React application
    ├── src/
    │   ├── components/       # Reusable UI components
    │   ├── pages/            # Route pages
    │   ├── utilities/        # Helper functions & API client
    │   ├── data/             # Static data (WCA events)
    │   └── assets/           # Images and static files
    ├── .env                  # Environment variables (NOT in git)
    ├── .env.example          # Template for environment setup
    └── package.json
```

## 🔧 Setup & Installation

### Prerequisites
- Node.js 18+ and npm
- MySQL database
- Git

### Backend Setup

1. **Navigate to backend directory**
   ```bash
   cd backend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   
   Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

   Edit `.env` with your actual values:
   ```env
   # Database Configuration
   DB_HOST=your_mysql_host
   DB_USER=your_mysql_user
   DB_PASSWORD=your_mysql_password
   DB_NAME=your_database_name
   DB_PORT=3306

   # Server Configuration
   PORT=3000

   # Admin Authentication
   ADMIN_USERNAME=admin
   
   # Generate password hash with: node -e "require('bcryptjs').hash('your_password', 10, (e,h) => console.log(h))"
   ADMIN_PASSWORD_HASH=your_bcrypt_hashed_password

   # Generate JWT secret with: node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
   JWT_SECRET=your_jwt_secret_key_here
   ```

4. **Generate secure credentials**
   
   Generate password hash:
   ```bash
   node -e "require('bcryptjs').hash('YourStrongPassword', 10, (e,h) => console.log(h))"
   ```

   Generate JWT secret:
   ```bash
   node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
   ```

5. **Start development server**
   ```bash
   npm run dev    # With nodemon (auto-restart)
   # or
   npm start      # Without nodemon
   ```

   Backend runs on `http://localhost:3000`

### Frontend Setup

1. **Navigate to frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   
   Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

   Edit `.env`:
   ```env
   # API Configuration
   VITE_API_BASE_URL=http://localhost:3000
   ```

4. **Start development server**
   ```bash
   npm run dev
   ```

   Frontend runs on `http://localhost:5173`

## 🔐 Authentication & Security

### JWT Authentication Flow

1. **Admin Login**
   - Admin enters username and password
   - Backend validates credentials against BCrypt hash
   - Backend generates JWT token (expires in 24h)
   - Frontend stores token in localStorage

2. **Protected Requests**
   - Frontend automatically adds `Authorization: Bearer <token>` header
   - Backend middleware verifies token validity
   - Expired/invalid tokens trigger automatic logout

3. **Public Endpoints** (no auth required)
   - `POST /api/submit-member` - Submit membership application
   - `POST /api/admin/login` - Admin login

4. **Protected Endpoints** (JWT required)
   - `GET /api/admin/submissions` - List pending applications
   - `POST /api/admin/approve` - Approve application
   - `POST /api/admin/reject` - Reject application
   - `GET /api/admin/members` - List all members

### Security Best Practices

✅ Passwords hashed with BCrypt (salt rounds: 10)
✅ JWT tokens with expiration
✅ Environment variables for secrets
✅ CORS configured for allowed origins only
✅ Input validation on all endpoints
✅ SQL injection prevention (parameterized queries)

## 🚢 Deployment

### Backend Deployment (Render.com)

1. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Add JWT authentication"
   git push
   ```

2. **Configure Render Environment Variables**
   
   In Render dashboard, add:
   - `DB_HOST`
   - `DB_USER`
   - `DB_PASSWORD`
   - `DB_NAME`
   - `DB_PORT`
   - `ADMIN_USERNAME`
   - `ADMIN_PASSWORD_HASH`
   - `JWT_SECRET`

3. **Auto-deploy**
   
   Render automatically deploys on git push.

### Frontend Deployment

Update `.env` to point to production backend:
```env
VITE_API_BASE_URL=https://your-backend-url.onrender.com
```

Build and deploy:
```bash
npm run build
# Upload dist/ folder to hosting provider
```

## 📖 API Documentation

### Public Endpoints

#### Submit Membership Application
```http
POST /api/submit-member
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "city": "Helsinki",
  "birthDate": "1990-01-01",
  "wcaId": "2023DOEJ01"  // optional
}
```

#### Admin Login
```http
POST /api/admin/login
Content-Type: application/json

{
  "username": "admin",
  "password": "your_password"
}

Response:
{
  "token": "eyJhbGc...",
  "username": "admin",
  "expiresIn": "24h"
}
```

### Protected Endpoints (Require JWT)

All requests must include:
```http
Authorization: Bearer <jwt_token>
```

#### Get Pending Submissions
```http
GET /api/admin/submissions
```

#### Approve Submission
```http
POST /api/admin/approve
Content-Type: application/json

{
  "index": 0
}
```

#### Reject Submission
```http
POST /api/admin/reject
Content-Type: application/json

{
  "index": 0
}
```

#### Get All Members
```http
GET /api/admin/members
```

## 🧪 Development

### Available Scripts

**Backend:**
- `npm start` - Start production server
- `npm run dev` - Start development server with nodemon
- `npm test` - Run tests (configured with Jest)

**Frontend:**
- `npm run dev` - Start Vite dev server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

### Database Schema

```sql
CREATE TABLE members (
  id INT AUTO_INCREMENT PRIMARY KEY,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  city VARCHAR(100) NOT NULL,
  birth_date DATE NOT NULL,
  wca_id VARCHAR(10),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request

## 📝 License

This project is private and belongs to Speedcubing Finland organization.

## 🐛 Known Issues & TODOs

- [ ] Add refresh token functionality
- [ ] Implement password reset flow
- [ ] Add email notifications for approved members
- [ ] Improve error handling and user feedback
- [ ] Add comprehensive test coverage
- [ ] Implement rate limiting
- [ ] Add admin activity logging

## 📧 Contact

Speedcubing Finland
- Website: https://speedcubingfinland.fi
- GitHub: https://github.com/Speedcubing-Finland

---

**Note**: Never commit `.env` files or expose secrets. Always use environment variables for sensitive data.
