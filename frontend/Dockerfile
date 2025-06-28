# Build stage
FROM node:20-alpine as builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

# Build the production frontend
RUN npm run build

# Production stage with NGINX
FROM nginx:stable-alpine

# Copy the built files
COPY --from=builder /app/dist /usr/share/nginx/html

# Remove default config and add custom one if needed
RUN rm /etc/nginx/conf.d/default.conf

# Simple NGINX config to serve SPA
RUN echo 'server { \
    listen 80; \
    server_name localhost; \
    root /usr/share/nginx/html; \
    index index.html; \
    location / { \
        try_files $uri $uri/ /index.html; \
    } \
}' > /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
