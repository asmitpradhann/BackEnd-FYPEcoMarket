# 🌿 Ecomarket — Backend API

Django REST Framework backend for **Ecomarket**, Nepal's first verified eco-friendly marketplace. This API powers product listings, user authentication, order management, real-time chat, payment integration, and a green points reward system.

**Live API:** https://ecomarket-backend-qlac.onrender.com  
**Frontend:** https://front-end-fyp-eco-market.vercel.app

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | Django 5.1.7 + Django REST Framework |
| Auth | JWT (SimpleJWT) + Google OAuth |
| Database | PostgreSQL (Render) |
| Media Storage | Cloudinary |
| Deployment | Render |
| Email | Gmail SMTP |
| Payments | eSewa, Khalti (sandbox) |

---

## Features

- **User System** — Customer and Seller roles, email verification, Google OAuth, JWT authentication
- **Seller Onboarding** — Document upload, admin verification workflow
- **Products** — CRUD, eco categories, product categories, ratings, stock management
- **Orders** — Cart, checkout, order tracking, status history
- **Payments** — eSewa and Khalti payment gateway integration, Cash on Delivery
- **Green Points** — Earn points on purchases, redeem for discounts, leaderboard
- **Chat** — Order-based messaging between buyers and sellers
- **Admin Panel** — Custom admin endpoints for user/seller/product management
- **Media** — Cloudinary for persistent image storage

---

## Project Structure

```
Ecomarket/
├── Ecomarket/          # Project settings and URLs
├── users/              # Auth, profiles, seller onboarding, green points
├── products/           # Product listings, categories, ratings
├── orders/             # Cart, orders, payments, tracking
├── chat/               # Buyer-seller messaging
├── adminpanel/         # Custom admin views
├── media/              # Local media (dev only)
├── build.sh            # Render build script
├── manage.py
└── requirements.txt
```

---

## API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/register/` | Register customer |
| POST | `/api/seller/register/` | Register seller |
| POST | `/api/login/` | Login |
| POST | `/api/verify-email/` | Email verification |
| POST | `/api/token/refresh/` | Refresh JWT token |
| GET/PUT | `/api/profile/` | Get/update profile |
| POST | `/api/auth/google-login/` | Google OAuth login |
| POST | `/api/auth/forgot-password/otp/` | Send OTP for password reset |
| POST | `/api/auth/reset-password/otp/` | Reset password with OTP |

### Products
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/products/` | List all products |
| POST | `/api/products/` | Create product (seller) |
| GET/PUT/DELETE | `/api/products/<id>/` | Product detail |
| GET | `/api/products/seller/my-products/` | Seller's own products |
| GET | `/api/products/categories/` | Eco categories |
| GET | `/api/products/product-categories/` | Product categories |
| GET/POST | `/api/products/<id>/ratings/` | Product ratings |

### Orders & Cart
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET/POST | `/api/cart/` | View/add to cart |
| PUT/DELETE | `/api/cart/<id>/` | Update/remove cart item |
| POST | `/api/orders/create/` | Create order |
| GET | `/api/orders/history/` | Order history |
| GET | `/api/orders/<id>/track/` | Track order |
| PATCH | `/api/orders/<id>/status/` | Update order status |
| PATCH | `/api/orders/<id>/cancel/` | Cancel order |
| GET | `/api/orders/seller/orders/` | Seller's orders |

### Payments
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/orders/esewa/initiate/` | Initiate eSewa payment |
| GET | `/api/orders/esewa/verify/` | Verify eSewa payment |
| POST | `/api/orders/khalti/initiate/` | Initiate Khalti payment |
| GET | `/api/orders/khalti/verify/` | Verify Khalti payment |
| POST | `/api/orders/cod/create/` | Create COD order |

### Green Points
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/auth/green-points/` | Get balance |
| GET | `/api/auth/green-points/history/` | Transaction history |
| GET | `/api/auth/green-points/leaderboard/` | Top users |

### Chat
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/chat/<order_id>/` | Get messages for order |
| POST | `/api/chat/<order_id>/send/` | Send message |
| GET | `/api/chat/<order_id>/sellers/` | Get sellers in order |

---

## Local Development Setup

### Prerequisites
- Python 3.10+
- pip
- Virtual environment

### Steps

**1. Clone the repo**
```bash
git clone https://github.com/ASMIT123445/BackEnd-FYPEcoMarket.git
cd BackEnd-FYPEcoMarket/Ecomarket
```

**2. Create and activate virtual environment**
```bash
python -m venv env
source env/bin/activate  # Mac/Linux
env\Scripts\activate     # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Create `.env` file**
```env
SECRET_KEY=your-secret-key
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# Leave commented for SQLite locally
# DATABASE_URL=postgresql://...

CORS_ALLOWED_ORIGINS=http://localhost:5173,http://localhost:5174

CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret
```

**5. Run migrations**
```bash
python manage.py migrate
```

**6. Create superuser (optional)**
```bash
python manage.py createsuperuser
```

**7. Start server**
```bash
python manage.py runserver
```

API available at `http://127.0.0.1:8000`

---

## Deployment (Render)

### Environment Variables (set on Render)
```
SECRET_KEY=
DEBUG=False
ALLOWED_HOSTS=your-app.onrender.com
DATABASE_URL=postgresql://...
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

### Build Command
```
chmod +x ./build.sh && ./build.sh
```

### Start Command
```
gunicorn Ecomarket.wsgi:application
```

The `build.sh` script handles `pip install`, `collectstatic`, and `migrate` automatically.

---

## Payment Gateway (Sandbox)

**eSewa**
- Merchant ID: `EPAYTEST`
- Test credentials: use eSewa sandbox

**Khalti**
- Test Phone: `9800000001`
- MPIN: `1111`
- OTP: `987654`

---

## Green Points System

| Action | Points |
|--------|--------|
| Earn rate | 1 point per Rs 10 spent |
| Redeem rate | 5 points = Rs 1 discount |
| Minimum to redeem | 50 points |
| Maximum discount | 50% of order total |

---

## Environment Notes

- **Local:** Uses SQLite (no `DATABASE_URL` set)
- **Production:** Uses PostgreSQL via `DATABASE_URL`
- **Media:** Cloudinary for all uploaded images in production
- **Static:** WhiteNoise serves static files on Render
