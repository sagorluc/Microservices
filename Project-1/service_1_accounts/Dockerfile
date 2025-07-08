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

# Start the Django development server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
