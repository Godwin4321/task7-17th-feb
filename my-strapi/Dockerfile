# ---------- BUILD STAGE ----------
FROM node:18-alpine AS builder

WORKDIR /app

# Install build dependencies for sqlite
RUN apk add --no-cache \
    python3 \
    make \
    g++ \
    libc6-compat

COPY package*.json ./

RUN npm install --production

COPY . .

RUN npm run build

# ---------- RUNTIME STAGE ----------
FROM node:18-alpine

WORKDIR /app

# Install runtime dependency only
RUN apk add --no-cache libc6-compat

COPY --from=builder /app ./

ENV NODE_ENV=production

EXPOSE 1337

CMD ["npm", "run", "start"]