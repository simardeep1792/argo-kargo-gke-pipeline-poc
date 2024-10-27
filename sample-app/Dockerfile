# Use a lightweight Python base image
FROM python:3.9-slim

# Set the working directory
WORKDIR /app

# Copy requirements file and install dependencies
COPY app/requirements.txt .
RUN pip install -r requirements.txt

# Copy application code
COPY app/ .

# Expose the application port
EXPOSE 8080

# Start the application using Gunicorn
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:8080", "app:app"]
