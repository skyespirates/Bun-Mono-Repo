# Stage 1: Builder
FROM oven/bun:1 AS builder
WORKDIR /app

# Copy seluruh proyek termasuk semua packages dan apps
COPY . .

# Install dependencies
RUN bun install

# Build semua packages (misalnya `bun build` digunakan untuk membuild package)
RUN bun run build --filter=@bun/utils
RUN cd apps/fastify-service && bun run build

# Stage 2: Runner (Production)
FROM oven/bun:1-slim AS runner
WORKDIR /app

# Copy hasil build dari packages dan fastify-service
COPY --from=builder /app/packages/bun.utils/dist /app/node_modules/@bun/utils
COPY --from=builder /app/apps/fastify-service/dist ./apps/fastify-service/dist
COPY --from=builder /app/apps/fastify-service/package.json ./apps/fastify-service/

# Set up environment
ENV PORT=8004
EXPOSE 8004
CMD ["bun", "apps/fastify-service/dist/index.js"]
