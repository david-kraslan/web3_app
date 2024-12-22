# Stage 1: BUILD
FROM node:18.20.4-alpine AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy the entire project into the container (excluding files in .dockerignore)
COPY . .

# Build the app
RUN npm run build

# Stage 2: RUN
FROM node:18.20.4-alpine AS run

# Set the working directory
WORKDIR /app

# Create the non-root user and group, install production dependencies and install 'serve'
RUN addgroup -S app_admins \
    && adduser -S app_admin -G app_admins \
    && npm install -g serve

# Copy the build output from the build stage
COPY --from=build /app/dist /app/dist

# Change ownership of the files to the non-root user
RUN chown -R app_admin:app_admins /app

# Switch to the non-root user
USER app_admin

# Expose port 3000
EXPOSE 3000

# Serve the application
CMD ["serve", "-s", "dist"]















