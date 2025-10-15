# Use a lightweight base image (python:3.10-slim)
FROM --platform=linux/amd64 python:3.10-slim

# Metadata
LABEL maintainer="Your Name <you@example.com>" \
      description="Challenge 1B: Persona-Driven PDF Analyzer (offline, CPU-only)" \
      version="1.0"

# Set working directory
WORKDIR /app

# Reduce interactive prompts & ensure reproducible builds
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PATH="/usr/local/bin:$PATH"

# Install only necessary system dependencies
# Combine layers to reduce image size
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        gcc \
        libpoppler-cpp-dev \
        poppler-utils && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Python dependencies in a single layer
COPY requirements.txt .
RUN pip install --upgrade pip setuptools wheel && \
    pip install --no-cache-dir -r requirements.txt

# Copy only necessary application files (avoid .git, caches, etc.)
COPY modules/ modules/
COPY run_1b.py .

# Run as non-root user for security
RUN useradd -m appuser
USER appuser

# Default command
ENTRYPOINT ["python", "run_1b.py"]
