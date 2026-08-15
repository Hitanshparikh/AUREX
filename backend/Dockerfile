FROM python:3.12-slim

WORKDIR /app

# Set environment variables for pip resiliency against transient PyPI CDN 502 errors
ENV PIP_DEFAULT_TIMEOUT=100 \
    PIP_RETRIES=10 \
    PYTHONUNBUFFERED=1

# Install build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip and install python dependencies with retries & backoff
COPY requirements.txt .
RUN pip install --upgrade pip && \
    pip install --no-cache-dir --retries 10 --timeout 100 -r requirements.txt

# Copy application source code
COPY . .

# Expose default port
EXPOSE 8000

# Start FastAPI server with dynamic PORT fallback
CMD ["sh", "-c", "uvicorn main:app --host 0.0.0.0 --port ${PORT:-8000}"]

