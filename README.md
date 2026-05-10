# Wanderlust – Discover, Share & Book Unique Travel Stays

Wanderlust is a Full-stack travel listing platform built with the MEN stack, enabling users to explore destinations, manage listings, upload images, interact via maps, and share reviews and ratings. Designed using MVC architecture, RESTful routing, and SSR for a secure, scalable, production-ready system.

## What You Can Do on Wanderlust

Wanderlust lets users discover, share, and interact with travel stays in a simple and intuitive way:

- Explore curated travel listings and unique stays from different users  
- View detailed property pages with images, descriptions, pricing, and location information  
- Create and publish your own travel or property listings  
- Upload and manage listing images securely using cloud storage  
- Read and write reviews to share travel experiences with the community  
- Register, log in, and manage your account securely with session-based authentication  
- View and manage your own listings in a personal dashboard-like experience  
- Seamlessly navigate through fast, server-rendered pages for a smooth experience  

## Try Wanderlust live:

https://wanderlust-t5nt.onrender.com/listings

---

# Overview

Wanderlust provides a structured platform for:

- Persistent CRUD operations on travel listings  
- Distributed media ingestion and storage via cloud infrastructure  
- Relational review aggregation linked to primary listing entities  
- Stateful authentication using session-backed identity management  
- Location-aware listing visualization via geospatial integration  

The backend enforces strict separation between routing, domain logic, and persistence layers to ensure extensibility and fault isolation.

---

# System Architecture
Wanderlust is designed as a modular, layered monolithic web application following MVC principles with clear separation of concerns across presentation, application, domain, and persistence layers. The system is optimized for scalability, maintainability, and future migration toward distributed microservices.

## Architectural Paradigm

- Layered MVC (Model-View-Controller)  
- RESTful API design principles  
- Server-Side Rendering (SSR) with templating engine  
- Middleware-driven request lifecycle

## High-Level Architecture
<img width="1388" height="1465" alt="Blank diagram" src="https://github.com/user-attachments/assets/c413e76c-0180-4bd0-b14f-468e92e8e18e" />


---

# Core Capabilities

- Stateless RESTful routing with stateful session overlays  
- Full CRUD lifecycle for listing resources  
- Asynchronous media upload and externalized storage  
- Relational data modeling between listings and reviews  
- Flash-based messaging for transactional feedback  
- Schema-level validation and request sanitization  
- Persistent session management across distributed requests  
- Template reuse via layout abstraction (ejs-mate)  

---

# Domain Workflows

## 1. Listing Lifecycle Management
- Input ingestion via HTTP POST  
- Schema validation using Joi middleware  
- Image stream processing via Multer  
- External asset storage via Cloudinary  
- Document persistence using Mongoose ORM  

## 2. Review Aggregation System
- One-to-many relationship modeling (Listing → Reviews)  
- Referential integrity maintained via ObjectId linking  
- Validation layer ensures schema conformity before persistence  

## 3. Authentication & Identity Management
- Passport.js implements pluggable authentication strategy  
- Password hashing handled via passport-local-mongoose  
- Session persistence using MongoDB-backed store  
- Cookie-based session identification for request continuity  

## 4. Centralized Error Propagation
- Higher-order async wrappers (`wrapAsync`) for promise handling  
- Custom error abstraction (`ExpressError`)  
- Unified error-handling middleware for consistent response formatting  

---

# Technology Stack

| Layer | Technology |
|------|-----------|
| Frontend / Presentation Layer | EJS, HTML5, CSS3, JavaScript (ES6+), Bootstrap |
| Backend / Application Layer | Express.js |
| Runtime Environment | Node.js |
| Database / Data Layer | MongoDB |
| ORM / ODM | Mongoose |
| Authentication | Passport.js (Local Strategy), passport-local-mongoose |
| Session Management / Session Store | express-session, connect-mongo |
| File Upload / File Ingestion | Multer |
| Cloud / Media Storage | Cloudinary (with CDN delivery & transformations) |
| Maps & Geolocation / Geospatial Services | Mapbox SDK |
| Validation Layer | Joi |
| Templating Engine / Utilities | EJS, ejs-mate |
| Middleware Stack / Utilities | method-override, cookie-parser, connect-flash |
| Configuration Management | dotenv |
| Architecture | MVC (Model-View-Controller), RESTful Routing, Server-Side Rendering (SSR) |

