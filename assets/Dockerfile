# Optimized MultiStage Build
# Builder stage
FROM python:3.12-alpine AS builder

WORKDIR /app

# Install build dependencies (so pip can compile wheels if needed)
RUN apk add --no-cache build-base

COPY requirements.txt .
RUN pip wheel --no-cache-dir --no-deps -r requirements.txt -w /wheels

# Final runtime stage
FROM python:3.12-alpine

WORKDIR /app

# Copy only the wheels from builder
COPY --from=builder /wheels /wheels
RUN pip install --no-cache-dir /wheels/*

# Copy only your app code
COPY app.py .

CMD [ "flask", "--app", "app", "run", "--host=0.0.0.0" ]


# Single Stage Build

# FROM python:3.12-alpine

# WORKDIR /app

# COPY requirements.txt .  
# RUN pip install -r requirements.txt
# WORKDIR /app
# COPY app.py .  
# CMD [ "flask", "--app", "app", "run" ]


# MultiStage Build
# FROM python:3.12-alpine AS builder

# WORKDIR /app

# COPY requirements.txt .  
# RUN pip install -r requirements.txt

# FROM python:3.12-slim
# WORKDIR /app
# COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
# COPY app.py .  
# CMD [ "flask", "--app", "app", "run" ]