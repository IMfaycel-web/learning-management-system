# Learning Management System

A full-stack learning management platform for publishing, purchasing, and consuming online courses. It provides separate learner and administrator experiences with secure authentication, course management, protected lectures, media uploads, payments, and reporting.

## Overview

The platform allows visitors to explore courses, registered users to purchase access and watch protected lectures, and administrators to manage course content and operational data.

The backend exposes a versioned REST API, while the frontend uses centralized state management and protected routes to deliver role-aware experiences.

## Features

### Learner Experience

- User registration and login
- Cookie-based authentication
- Public course browsing
- Course purchase and payment verification
- Subscription-protected lecture access
- Profile and avatar management
- Password changes and recovery
- Contact form support
- Responsive course and account pages

### Administration

- Role-protected administration dashboard
- Course creation and metadata updates
- Course thumbnail uploads
- Lecture video uploads and deletion
- User statistics
- Payment history and monthly analytics
- Protected routes for administrative operations

### Platform Capabilities

- Versioned REST API
- Role-based authorization
- Subscription-based content access
- MongoDB persistence
- Cloud-hosted media storage
- Payment verification
- SMTP email delivery
- Centralized error handling
- Request logging
- File type and upload size validation

## Tech Stack

| Area | Technologies |
| --- | --- |
| Frontend | React 18, Create React App |
| Routing | React Router DOM 6 |
| State management | Redux Toolkit, React Redux |
| HTTP client | Axios |
| Styling | Tailwind CSS, DaisyUI |
| Notifications | React Hot Toast |
| Charts | Chart.js, React Chart.js 2 |
| Backend | Node.js, Express 4 |
| Database | MongoDB |
| Object modeling | Mongoose |
| Authentication | JSON Web Tokens, HTTP cookies |
| Password security | bcryptjs |
| File uploads | Multer |
| Media storage | Cloudinary |
| Payments | Razorpay |
| Email | Nodemailer |
| Logging | Morgan |
| Configuration | dotenv |

## Installation

### Requirements

- Node.js 16 or later
- npm 8 or later
- MongoDB
- Cloudinary account
- Razorpay account
- SMTP email account

### Backend Setup

Install backend dependencies:

```bash
cd backend
npm install
```

Create a `.env` file inside `backend`:

```env
PORT=5000
NODE_ENV=development
FRONTEND_URL=local_frontend_origin

MONGO_URI=mongodb_connection_string

JWT_SECRET=replace_with_a_long_random_secret
JWT_EXPIRY=7d

CLOUDINARY_CLOUD_NAME=replace_with_cloud_name
CLOUDINARY_API_KEY=replace_with_api_key
CLOUDINARY_API_SECRET=replace_with_api_secret

RAZORPAY_KEY_ID=replace_with_key_id
RAZORPAY_SECRET=replace_with_key_secret

SMTP_HOST=smtp.example.com
SMTP_PORT=587
SMTP_USERNAME=replace_with_smtp_username
SMTP_PASSWORD=replace_with_smtp_password
SMTP_FROM_EMAIL=no-reply@example.com
CONTACT_US_EMAIL=support@example.com
```

Start the backend development server:

```bash
npm run dev
```

The backend runs on port `5000` by default.

### Frontend Setup

Open another terminal:

```bash
cd client
npm install
npm start
```

The frontend runs on port `3000` by default.

The API base URL is currently configured in:

```text
client/src/Helper/axiosInstance.js
```

For local development, configure it to use the backend on port `5000` with the `/api/v1` prefix.

For production, replace the hardcoded value or move it to a build-time environment variable.

## Usage

### Learner Workflow

1. Register a new account.
2. Log in with email and password.
3. Browse the public course catalog.
4. Select a course and complete payment.
5. Verify the payment through the backend.
6. Open protected lectures after subscription activation.
7. Manage profile information and avatar.
8. Change or recover the account password.

### Administrator Workflow

1. Log in with an administrator account.
2. Open the protected administration dashboard.
3. Create a course and upload its thumbnail.
4. Update course details when required.
5. Add lecture videos to the course.
6. Remove outdated lectures.
7. Review user and payment statistics.
8. Monitor course and payment activity.

## API

The main API is served under:

```text
/api/v1
```

### System

| Method | Path | Access | Purpose |
| --- | --- | --- | --- |
| `GET` | `/ping` | Public | Check server availability |

The health-check route is mounted outside the versioned API prefix.

### Users

| Method | Path | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/user/register` | Public | Register a user |
| `POST` | `/user/login` | Public | Log in and create the authentication cookie |
| `POST` | `/user/logout` | User | Clear the authentication cookie |
| `GET` | `/user/me` | User | Return the current user |
| `POST` | `/user/reset` | Public | Send a password-reset email |
| `POST` | `/user/reset/:resetToken` | Public | Reset a password |
| `POST` | `/user/change-password` | User | Change the current password |
| `PUT` | `/user/update/:id` | User | Update profile information |

### Courses

| Method | Path | Access | Purpose |
| --- | --- | --- | --- |
| `GET` | `/courses` | Public | List available courses |
| `POST` | `/courses` | Admin | Create a course |
| `GET` | `/courses/:id` | Admin or subscriber | Return course lectures |
| `POST` | `/courses/:id` | Admin | Add a lecture |
| `PUT` | `/courses/:id` | Admin | Update course metadata |
| `DELETE` | `/courses` | Admin | Remove a lecture |

### Payments

| Method | Path | Access | Purpose |
| --- | --- | --- | --- |
| `POST` | `/payments/subscribe` | User | Create a Razorpay order |
| `POST` | `/payments/verify` | User | Verify payment and activate access |
| `POST` | `/payments/unsubscribe` | Subscriber | Cancel application access |
| `GET` | `/payments/razorpay-key` | User | Return the checkout key |
| `GET` | `/payments` | Admin | Return payment records and analytics |


Production deployments should:

- Use a strong and unique JWT secret
- Use a secured MongoDB instance
- Configure HTTPS
- Restrict allowed frontend origins
- Protect Cloudinary, Razorpay, and SMTP credentials
- Verify cookie security settings
- Use production provider credentials intentionally
- Store environment files outside version control
- Validate upload limits and allowed file types
- Monitor payment verification and email failures

## Available Scripts

### Backend

Run from `backend`:

```bash
npm run dev
```

Starts the Express server with automatic restart.

```bash
npm start
```

Starts the Express server with Node.js.

### Frontend

Run from `client`:

```bash
npm start
```

Starts the React development server.

```bash
npm run build
```

Creates a production frontend build.

```bash
npm test
```

Runs the React test runner.

## Deployment

### Backend

```bash
cd backend
npm install
npm start
```

Before starting the production service:

- Set `NODE_ENV=production`
- Configure the production frontend origin
- Use a production MongoDB connection
- Provide Cloudinary credentials
- Provide Razorpay credentials
- Provide SMTP credentials
- Confirm secure cookie behavior behind HTTPS


### Frontend

```bash
cd client
npm install
npm run build
```

The production build is generated inside:

```text
client/build
```

Ensure the frontend API base URL points to the deployed backend before building.

## Contributing

Create a focused branch and keep frontend, backend, API, and configuration changes within their existing responsibilities.

Before submitting a change:

- Add tests where practical
- Run the frontend test suite
- Create a production frontend build
- Verify backend startup
- Test authentication and protected routes
- Confirm administrative authorization
- Validate upload workflows
- Test payment verification
- Test password-reset and contact emails
- Avoid committing environment files or credentials
- Keep changes focused and clearly documented