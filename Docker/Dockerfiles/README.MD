# Dockerfiles

## 1. Flask (Production Ready)

Best for: Python web apps using Gunicorn. Key features: No cache to save space, unbuffered logs for real-time debugging.

``` docker
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

# --no-cache-dir tells pip: "Don't save the downloaded files", saving space.
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Ensure logs appear immediately
ENV PYTHONUNBUFFERED=1

EXPOSE 5000

# Start the production server
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]  
```

## 2. Node.js (Alpine)
Best for: Lightweight Javascript applications.


``` docker
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --production

COPY . .

CMD ["node", "server.js"]
```
