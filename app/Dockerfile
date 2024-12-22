# Stage 1: BUILD
FROM node:18.20.4-alpine AS build

WORKDIR /app

# Copy the necessary files for npm install
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the app
RUN npm run build

# Stage 2: RUN
FROM node:18.20.4-alpine AS production

WORKDIR /app

# Create the non-root user and group
RUN addgroup -S app_admins && adduser -S app_admin -G app_admins

# Install production dependencies and install 'serve'
RUN npm install -g serve

# Copy the build output from the build stage
COPY --from=build /app/dist /app/dist

# Change ownership of the files to the non-root user
RUN chown -R app_admin:app_admins /app

# Switch to the non-root user
USER app_admin

# Expose the necessary port
EXPOSE 3000

# Serve the built application
CMD ["serve", "-s", "dist"]















