# ------------ STAGE 1: Install dependencies ------------

FROM node:20 as builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install all dependencies (including devDependencies)
RUN npm install

# Copy app source code
COPY . .

# ------------ STAGE 2: Create final runtime image ------------

FROM node:20-slim as production

# Set working directory
WORKDIR /app

# Copy only production dependencies from builder
COPY --from=builder /app/node_modules ./node_modules

# Copy app source code
COPY --from=builder /app .

# Expose port
EXPOSE 3000

# Start app
CMD ["node", "app.js"]