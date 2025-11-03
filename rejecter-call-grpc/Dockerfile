# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
COPY tsconfig*.json ./
COPY nest-cli.json ./

RUN npm ci

COPY src/ ./src/
COPY logs.proto ./

RUN npm run build

FROM node:20-alpine AS production

WORKDIR /app

COPY package*.json ./

RUN npm ci --only=production

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/logs.proto ./
COPY --from=builder /app/node_modules ./node_modules

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nestjs -u 1001
RUN chown -R nestjs:nodejs /app
USER nestjs

EXPOSE 5002

CMD ["node", "dist/main"]