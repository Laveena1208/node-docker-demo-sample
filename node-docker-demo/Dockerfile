# Stage 1: Build Stage
FROM ubuntu:20.04 AS build

# Set environment variables to avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Install Node.js and build dependencies
RUN apt-get update && \
    apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_14.x | bash - && \
    apt-get install -y nodejs build-essential && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Create and set the working directory
WORKDIR /usr/src/app

# Copy package.json and package-lock.json files
COPY package*.json ./

# Install Node.js dependencies
RUN npm install

# Copy the application files
COPY . .

# Stage 2: Production Stage
FROM ubuntu:20.04 AS running

# Set the working directory in the production image
WORKDIR /usr/src/app

# Copy Node.js and npm from the build stage
COPY --from=build /usr/bin/node /usr/bin/node
COPY --from=build /usr/bin/npm /usr/bin/npm
COPY --from=build /usr/src/app ./

# Expose the application port
EXPOSE 3000

# Start the application
CMD ["node", "app.js"]
