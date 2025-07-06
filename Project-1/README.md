# Project-1 Architecture Overview

```yaml
/django-microservices/
├── service_1_accounts/ # User authentication and roles
├── service_2_restaurants/ # Restaurant management
├── service_3_employee/ # Employee management per restaurant
├── common_libs/ # Shared logic: env loader, API clients, auth middleware
└── docker-compose.yml # Orchestration of all services
```
## 🔌 Services Overview

```yaml
| Service         | Description                         | Readme Link                                      |
|----------------|------------------------------------- --------------------------------------------------|
| `accounts`      | Manages users and authentication     | [service_1_accounts/README.md](./service_1_accounts/README.md) |
| `restaurants`   | Manages restaurants and owners       | [service_2_restaurants/README.md](./service_2_restaurants/README.md) |
| `employee`      | Handles restaurant employee data     | [service_3_employee/README.md](./service_3_employee/README.md) |
| `common_libs`   | Shared logic between all services    | [common_libs/README.md](./common_libs/README.md) *(optional)* |

---

## 🚀 Features

- ✅ Fully containerized (Docker + Compose)
- ✅ Environment-based config using `python-decouple`
- ✅ Isolated databases per service
- ✅ Cross-service communication via REST
- ✅ DRY principle using `common_libs/`
- ✅ Ready for production extensions: JWT, Celery, PostgreSQL, Redis

---

```

## 📦 Getting Started
To spin up all services:

```bash
docker-compose up --build
```

To run a service locally without Docker:
```bash
cd service_1_accounts
python manage.py runserver 8001
```
