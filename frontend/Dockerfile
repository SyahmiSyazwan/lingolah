# Build stage
FROM node:20-alpine as builder
WORKDIR /app

# Copy and load .env first
COPY .env .env

# Load the env variables
RUN export $(grep -v '^#' .env | xargs) && \
    echo "VITE_CLERK_PUBLISHABLE_KEY=$VITE_CLERK_PUBLISHABLE_KEY" >> .env && \
    echo "VITE_API_URL=$VITE_API_URL" >> .env

COPY package*.json ./
RUN npm ci

COPY . .

RUN npm run build

# Production stage with NGINX
FROM nginx:stable-alpine
COPY --from=builder /app/dist /usr/share/nginx/html

RUN rm /etc/nginx/conf.d/default.conf
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
