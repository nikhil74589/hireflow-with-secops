# ---- Build stage ----
FROM node:22-alpine AS build

RUN apk update && apk upgrade --no-cache

WORKDIR /app

COPY package*.json ./

# Install ONLY production dependencies using npm ci for speed and security
RUN npm ci --only=production

COPY . .


# ---- Final runtime stage ----
FROM node:22-alpine

# Update OS packages and remove npm/npx CLI to reduce attack surface
RUN apk update && apk upgrade --no-cache && \
    rm -rf /usr/local/lib/node_modules/npm /usr/local/bin/npm /usr/local/bin/npx

WORKDIR /app

# Copy built app and dependencies from build stage
COPY --from=build --chown=node:node /app ./

# Switch to non-root user (Security Best Practice)
USER node

EXPOSE 3000

CMD ["node", "src/index.js"]