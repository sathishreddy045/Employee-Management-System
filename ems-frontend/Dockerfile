# Build stage
FROM node:18-alpine AS build

WORKDIR /app

# Copy package files first for better caching
COPY package*.json ./

# Install ALL dependencies (including devDependencies for build)
RUN npm ci

# Copy source code
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM nginx:alpine

# Copy custom nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Copy built application
COPY --from=build /app/dist /usr/share/nginx/html

# Create pid file directory
RUN mkdir -p /var/run/nginx && \
    chown -R nginx:nginx /var/run/nginx

EXPOSE 3000

CMD ["nginx", "-g", "daemon off;"]
