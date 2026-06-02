# AI Interview Report Generator

A full-stack AI-powered interview preparation application that helps users generate personalized interview reports from a resume, a self-description, and a target job description.

The project contains a React frontend and an Express backend. Users can register, log in, upload a resume PDF, enter job-specific context, generate an interview report, view previous reports, and generate a resume PDF based on the saved interview report data.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Environment Variables](#environment-variables)
- [How to Run the Project](#how-to-run-the-project)
- [Frontend Commands](#frontend-commands)
- [Backend Commands](#backend-commands)
- [Application Routes](#application-routes)
- [Backend API Routes](#backend-api-routes)
- [Data Flow](#data-flow)
- [Troubleshooting](#troubleshooting)
- [Development Notes](#development-notes)

## Project Overview

This application is designed for interview preparation. It takes user-provided career information and a target job description, then uses AI to generate a structured interview report.

The generated report can include:

- A match score for the candidate against the job description
- Technical interview questions
- Behavioral interview questions
- Suggested answers
- Skill gaps
- A preparation plan
- A generated resume PDF based on the report context

Authentication is handled with cookies and JWTs. The backend stores users and interview reports in MongoDB.

## Features

### User Authentication

- Register a new account
- Log in with email and password
- Maintain login state using cookies
- Log out and blacklist the active token
- Protect private frontend pages

### Interview Report Generation

- Submit a job description
- Submit a self-description
- Upload a resume PDF
- Extract text from the uploaded PDF
- Generate a personalized AI interview report
- Save generated reports in MongoDB
- Fetch all reports for the logged-in user
- Fetch one detailed report by ID

### Resume PDF Generation

- Generate a PDF resume from an existing interview report
- Download the generated PDF from the backend

### Frontend Experience

- React single-page application
- Protected dashboard/home page
- Login and registration pages
- Interview report detail page
- API communication with Axios

## Tech Stack

### Frontend

- React 19
- Vite
- React Router
- Axios
- Sass
- ESLint

### Backend

- Node.js
- Express 5
- MongoDB
- Mongoose
- JWT authentication
- bcryptjs password hashing
- cookie-parser
- CORS
- multer file uploads
- pdf-parse for resume PDF text extraction
- Puppeteer for PDF generation
- Google GenAI SDK
- dotenv
- Zod

## Project Structure

```text
interview-ai-yt/
├── Backend/
│   ├── src/
│   │   ├── config/
│   │   │   └── database.js
│   │   ├── controllers/
│   │   │   ├── auth.controller.js
│   │   │   └── interview.controller.js
│   │   ├── middlewares/
│   │   │   ├── auth.middleware.js
│   │   │   └── file.middleware.js
│   │   ├── models/
│   │   │   ├── blacklist.model.js
│   │   │   ├── interviewReport.model.js
│   │   │   └── user.model.js
│   │   ├── routes/
│   │   │   ├── auth.routes.js
│   │   │   └── interview.routes.js
│   │   ├── services/
│   │   │   └── ai.service.js
│   │   └── app.js
│   ├── .env
│   ├── package.json
│   └── server.js
│
├── Frontend/
│   ├── public/
│   ├── src/
│   │   ├── features/
│   │   │   ├── auth/
│   │   │   └── interview/
│   │   ├── App.jsx
│   │   ├── app.routes.jsx
│   │   ├── main.jsx
│   │   └── style.scss
│   ├── package.json
│   ├── vite.config.js
│   └── README.md
│
└── README.md
```

## Prerequisites

Before running the project, make sure you have:

- Node.js installed
- npm installed
- MongoDB available locally or through a cloud database such as MongoDB Atlas
- A Google GenAI API key

You can check Node.js and npm with:

```bash
node -v
npm -v
```

## Environment Variables

The backend requires environment variables in:

```text
Backend/.env
```

Required variables:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
GOOGLE_GENAI_API_KEY=your_google_genai_api_key
```

### Environment Variable Details

`MONGO_URI`

The MongoDB connection string used by Mongoose.

Example:

```env
MONGO_URI=mongodb://127.0.0.1:27017/interview-ai
```

Or, for MongoDB Atlas:

```env
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/interview-ai
```

`JWT_SECRET`

A private secret used to sign authentication tokens.

Example:

```env
JWT_SECRET=my_super_secret_jwt_key
```

`GOOGLE_GENAI_API_KEY`

The API key used by the backend AI service.

Example:

```env
GOOGLE_GENAI_API_KEY=your_api_key_here
```

## How to Run the Project

This project has two separate apps:

- Backend runs on `http://localhost:3000`
- Frontend runs on `http://localhost:5173`

You need to run both at the same time in two terminals.

### 1. Install Backend Dependencies

From the project root:

```bash
cd Backend
npm install
```

### 2. Create Backend Environment File

Create a `.env` file inside the `Backend` folder and add:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
GOOGLE_GENAI_API_KEY=your_google_genai_api_key
```

### 3. Start the Backend Server

Inside the `Backend` folder:

```bash
npm run dev
```

The backend should start on:

```text
http://localhost:3000
```

Expected console message:

```text
Server is running on port 3000
```

### 4. Install Frontend Dependencies

Open a second terminal from the project root:

```bash
cd Frontend
npm install
```

### 5. Start the Frontend App

Inside the `Frontend` folder:

```bash
npm run dev
```

The frontend should start on:

```text
http://localhost:5173
```

Open that URL in your browser.

## Frontend Commands

Run these commands inside the `Frontend` folder.

### Start Development Server

```bash
npm run dev
```

Starts the Vite development server.

### Build for Production

```bash
npm run build
```

Creates a production build.

### Preview Production Build

```bash
npm run preview
```

Serves the production build locally.

### Run Linter

```bash
npm run lint
```

Runs ESLint on the frontend code.

## Backend Commands

Run these commands inside the `Backend` folder.

### Start Development Server

```bash
npm run dev
```

Starts the backend with nodemon:

```bash
npx nodemon server.js
```

### Test Command

```bash
npm test
```

The current backend test script is a placeholder and will print:

```text
Error: no test specified
```

## Application Routes

The frontend uses React Router.

| Route | Page | Access |
| --- | --- | --- |
| `/login` | Login page | Public |
| `/register` | Register page | Public |
| `/` | Home/dashboard page | Protected |
| `/interview/:interviewId` | Interview report detail page | Protected |

## Backend API Routes

The backend API base URL is:

```text
http://localhost:3000
```

### Auth Routes

| Method | Endpoint | Description | Access |
| --- | --- | --- | --- |
| `POST` | `/api/auth/register` | Register a new user | Public |
| `POST` | `/api/auth/login` | Log in an existing user | Public |
| `GET` | `/api/auth/logout` | Log out the current user | Public |
| `GET` | `/api/auth/get-me` | Get current logged-in user details | Private |

### Interview Routes

| Method | Endpoint | Description | Access |
| --- | --- | --- | --- |
| `POST` | `/api/interview/` | Generate a new interview report | Private |
| `GET` | `/api/interview/` | Get all interview reports for the logged-in user | Private |
| `GET` | `/api/interview/report/:interviewId` | Get one interview report by ID | Private |
| `POST` | `/api/interview/resume/pdf/:interviewReportId` | Generate a resume PDF for a report | Private |

## API Request Details

### Register

```http
POST /api/auth/register
```

Request body:

```json
{
  "username": "aditi",
  "email": "aditi@example.com",
  "password": "password123"
}
```

### Login

```http
POST /api/auth/login
```

Request body:

```json
{
  "email": "aditi@example.com",
  "password": "password123"
}
```

### Generate Interview Report

```http
POST /api/interview/
```

This endpoint expects `multipart/form-data`.

Fields:

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `jobDescription` | Text | Yes | Target job description |
| `selfDescription` | Text | Optional if resume is provided | User-written profile or background |
| `resume` | File | Optional if self-description is provided | Resume PDF file |

At least one of `resume` or `selfDescription` must be provided.

### Get All Interview Reports

```http
GET /api/interview/
```

Returns reports for the logged-in user, sorted by newest first.

### Get Interview Report by ID

```http
GET /api/interview/report/:interviewId
```

Returns one full report if it belongs to the logged-in user.

### Generate Resume PDF

```http
POST /api/interview/resume/pdf/:interviewReportId
```

Returns a PDF file as a binary response.

## Data Flow

1. User registers or logs in.
2. Backend creates a JWT and stores it in a cookie.
3. Frontend sends authenticated requests with cookies enabled.
4. User enters a job description and either uploads a resume PDF or enters a self-description.
5. Backend extracts text from the resume PDF if a file is uploaded.
6. Backend sends the resume text, self-description, and job description to the AI service.
7. AI service returns a structured interview report.
8. Backend saves the report in MongoDB.
9. Frontend displays the generated report.
10. User can revisit saved reports or generate a resume PDF.

## Database Models

### User

Stores registered user account details.

Typical fields include:

- Username
- Email
- Hashed password

### Interview Report

Stores the generated report and the original input context.

Important fields include:

- User reference
- Job description
- Resume text
- Self-description
- Report title
- Match score
- Technical questions
- Behavioral questions
- Skill gaps
- Preparation plan
- Created and updated timestamps

### Token Blacklist

Stores logged-out tokens so they cannot be reused.

## CORS and Cookies

The backend is configured to accept frontend requests from:

```text
http://localhost:5173
```

Axios is configured with:

```js
withCredentials: true
```

This allows browser cookies to be included in requests between the frontend and backend during local development.

## Troubleshooting

### Backend Does Not Start

Check that:

- You are inside the `Backend` folder
- Dependencies are installed with `npm install`
- `.env` exists inside `Backend`
- `MONGO_URI`, `JWT_SECRET`, and `GOOGLE_GENAI_API_KEY` are set
- MongoDB is running or your cloud database connection string is correct

### Frontend Cannot Reach Backend

Check that:

- Backend is running on `http://localhost:3000`
- Frontend is running on `http://localhost:5173`
- The backend CORS origin matches the frontend URL
- Axios base URL is `http://localhost:3000`

### Login Works But Protected Pages Do Not Load

Check that:

- Cookies are being set in the browser
- Backend responses include the token cookie
- Requests are sent with credentials enabled
- `JWT_SECRET` is the same between token creation and verification

### Interview Report Generation Fails

Check that:

- `GOOGLE_GENAI_API_KEY` is valid
- A job description is provided
- Either a resume PDF or self-description is provided
- The uploaded resume is a valid PDF
- MongoDB is connected

### PDF Generation Fails

Check that:

- The interview report ID is valid
- The report exists in MongoDB
- Puppeteer installed correctly during `npm install`

## Development Notes

- Backend server port is currently hardcoded to `3000` in `Backend/server.js`.
- Frontend API calls currently target `http://localhost:3000`.
- Backend CORS currently allows `http://localhost:5173`.
- The frontend and backend are installed and run separately.
- The backend `.env` file should not be committed to version control.
- The project currently does not include a real backend test suite.

## Quick Start

Use this condensed version after your environment variables are ready.

Terminal 1:

```bash
cd Backend
npm install
npm run dev
```

Terminal 2:

```bash
cd Frontend
npm install
npm run dev
```

Then open:

```text
http://localhost:5173
```
