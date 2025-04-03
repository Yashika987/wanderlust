#----------------------------- Stage 1: Builder ------------------------------
# Use a lightweight Node.js image
FROM node:21-slim AS backend-builder

# Set working directory inside the container
WORKDIR /app

# Copy package files first to optimize caching
COPY package*.json ./

# Install dependencies
RUN npm i 

# Copy the rest of the application files
COPY . .

#----------------------------- Stage 2: Final Image ------------------------------
# Use an even smaller image for the final runtime
FROM node:21-alpine

# Set working directory
WORKDIR /app

# Create and use a non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Copy only necessary files from the builder stage
COPY --from=backend-builder /app /app

# Copy environment configuration
COPY .env.docker .env

# Expose application port
EXPOSE 8080

# Define startup command
CMD ["npm", "start"]



