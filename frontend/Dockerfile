# Build stage
FROM node:20-alpine as builder

WORKDIR /app

# Accept environment variables as build arguments
ARG VITE_CLERK_PUBLISHABLE_KEY
ARG VITE_API_URL

# Inject them as actual environment variables
ENV VITE_CLERK_PUBLISHABLE_KEY=$VITE_CLERK_PUBLISHABLE_KEY
ENV VITE_API_URL=$VITE_API_URL

COPY package*.json ./
RUN npm ci

COPY . .

# Inject them manually into a .env file Vite understands
RUN echo "VITE_CLERK_PUBLISHABLE_KEY=$VITE_CLERK_PUBLISHABLE_KEY" >> .env && \
    echo "VITE_API_URL=$VITE_API_URL" >> .env

RUN npm run build

# Serve with NGINX
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
