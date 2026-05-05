# 🛒 FreshCart — Backend API

<div align="center">

![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-EF_Core-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-Authentication-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![SignalR](https://img.shields.io/badge/SignalR-Real--time-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Railway](https://img.shields.io/badge/Deployed-Railway-0B0D0E?style=for-the-badge&logo=railway&logoColor=white)

**A robust, multi-role e-commerce REST API built with .NET 8.**  
Powers the FreshCart marketplace with role-based access control for Customers, Vendors, and Admins.

[🌐 Live API](https://backend-freshcart-production.up.railway.app/swagger) • [💻 Frontend Repo](https://github.com/Ahmed-devS3B/FreshCart--FrontEnd) • [🐛 Report Bug](#) • [✨ Request Feature](#)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Tech Stack](#-tech-stack)
- [Features](#-features)
- [API Endpoints](#-api-endpoints)
- [Architecture](#-architecture)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Project Structure](#-project-structure)
- [Database Schema](#-database-schema)
- [Deployment](#-deployment)
- [Team](#-team)

---

## 🌟 Overview

MarketPlaceApi is a fully-featured e-commerce backend supporting three distinct user roles. Vendors can list and manage products, Admins oversee the entire marketplace with moderation tools, and Customers can browse, purchase, and track orders — all secured with JWT authentication and role-based access control.

---

## 🛠 Tech Stack

| Category | Technology |
|---|---|
| **Framework** | .NET 8.0 Web API |
| **Database** | PostgreSQL via Entity Framework Core |
| **Authentication** | JWT (JSON Web Tokens) with custom role claims |
| **Password Security** | BCrypt.Net-Next |
| **Real-time** | Microsoft SignalR |
| **Validation** | FluentValidation |
| **Documentation** | Swagger / OpenAPI (Swashbuckle) |
| **Containerization** | Docker |
| **Deployment** | Railway |

---

## ✨ Features

### 🛡️ Admin
- List, approve, disapprove, enable, and disable vendor accounts
- Review and moderate pending product submissions
- Accept or reject vendor products before they go live
- Toggle auto-approval settings per vendor or globally

### 🏪 Vendor
- Register and manage a storefront
- Add, update, and delete products
- Track product approval status (Pending / Approved / Rejected)
- View orders placed for their specific products

### 🧑‍💼 Customer
- Browse and search products by category and price
- View detailed product information
- Manage a persistent shopping cart (Add / Remove / Clear)
- Save products to a personal wishlist
- Place orders (Cash on Delivery) and view order history

---

## 🔌 API Endpoints

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/Auth/register/customer` | Register a new customer |
| POST | `/Auth/register/vendor` | Register a new vendor |
| POST | `/Auth/login/customer` | Customer login |
| POST | `/Auth/login/vendor` | Vendor login |
| POST | `/Auth/login/admin` | Admin login |

### Customer
| Method | Endpoint | Description |
|---|---|---|
| GET | `/Customer/products` | Get all approved products |
| GET | `/Customer/products/{id}` | Get product details |
| POST | `/Customer/cart` | Add item to cart |
| GET | `/Customer/cart` | View cart |
| DELETE | `/Customer/cart/{id}` | Remove cart item |
| POST | `/Customer/checkout` | Place an order |
| GET | `/Customer/orders` | View order history |
| POST | `/Customer/wishlist` | Save a product |
| GET | `/Customer/wishlist` | View wishlist |

### Vendor
| Method | Endpoint | Description |
|---|---|---|
| POST | `/Vendor/addproduct` | Add a new product |
| PUT | `/Vendor/updateproduct/{id}` | Update a product |
| DELETE | `/Vendor/deleteproduct/{id}` | Delete a product |
| GET | `/Vendor/products` | List vendor's products |
| GET | `/Vendor/orders` | View orders for vendor's products |

### Admin
| Method | Endpoint | Description |
|---|---|---|
| GET | `/Admin/vendors` | List all vendors |
| PUT | `/Admin/vendors/{phone}/approve` | Approve a vendor |
| PUT | `/Admin/vendors/{phone}/disapprove` | Disapprove a vendor |
| PUT | `/Admin/vendors/{phone}/enable` | Enable a vendor |
| PUT | `/Admin/vendors/{phone}/disable` | Disable a vendor |
| PUT | `/Admin/vendors/{phone}/auto-approve-products` | Toggle auto-approve |
| GET | `/Admin/products/pending` | List pending products |
| PUT | `/Admin/products/{id}/accept` | Approve a product |
| PUT | `/Admin/products/{id}/reject` | Reject a product |

---

## 🏗 Architecture

The project follows the **Controller → Service → Repository** pattern:

```
Request → Controller → Service (Business Logic) → Repository → Database
```

Key design decisions:
- **Generic Repository** (`IGenericRepository<T>`) for reusable CRUD operations
- **Scoped Services** via Dependency Injection for clean separation of concerns
- **Custom Middleware** (`AdminAuthenticationMiddleware`) for admin header-based authentication
- **SignalR Hub** (`NotificationHub`) for real-time product approval notifications

---

## 🚀 Getting Started

### Prerequisites
- .NET 8.0 SDK
- PostgreSQL database
- Docker (optional)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/Ahmed-devS3B/FreshCart--BackEnd.git
cd FreshCart--BackEnd
```

2. **Set up environment variables**  
Create `appsettings.Development.json` (see [Environment Variables](#-environment-variables))

3. **Apply database migrations**
```bash
dotnet ef database update
```

4. **Run the API**
```bash
dotnet run
```

5. **Access Swagger UI**
```
https://localhost:7219/swagger
```

### Run with Docker
```bash
docker build -t freshcart-api .
docker run -p 8080:8080 freshcart-api
```

---

## 🔐 Environment Variables

Create `appsettings.Development.json` in the project root:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=FreshCart;Username=postgres;Password=yourpassword"
  },
  "Jwt": {
    "Key": "your-secret-key-min-32-characters",
    "Issuer": "YourIssuer",
    "Audience": "YourAudience"
  },
  "AdminCredentials": {
    "Email": "admin@marketplace.com",
    "Password": "YourAdminPassword"
  }
}
```

> ⚠️ **Never commit `appsettings.Development.json`** — it is listed in `.gitignore`.

For **Railway deployment**, set these as environment variables using double underscore notation:
```
ConnectionStrings__DefaultConnection=Host=...
Jwt__Key=...
Jwt__Issuer=...
Jwt__Audience=...
AdminCredentials__Email=...
AdminCredentials__Password=...
```

---

## 📁 Project Structure

```
MarketPlaceApi/
├── Controllers/
│   ├── AdminController.cs       # Admin management endpoints
│   ├── AuthController.cs        # Registration and login
│   ├── CustomerController.cs    # Customer shopping endpoints
│   └── VendorController.cs      # Vendor product management
├── Data/
│   ├── MarketPlaceDbContext.cs  # EF Core DB context
│   ├── AppDbContextFactory.cs   # Design-time factory
│   └── Migrations/              # EF Core migrations
├── Hubs/
│   └── NotificationHub.cs       # SignalR real-time hub
├── Middleware/
│   └── AdminAuthenticationMiddleware.cs  # Custom admin auth
├── Models/
│   ├── Product.cs               # Product entity
│   ├── Vendor.cs                # Vendor entity
│   ├── Customer.cs              # Customer entity
│   ├── Order.cs                 # Order entity
│   ├── OrderItem.cs             # Order line items
│   ├── CartItem.cs              # Shopping cart item
│   ├── SavedProduct.cs          # Wishlist item
│   └── DTOs/                    # Data transfer objects
├── Repositories/
│   ├── IGenericRepository.cs    # Generic repository interface
│   └── GenericRepository.cs     # Generic repository implementation
├── Services/
│   ├── AdminService.cs          # Admin business logic
│   ├── AuthService.cs           # Auth business logic
│   ├── CustomerService.cs       # Customer business logic
│   └── VendorService.cs         # Vendor business logic
├── Program.cs                   # App entry point and DI config
├── Dockerfile                   # Docker configuration
└── appsettings.json             # Base configuration (no secrets)
```

---

## 🗄 Database Schema

| Table | Primary Key | Key Relations |
|---|---|---|
| `Customers` | `PhoneNumber` | Has many `CartItems`, `Orders`, `SavedProducts` |
| `Vendors` | `PhoneNumber` | Has many `Products` |
| `Products` | `Id` | Belongs to `Vendor`, has many `CartItems`, `OrderItems` |
| `CartItems` | `Id` | Belongs to `Customer` and `Product` |
| `Orders` | `Id` | Belongs to `Customer`, has many `OrderItems` |
| `OrderItems` | `Id` | Belongs to `Order` and `Product` |
| `SavedProducts` | `CustomerPhoneNumber + ProductId` | Belongs to `Customer` and `Product` |

---

## 🌐 Deployment

The API is deployed on **Railway** with a **PostgreSQL** database.

Live API:
```
https://backend-freshcart-production.up.railway.app
```

Swagger UI:
```
https://backend-freshcart-production.up.railway.app/swagger
```

---

## 👥 Team

<div align="center">

This project was built with ❤️ by:

| Name | GitHub |
|---|---|
| **Ahmed Ibrahim** | [@Ahmed-devS3B](https://github.com/Ahmed-devS3B) |
| **Rawan Mahmoud** | [@Rwan-Mahmoud](https://github.com/Rwan-Mahmoud) |
| **Maha Elsayed** | [@mahaelsayed2026-ship-it](https://github.com/mahaelsayed2026-ship-it) |

</div>

---

## 📄 License

This project is for educational and portfolio purposes.
