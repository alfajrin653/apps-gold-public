# GoldTrade

GoldTrade is a full-stack gold transaction management system designed to manage gold purchase and sales submissions, approval workflows, stock tracking, supporting documents, and audit records between Sales and Management users.

The system was built to make transaction processing more structured, traceable, and easier to monitor in real time.

---

## Overview

GoldTrade provides two main user roles:

### Sales
Sales users can:

- Submit gold purchase transactions
- Submit gold sales transactions
- Enter transaction details such as:
  - Sales name
  - Location
  - Seller or buyer name
  - Gold weight
  - Price per gram
  - Karat
  - Transaction description
- Upload supporting documents and transaction photos
- View their own transaction history
- Receive real-time approval or rejection notifications

### Management
Management users can:

- Review incoming transactions
- View transaction details and supporting photos
- Approve or reject submissions
- Add rejection descriptions
- Upload transfer proof when approving gold purchases
- Automatically update gold stock after approval
- Monitor transaction history
- View dashboard statistics
- Review audit logs
- Receive real-time transaction notifications

---

## Key Features

- Role-based authentication
- Gold purchase and sales transaction workflow
- Transaction approval and rejection system
- Gold stock management
- Automatic stock updates
- Transaction descriptions and notes
- Supporting image uploads
- Management transfer proof upload
- Rejection reason tracking
- Transaction history and filtering
- Audit logging
- Real-time notifications using Socket.io
- Responsive user interface
- Server-side validation
- Cloud-based image storage
- Protected API routes

---

## Tech Stack

### Frontend

- React.js
- Vite
- Tailwind CSS
- React Router
- Axios
- Framer Motion
- GSAP
- Recharts
- Socket.io Client
- Lucide React

### Backend

- Node.js
- Express.js
- Prisma ORM
- Socket.io
- JWT Authentication
- bcryptjs
- Multer
- Cloudinary

### Database

- MySQL

The project was originally developed using PostgreSQL and later migrated to MySQL for production deployment.

### Infrastructure

- Hostinger
- Cloudinary
- GitHub

---

## System Architecture

```text
User
  |
  v
React Frontend
  |
  | REST API + Socket.io
  v
Express Backend
  |
  | Prisma ORM
  v
MySQL Database
  |
  +-- User
  +-- Transaction
  +-- Stock
  +-- AuditLog

Image Upload
  |
  v
Cloudinary
```

---

## Transaction Workflow

### Gold Purchase

```text
Sales
  |
  v
Submit Purchase
  |
  +-- Seller Information
  +-- Gold Weight
  +-- Price
  +-- Karat
  +-- Description
  +-- Account Number
  +-- Gold Photo
  |
  v
Waiting for Management Approval
  |
  +-- Approve
  |     |
  |     +-- Upload Transfer Proof
  |     +-- Gold Stock Increases
  |
  +-- Reject
        |
        +-- Rejection Description
```

### Gold Sale

```text
Sales
  |
  v
Submit Sale
  |
  +-- Buyer Information
  +-- Gold Weight
  +-- Price
  +-- Karat
  +-- Description
  +-- Sale Proof
  +-- Transfer Proof
  |
  v
Waiting for Management Approval
  |
  +-- Approve
  |     |
  |     +-- Gold Stock Decreases
  |
  +-- Reject
        |
        +-- Rejection Description
```

---

## User Roles

### SALES

Sales users are responsible for submitting transaction data and supporting documents.

Available routes:

```text
/sales
/sales/beli
/sales/jual
```

### MANAGEMENT

Management users are responsible for reviewing transactions and monitoring operational data.

Available routes:

```text
/management
/management/tracking
/management/audit
```

---

## API Overview

### Authentication

```text
POST /api/auth/login
GET  /api/auth/me
```

### Transactions

```text
POST  /api/transactions
GET   /api/transactions/mine
GET   /api/transactions
GET   /api/transactions/:id
PATCH /api/transactions/:id/approve
PATCH /api/transactions/:id/reject
```

### Dashboard

```text
GET /api/dashboard/summary
GET /api/dashboard/audit
```

### Health Check

```text
GET /api/health
```

---

## Real-Time Notifications

GoldTrade uses Socket.io to provide real-time communication between Sales and Management.

Main events:

```text
transaction:new
```

Sent to Management when a Sales user submits a new transaction.

```text
transaction:reviewed
```

Sent to the Sales owner when Management approves or rejects the transaction.

---

## Database Models

The core database structure consists of:

### User

Stores authentication and role information.

### Transaction

Stores purchase and sales transaction data including:

- Transaction type
- Status
- Sales owner
- Counterparty
- Gold weight
- Price
- Karat
- Description
- Supporting images
- Review information
- Transfer proof
- Rejection notes

### Stock

Stores the current total gold stock.

### AuditLog

Records important system activities such as:

- Login
- Transaction submission
- Approval
- Rejection

---

## Project Structure

```text
gold-app/
│
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── seed.js
│   │
│   └── src/
│       ├── config/
│       ├── middleware/
│       ├── routes/
│       ├── utils/
│       ├── server.js
│       └── socket.js
│
└── frontend/
    └── src/
        ├── components/
        ├── context/
        ├── lib/
        ├── pages/
        ├── App.jsx
        └── main.jsx
```

---

## Environment Variables

Create a `.env` file inside the backend directory.

```env
DATABASE_URL=
JWT_SECRET=
CLIENT_URL=
PORT=

CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

Create a `.env` file inside the frontend directory.

```env
VITE_API_URL=
```

Never commit real credentials or secret values to the repository.

---

## Installation

Clone the repository:

```bash
git clone <repository-url>
cd gold-app
```

### Backend

```bash
cd backend
npm install
npx prisma generate
npm run dev
```

### Frontend

Open another terminal:

```bash
cd frontend
npm install
npm run dev
```

---

## Database Setup

Make sure the `DATABASE_URL` points to a valid MySQL database.

Generate the Prisma client:

```bash
npx prisma generate
```

For a fresh development database:

```bash
npx prisma db push
```

Then create the initial application users and stock record:

```bash
npm run seed
```

---

## Development URLs

Frontend:

```text
http://localhost:5173
```

Backend:

```text
http://localhost:4000
```

Health check:

```text
http://localhost:4000/api/health
```

---

## Security

The application includes:

- Password hashing using bcrypt
- JWT-based authentication
- Role-based authorization
- Server-side transaction validation
- Protected management routes
- File type validation for uploads
- Transaction status validation
- Stock validation before gold sales approval

Sensitive credentials are stored through environment variables and are not included in the repository.

---

## UI & UX

GoldTrade uses a clean light interface designed for operational workflows.

The interface includes:

- Responsive layouts
- Transaction status indicators
- Dashboard cards
- Transaction review modal
- Image preview
- Realtime toast notifications
- Data visualization
- Mobile-friendly navigation
- Smooth page transitions

---

## Production

The application is deployed as a full-stack web system with:

```text
Frontend + Backend
Hostinger

Database
MySQL

Image Storage
Cloudinary
```

Live URL:

```text
https://intranet-bmi.com
```

---

## Development Notes

This project focuses on:

- Transaction integrity
- Clear approval workflows
- Operational traceability
- Role separation
- Data validation
- Real-time communication
- Maintainable full-stack architecture

---

## Author

Developed as a full-stack transaction management system.

Role:

```text
Full-Stack Developer
System Design and Development
```

---

## License

This project is intended for portfolio and project demonstration purposes.

Commercial usage, redistribution, or modification should be discussed with the project owner.
