# Use Node.js as the base image
FROM node:20-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy the rest of the application
COPY . .
RUN npm run build

# Install static file server
RUN npm install -g serve

# Expose the port
EXPOSE 5173

# Serve the built Vue app from the dist folder
CMD ["serve", "-s", "dist", "-l", "5173"]