---

# Engineering Concepts and Implementation Details

## Separation of Concerns (MVC)
- Models encapsulate schema and database interaction  
- Views handle presentation logic via SSR  
- Controllers orchestrate request handling and business rules  

## Server-Side Rendering (SSR)
- HTML generation occurs on the server  
- Reduces client-side computation overhead  
- Improves SEO and first contentful paint (FCP)  

## RESTful Resource Design
- Resource-oriented endpoints  
- Idempotent and non-idempotent HTTP method usage  
- `method-override` enables PUT/DELETE semantics in form submissions  

## Authentication and Session Management
- Stateful authentication using encrypted session cookies  
- Session persistence via MongoDB-backed store  
- Serialization/deserialization of user identity per request  

## Externalized Media Pipeline
- Decouples application server from binary storage  
- Uses Cloudinary for scalable, CDN-backed delivery  
- Stores only asset references in database  

## Input Validation Layer
- Joi enforces schema constraints at request boundary  
- Prevents malformed payloads from reaching business logic  

## Asynchronous Error Handling Strategy
- Eliminates redundant try-catch blocks  
- Ensures consistent error bubbling to centralized handler  

## Custom Error Abstraction
- Structured error objects with status codes  
- Middleware-based response normalization  

## Session-Based Access Control
- Middleware enforces route-level protection  
- Ensures only authenticated users access protected resources  

---

# Project Structure

```
Wanderlust/
│
├── controller/
│   ├── listings.js
│   ├── reviews.js
│   └── users.js
│
├── init/
│   ├── data.js
│   └── index.js
│
├── models/
│   ├── listing.js
│   ├── review.js
│   └── user.js
│
├── public/
│   ├── css/
│   │   ├── rating.css
│   │   └── style.css
│   │
│   └── js/
│       ├── map.js
│       └── script.js
│
├── routes/
│   ├── listing.js
│   ├── review.js
│   └── user.js
│
├── utils/
│   ├── ExpressError.js
│   └── wrapAsync.js
│
├── views/
│   ├── includes/
│   │   ├── flash.ejs
│   │   ├── footer.ejs
│   │   └── navbar.ejs
│   │
│   ├── layouts/
│   │   └── boilerplate.ejs
│   │
│   ├── listings/
│   │   ├── edit.ejs
│   │   ├── index.ejs
│   │   ├── new.ejs
│   │   └── show.ejs
│   │
│   ├── users/
│   │   ├── login.ejs
│   │   └── signup.ejs
│   │
│   └── error.ejs
│
├── .gitignore
├── app.js
├── cloudConfig.js
├── middleware.js
├── package-lock.json
├── package.json
└── schema.js
```

---

# Security and Reliability

- Cryptographic password hashing via passport-local-mongoose  
- Session persistence in database-backed store  
- Input validation and sanitization using Joi  
- Route-level access control via authentication middleware  
- Centralized error handling to prevent leakage of internal state  

---

# Key Engineering Highlights

- Modular and extensible MVC architecture  
- Clear decoupling between application layers  
- Externalized and scalable media handling pipeline  
- Robust session lifecycle management  
- Reusable and composable templating system  
- Strong validation and error-handling guarantees  

---

# Environment and Setup

## Prerequisites
- Node.js (v24.13.0 or later)  
- MongoDB (local instance or MongoDB Atlas)  
- Cloudinary account  
- Mapbox API token  

## Installation

```bash
git clone https://github.com/aarul-kumar/wanderlust
cd Wanderlust
npm install
````

## Environment Variables (.env)

```
DB_URL=your_mongodb_connection_string
SECRET=session_secret_key
CLOUD_NAME=cloudinary_cloud_name
CLOUD_API_KEY=cloudinary_api_key
CLOUD_API_SECRET=cloudinary_api_secret
MAP_TOKEN=mapbox_access_token
```

## Application Execution

```bash
node app.js
```
