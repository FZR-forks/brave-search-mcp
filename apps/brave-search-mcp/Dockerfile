FROM node:23.11-alpine AS builder

WORKDIR /workspace

# Must copy the monorepo because workspace dependencies are required during build.
COPY . .

RUN corepack enable \
  && corepack pnpm install --frozen-lockfile \
  && corepack pnpm -C apps/brave-search-mcp run build

FROM node:23.11-alpine AS release

WORKDIR /app

COPY --from=builder /workspace/apps/brave-search-mcp/dist ./dist

ENV NODE_ENV=production

ENTRYPOINT ["node", "dist/index.js"]
