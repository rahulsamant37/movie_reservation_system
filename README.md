# Movie Reservation System 🎬

A comprehensive cinema ticket reservation system built with FastAPI, implementing Clean Architecture and Domain-Driven Design principles. This project demonstrates modern backend development practices with a focus on maintainability, scalability, and testability.

## 🚀 Features

- **User Management**: User registration, authentication with JWT tokens
- **Movie Management**: CRUD operations for movies with genre assignment and poster image upload
- **Showtime Management**: Schedule movie showtimes in different rooms
- **Room & Seat Management**: Create cinema rooms with configurable seat layouts
- **Reservation System**: Book seats with automatic expiration for unpaid reservations
- **Payment Integration**: Stripe payment processing with webhook support
- **Event-Driven Architecture**: RabbitMQ for handling domain events
- **Image Storage**: AWS S3 integration for movie poster images
- **Background Jobs**: Automated cleanup of expired reservations

## 🏗️ Architecture

This project follows **Clean Architecture** principles with clear separation of concerns:

```
app/
├── auth/           # Authentication bounded context
├── movies/         # Movie management bounded context  
├── reservations/   # Reservation bounded context
├── payments/       # Payment processing bounded context
├── rooms/          # Room management bounded context
├── showtimes/      # Showtime scheduling bounded context
├── users/          # User management bounded context
└── shared/         # Shared kernel components
```

Each bounded context is organized with:
- **Domain Layer**: Business logic, entities, value objects, domain services
- **Application Layer**: Use cases (commands/queries), application services
- **Infrastructure Layer**: External concerns (database, APIs, messaging)

### Key Patterns

- **CQRS**: Commands for write operations, queries for read operations
- **Event Sourcing**: Domain events for cross-context communication
- **Repository Pattern**: Data access abstraction
- **Dependency Injection**: Loose coupling between layers

## 🛠️ Tech Stack

**Backend Framework**
- FastAPI - Modern, fast web framework for building APIs
- Pydantic - Data validation and serialization
- SQLModel - SQL databases in Python with SQLAlchemy

**Database & Storage**
- PostgreSQL - Primary database
- Alembic - Database migrations
- AWS S3 - Image storage

**Message Broker**
- RabbitMQ - Event-driven communication

**Payment Processing**
- Stripe - Payment gateway integration

**Development & Testing**
- Poetry - Dependency management
- Pytest - Testing framework
- MyPy - Static type checking
- Ruff - Code linting and formatting
- Pre-commit - Git hooks

**Infrastructure**
- Docker & Docker Compose - Containerization
- Gunicorn + Uvicorn - Production ASGI server

## 📋 Requirements

- [Docker](https://www.docker.com/) (v20.10+)
- [Docker Compose](https://docs.docker.com/compose/) (v2.0+)
- [Poetry](https://python-poetry.org/) (for local development)
- Python 3.11+

## 🚀 Quick Start

### Using Docker (Recommended)

1. **Clone the repository**
   ```bash
   git clone https://github.com/rahulsamant37/movie_reservation_system.git
   cd movie_reservation_system
   ```

2. **Set up environment variables**
   ```bash
   touch .env
   # Edit .env with your configuration
   ```

3. **Start the application**
   ```bash
   docker compose up -d
   ```

4. **Access the application**
   - API: http://localhost/api/v1/
   - Documentation: http://localhost/docs
   - RabbitMQ Management: http://localhost:15672

### Local Development

1. **Install dependencies**
   ```bash
   poetry install
   ```

2. **Start infrastructure services**
   ```bash
   docker compose up -d db rabbitmq
   ```

3. **Run database migrations**
   ```bash
   poetry run alembic upgrade head
   ```

4. **Start the development server**
   ```bash
   poetry run uvicorn app.main:app --reload
   ```

## 🧪 Testing

Run the complete test suite:
```bash
make tests
# or
poetry run pytest app
```

Run specific test types:
```bash
# Unit tests only
poetry run pytest app -m "not integration"

# Integration tests only  
poetry run pytest app -m integration
```

## 🔧 Code Quality

Check code quality:
```bash
make check
```

Auto-format code:
```bash
make format
```

Individual commands:
```bash
# Type checking
poetry run mypy app

# Linting
poetry run ruff check app

# Formatting
poetry run ruff format app
```

## 📊 API Documentation

The API follows OpenAPI 3.0 specification. Once the application is running, you can access:

- **Interactive Documentation**: http://localhost/docs (Swagger UI)
- **Alternative Documentation**: http://localhost/redoc (ReDoc)
- **OpenAPI Schema**: http://localhost/api/v1/openapi.json

### Key Endpoints

- `POST /api/v1/auth/access-token/` - User authentication
- `POST /api/v1/users/` - User registration
- `GET /api/v1/movies/` - List movies
- `POST /api/v1/reservations/` - Create reservation
- `GET /api/v1/showtimes/{id}/seats/` - View available seats

## 🔐 Environment Configuration

Key environment variables:

```bash
# Application
SECRET_KEY=your-secret-key
ENVIRONMENT=local|staging|production

# Database
POSTGRES_SERVER=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=password
POSTGRES_DB=movie_reservation

# Stripe
STRIPE_API_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# AWS S3
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_S3_BUCKET_NAME=your-bucket

# RabbitMQ
RABBITMQ_HOST=localhost
RABBITMQ_USER=guest
RABBITMQ_PASSWORD=guest
```

## 🗄️ Database Schema

The application uses PostgreSQL with the following main entities:

- **Users**: Authentication and user management
- **Movies**: Movie catalog with genres and poster images
- **Rooms**: Cinema rooms with seat configurations
- **Showtimes**: Movie screening schedules
- **Reservations**: Ticket bookings with seat assignments
- **Seats**: Individual seat tracking per showtime

## 🎯 Business Rules

- Reservations expire after 30 minutes if payment is not completed
- Seats are locked during the reservation process
- Users can only cancel their own reservations
- Only superusers can manage movies, rooms, and showtimes
- Payment confirmation is handled via Stripe webhooks

## 🚀 Deployment

### Production Considerations

1. **Environment Variables**: Ensure all production secrets are properly configured
2. **Database**: Use managed PostgreSQL service
3. **File Storage**: Configure AWS S3 or compatible service
4. **Message Broker**: Use managed RabbitMQ or similar service
5. **Monitoring**: Add logging, metrics, and health checks
6. **Security**: Enable HTTPS, configure CORS, and rate limiting

### Docker Production

```bash
# Build production image
docker build -f compose/backend/Dockerfile --build-arg INSTALL_DEV=false .

# Run with production settings
ENVIRONMENT=production docker compose up -d
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Run tests and quality checks (`make check && make tests`)
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**rahulsamant37** - [GitHub](https://github.com/rahulsamant37)

---

*This project serves as a demonstration of modern backend development practices using Python, FastAPI, and Clean Architecture principles.*