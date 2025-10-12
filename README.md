# 🏢 Acquisitions API

A modern, secure Node.js Express API for acquisitions system management with authentication, user management, and comprehensive security features.

![Node.js](https://img.shields.io/badge/Node.js-18.20.8-green)
![Express](https://img.shields.io/badge/Express-5.1.0-blue)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon-blue)
![Docker](https://img.shields.io/badge/Docker-Ready-blue)
![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-green)

## 🚀 Features

- **🔐 Authentication & Authorization**: JWT-based auth with role-based access control
- **👥 User Management**: Complete CRUD operations for user accounts
- **🛡️ Security**: Integrated with Arcjet for bot protection and rate limiting
- **📊 Database**: PostgreSQL with Drizzle ORM and migrations
- **🐳 Docker Support**: Full containerization with multi-stage builds
- **🔄 CI/CD**: Automated testing, linting, and Docker image deployment
- **📋 Testing**: Comprehensive test suite with Jest and SuperTest
- **📈 Monitoring**: Health checks and structured logging with Winston
- **🌐 Development Tools**: Hot reload, linting, formatting, and code quality

## 🏗️ Architecture

```
src/
├── config/          # Configuration files (database, logger, arcjet)
├── controllers/     # Route handlers and business logic
├── middleware/      # Custom middleware (auth, security)
├── models/          # Database models with Drizzle ORM
├── routes/          # API route definitions
├── services/        # Business logic and data access layer
├── utils/           # Utility functions (JWT, cookies, formatting)
├── validations/     # Input validation schemas with Zod
├── app.js           # Express app configuration
├── index.js         # Application entry point
└── server.js        # Server initialization
```

## 📋 Prerequisites

Before running this project, make sure you have:

- **Node.js** (v18.20.8 or higher)
- **npm** (v10.8.2 or higher)
- **Docker** and **Docker Compose**
- **Git**
- **Neon Database Account** (for production) or Docker for local development

## 🛠️ Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/RISHII7/acquisitions.git
cd acquisitions
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Environment Configuration

#### Development Environment

Create `.env.development` file:

```env
# Node.js Environment
NODE_ENV=development
PORT=3000
LOG_LEVEL=debug

# Database (Neon Local - handled by Docker)
DATABASE_URL=postgres://neon:npg@localhost:5432/neondb?sslmode=require
DATABASE_NAME=neondb

# Neon Configuration (Get from https://neon.tech)
NEON_API_KEY=your_neon_api_key_here
NEON_PROJECT_ID=your_neon_project_id_here
PARENT_BRANCH_ID=your_parent_branch_id_here
DELETE_BRANCH=true

# Security (Get from https://arcjet.com)
ARCJET_KEY=your_arcjet_key_here

# JWT Secret (Use a strong secret in production)
JWT_SECRET=your-development-jwt-secret
```

#### Production Environment

Create `.env.production` file:

```env
# Database (Direct Neon Cloud connection)
DATABASE_URL=postgresql://username:password@host/database?sslmode=require&channel_binding=require

# Application Settings
JWT_SECRET=your-strong-production-jwt-secret-key-here
PORT=3000
LOG_LEVEL=info
CORS_ORIGIN=https://yourdomain.com

# Security
ARCJET_KEY=your_production_arcjet_key_here
```

### 4. Database Setup

The project uses Drizzle ORM with PostgreSQL. Database migrations are handled automatically.

#### For Development (with Neon Local):

```bash
npm run dev:docker
```

#### For Production (with Neon Cloud):

```bash
npm run db:migrate
```

## 🚀 Running the Application

### Development Mode

#### Option 1: With Docker (Recommended)

```bash
npm run dev:docker
```

This will:

- Start Neon Local proxy (ephemeral database branch)
- Run the application with hot reload
- Available at: http://localhost:3000

#### Option 2: Local Development

```bash
npm run dev
```

Runs the app in development mode with hot reload using Node.js `--watch` flag.

### Production Mode

#### With Docker:

```bash
npm run prod:docker
```

#### Direct Node.js:

```bash
npm start
```

## 📜 Available Scripts

### Development

- `npm run dev` - Start development server with hot reload
- `npm run dev:docker` - Start development environment with Docker & Neon Local

### Production

- `npm start` - Start production server
- `npm run prod:docker` - Start production environment with Docker

### Database

- `npm run db:generate` - Generate database migrations
- `npm run db:migrate` - Run database migrations
- `npm run db:studio` - Open Drizzle Studio (database GUI)

### Code Quality

- `npm run lint` - Run ESLint
- `npm run lint:fix` - Fix ESLint issues
- `npm run format` - Format code with Prettier
- `npm run format:check` - Check code formatting

### Testing

- `npm test` - Run all tests with coverage
- `npm run test:watch` - Run tests in watch mode

## 🧪 API Endpoints

### Health & Status

- `GET /health` - Health check endpoint
- `GET /api` - API welcome message

### Authentication

- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/me` - Get current user profile

### Users (Protected Routes)

- `GET /api/users` - Get all users (Admin only)
- `GET /api/users/:id` - Get user by ID
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user (Admin only)

### Example API Usage

#### Register a new user:

```bash
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securepassword123"
  }'
```

#### Login:

```bash
curl -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "securepassword123"
  }'
```

## 🐳 Docker Configuration

### Development

- **Image**: Built with development dependencies
- **Hot Reload**: Source code mounted as volume
- **Database**: Neon Local proxy container
- **Port**: 3000

### Production

- **Image**: Optimized multi-stage build
- **Security**: Non-root user, minimal dependencies
- **Database**: Direct connection to Neon Cloud
- **Health Checks**: Built-in health monitoring

## 🔧 Environment Variables Reference

| Variable           | Description                   | Required  | Default       |
| ------------------ | ----------------------------- | --------- | ------------- |
| `NODE_ENV`         | Node environment              | No        | `development` |
| `PORT`             | Server port                   | No        | `3000`        |
| `LOG_LEVEL`        | Logging level                 | No        | `info`        |
| `DATABASE_URL`     | PostgreSQL connection string  | Yes       | -             |
| `JWT_SECRET`       | JWT signing secret            | Yes       | -             |
| `ARCJET_KEY`       | Arcjet API key for security   | Yes       | -             |
| `NEON_API_KEY`     | Neon API key (dev only)       | Yes (dev) | -             |
| `NEON_PROJECT_ID`  | Neon project ID (dev only)    | Yes (dev) | -             |
| `PARENT_BRANCH_ID` | Neon parent branch (dev only) | Yes (dev) | -             |
| `CORS_ORIGIN`      | CORS allowed origins          | No        | `*`           |

## 🧪 Testing

The project includes comprehensive testing with:

- **Unit Tests**: Individual component testing
- **Integration Tests**: API endpoint testing
- **Coverage Reports**: Code coverage analysis

```bash
# Run all tests
npm test

# Run tests with watch mode
npm run test:watch

# Generate coverage report
npm test -- --coverage
```

Test files are located in the `tests/` directory.

## 🚦 CI/CD Pipeline

The project includes GitHub Actions workflows for:

### 1. Tests (`tests.yml`)

- Runs on pull requests and pushes
- Tests against multiple Node.js versions
- Includes code coverage reporting

### 2. Lint and Format (`lint-and-format.yml`)

- Code quality checks
- Prettier formatting validation
- ESLint compliance

### 3. Docker Build and Push (`docker-build-and-push.yml`)

- Multi-platform builds (AMD64, ARM64)
- Automated Docker Hub publishing
- Production-ready image tagging

## 🔒 Security Features

- **Arcjet Integration**: Bot protection and rate limiting
- **JWT Authentication**: Secure token-based auth
- **Helmet**: Security headers
- **CORS**: Cross-origin request handling
- **Input Validation**: Zod schema validation
- **Password Hashing**: Bcrypt encryption
- **Security Middleware**: Custom security policies

## 📊 Monitoring & Logging

- **Winston Logger**: Structured logging with different levels
- **Health Checks**: Built-in endpoint for monitoring
- **Request Logging**: Morgan middleware for HTTP logging
- **Error Handling**: Centralized error management

## 🚀 Deployment

### Production Deployment Steps:

1. **Set up your production database** (Neon Cloud recommended)
2. **Configure environment variables** in your hosting platform
3. **Build and deploy** using Docker:

```bash
# Build production image
docker build --target production -t acquisitions-app:prod .

# Run production container
docker run -d \
  --name acquisitions-app \
  --env-file .env.production \
  -p 3000:3000 \
  acquisitions-app:prod
```

### Docker Hub Images

Pre-built images are available at:

```
docker pull rishii7/acquisitions-app:latest
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines:

- Follow the existing code style
- Write tests for new features
- Update documentation as needed
- Ensure all CI checks pass

## 📝 License

This project is licensed under the ISC License. See the `package.json` file for details.

## 👨‍💻 Author

**Rishikesh Palande**

- GitHub: [@RISHII7](https://github.com/RISHII7)
- Project: [acquisitions](https://github.com/RISHII7/acquisitions)

## 🆘 Troubleshooting

### Common Issues:

#### 1. Docker Permission Errors

```bash
# On Linux/macOS, add your user to docker group
sudo usermod -aG docker $USER
```

#### 2. Port Already in Use

```bash
# Find and kill process using port 3000
lsof -ti:3000 | xargs kill
```

#### 3. Database Connection Issues

- Ensure your `DATABASE_URL` is correct
- Check if Neon database is accessible
- Verify SSL requirements

#### 4. Environment Variables Not Loaded

- Ensure `.env.development` or `.env.production` exists
- Check file permissions
- Verify variable names match exactly

### Getting Help:

- Check the [Issues](https://github.com/RISHII7/acquisitions/issues) page
- Create a new issue with detailed information
- Include error logs and environment details

---

**Made with ❤️ by Rishikesh Palande**
