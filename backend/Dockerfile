FROM node:14-alpine3.12

# Set working directory
WORKDIR /app

# Copy only package files first for better caching
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the app code
COPY . .

# Expose the port
EXPOSE 4000

# Start the app
CMD ["node", "index.js"]
