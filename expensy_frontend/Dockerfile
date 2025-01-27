# Use a lightweight Node.js image
FROM node:18-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application files
COPY . .

# Build the app for production
ARG NODE_ENV=production
RUN if [ "$NODE_ENV" = "production" ]; then npm run build; fi

# Expose the application port
EXPOSE 3000

# Default command
CMD ["npm", "run", "start"]
