# Stage 1: Build the application
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the entire application
COPY . .

# Build the application (transpile TypeScript to JavaScript)
RUN npm run build

# Stage 2: Run the application
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy only necessary files from the builder stage
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./

# Install only production dependencies
RUN npm ci --production

# Expose the port your backend uses (e.g., 8706 based on your `.env`)
EXPOSE 8706

# Start the server
CMD ["node", "dist/server.js"]
