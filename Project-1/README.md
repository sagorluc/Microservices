# Project-1 Architecture Overview And Run Locally By Docker OR Without Docker

```yaml
/django-microservices/
├── service_1_accounts/
    ├── apps
    ├── accounts
        ├── settings.py
    ├── vrenv
    .gitignore
    Dockerfile
    manage.py
    readme.md
    requirements.py

├── service_2_restaurants/ 
    ├── apps
    ├── restaurants
        ├── settings.py
    ├── vrenv
    .gitignore
    Dockerfile
    manage.py
    readme.md
    requirements.py

├── service_3_employees/ 
    ├── apps
    ├── employees
        ├── settings.py
    ├── vrenv
    .gitignore
    Dockerfile
    manage.py
    readme.md
    requirements.py

├── common_libs/ 
    __init__.py
    .env

└── .gitignore
└── docker-compose.yml # Orchestration of all services
└── README.md
```
## 🚀 Features
```bash
- ✅ Fully containerized (Docker + Compose)
- ✅ Environment-based config using `python-decouple`
- ✅ Isolated databases per service
- ✅ Cross-service communication via REST
- ✅ DRY principle using `common_libs/`
- ✅ Ready for production extensions: JWT, Celery, PostgreSQL, Redis
```

## Dockerfile Configaretion
```Dockerfile```
```dockerfile
# Use official Python image from docker
FROM python:3.10

# Ensure output is not buffered (helpful for Docker logs)
ENV PYTHONUNBUFFERED=1

# Set working directory inside container
WORKDIR /app

# Copy and install requirements
COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

# Copy the entire application code
COPY . .

# Expose port (optional, mostly for documentation)
EXPOSE 8000
# EXPOSE 8001 (expose for restaurants)
# EXPOSE 8002 (expose for employees)

# Start the Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

## Docker Compose File Configaretion
```docker-compose.yml```
```yml
version: "3.9"

services:
  accounts:
    build:
      context: ./service_1_accounts
    ports:
      - "8000:8000"
    volumes:
      - ./service_1_accounts:/app
      - ./common_libs:/common_libs  # shared .env directory
    working_dir: /app
    command: >
      sh -c "python manage.py makemigrations &&
            python manage.py migrate &&
            python manage.py runserver 0.0.0.0:8000"

  restaurants:
    build:
      context: ./service_2_restaurants
    ports:
      - "8001:8001"
    volumes:
      - ./service_2_restaurants:/app
      - ./common_libs:/common_libs  # shared .env directory
    working_dir: /app
    command: >
      sh -c "python manage.py makemigrations &&
            python manage.py migrate &&
            python manage.py runserver 0.0.0.0:8001"

  employees:
    build:
      context: ./service_3_employees
    ports:
      - "8002:8002"
    volumes:
      - ./service_3_employees:/app
      - ./common_libs:/common_libs  # shared .env directory
    working_dir: /app
    command: >
      sh -c "python manage.py makemigrations &&
            python manage.py migrate &&
            python manage.py runserver 0.0.0.0:8002"
  
```

## 📦 Getting Started
To run a service locally without Docker:

```bash
# For accounts
cd service_1_accounts
python manage.py runserver 8000

# For restaurants
cd service_2_restaurants
python manage.py runserver 8001

# For employees
cd service_3_employees
python manage.py runserver 8002
```

To spin up all services by docker command:

```bash
# If you want logs + auto-rebuild on code changes (in dev)
docker-compose up --build
docker-compose build --no-cache # without cache

# Then run the server in docker
docker-compose up

# To stop them cleanly:
CTRL+C

# Then to remove the containers:
docker-compose down

# 📦 View Docker Images
docker images OR # all locally built/downloaded
docker image ls

# 🧱 View Docker Containers
docker ps # running containers only
docker ps -a # all containers

# 🧹 Delete Docker Images
docker rmi <image_id or name> # Delete a specific image
docker rmi $(docker images -q) # Delete all images

# 🚫 Delete Docker Containers
docker rm <container_id or name> #  Delete a specific container (must be stopped)

# Stop and delete all containers
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)


# 🧼 Cleanup (Dangling/Unused)
# Remove all unused images, networks, and containers
docker system prune -a
    - -a = remove all unused images (not just dangling)
    - This will prompt: Are you sure you want to continue? [y/N]

# To run it without prompt
docker system prune -a -f

```

