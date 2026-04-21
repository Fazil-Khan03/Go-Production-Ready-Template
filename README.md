# 🚀 Go + Gin Production-Ready Template

A minimal, scalable, and production-grade backend template using Go and Gin.
Designed for real-world systems: APIs, microservices, event-driven systems.

---

# 📦 Tech Stack

* Go (1.21+)
* Gin (HTTP framework)
* `database/sql` (DB layer)
* PostgreSQL (default, pluggable)
* Kafka / PubSub (eventing)
* Prometheus (metrics)
* Zap / Logrus (logging)

---

# 📁 Project Structure

```
/cmd/
   server/main.go

/internal/
   handler/        # HTTP layer
   service/        # business logic
   repository/     # DB queries
   model/          # DB models
   dto/            # API request/response
   middleware/     # HTTP middleware
   client/         # external services
   producer/       # event producers
   consumer/       # event consumers
   config/         # config loader
   db/             # DB init
   logger/         # logging setup
   errors/         # centralized errors

/pkg/              # shared utilities (optional)

/configs/          # config files
/scripts/          # scripts (migrations, etc.)
```

---

# 🧠 Architecture Flow

```
HTTP → Handler → Service → Repository → DB
                    ↓
                External Client
                    ↓
                 Producer
```

Strict separation. No layer skipping.

---

# ⚙️ Setup

## 1. Clone repo

```
git clone <your-repo>
cd project
```

## 2. Install dependencies

```
go mod tidy
```

## 3. Setup environment

Create `.env`:

```
APP_PORT=8080
DB_URL=postgres://user:pass@localhost:5432/dbname
```

---

# ▶️ Run Server

```
go run cmd/server/main.go
```

---

# 🔌 Database

Uses `database/sql`.

### Connection

```go
db, _ := sql.Open("postgres", cfg.DB.URL)
```

### Pooling

```go
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(10)
```

---

# 📡 API Example

### GET /books/:id

Response:

```
{
  "id": "1",
  "title": "Go in Action"
}
```

---

# 🧩 Layers Explained

## Handler

* HTTP parsing
* response formatting
* no business logic

## Service

* core logic
* orchestration

## Repository

* SQL queries only

## DTO

* API contracts

## Model

* DB schema mapping

---

# ⚠️ Error Handling

Centralized error system:

```
ErrNotFound
ErrInternal
ErrValidation
```

Mapped to HTTP responses.

---

# 🧾 Logging

* Structured logging (JSON)
* Request ID support
* Middleware-based

---

# 🔐 Middleware

* Logging
* Recovery
* Request ID
* Auth (extendable)

---

# 🌐 External API Integration

* Defined in `/internal/client`
* Always use:

  * context
  * timeout
  * retries (recommended)

---

# 📩 Event System

## Producer

* Publishes domain events (Kafka / PubSub)

## Consumer

* Background workers
* Handles async processing

---

# 📊 Observability

Add:

* `/health` endpoint
* `/metrics` (Prometheus)

---

# 🧪 Testing

* Unit tests for service layer
* Mock repository interfaces
* Use table-driven tests

---

# 🧠 Best Practices

* Pass `context.Context` everywhere
* Never expose DB models in API
* Keep handlers thin
* Prefer explicit over magic
* Avoid heavy ORMs (use raw SQL or sqlc)

---

# 🚨 Common Mistakes

* Putting logic in handlers ❌
* Ignoring context ❌
* No error categorization ❌
* Tight coupling between layers ❌

---

# 🔄 Graceful Shutdown

Handle SIGTERM:

```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()
server.Shutdown(ctx)
```

---

# 📦 Future Improvements

* Add OpenAPI (Swagger)
* Add circuit breaker (e.g. Hystrix pattern)
* Add rate limiting
* Add caching (Redis)

---

# 🏁 Final Thought

This template is intentionally:

* minimal
* explicit
* production-safe

It scales well without framework lock-in.

---

# 📜 License

MIT
