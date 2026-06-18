# Use a lightweight Python base image
FROM python:3.11-slim

# Prevent Python from writing .pyc files and force stdout to be unbuffered (better logging)
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Set the working directory inside the container
WORKDIR /app

# Install dependencies first (this caches the layer so rebuilds are faster)
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Copy your actual Django code into the container
COPY . /app/

# Expose the API port
EXPOSE 8000

# Run Gunicorn instead of manage.py runserver for better stability
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "core.wsgi:application"]
