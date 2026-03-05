FROM node:23.11-alpine AS builder

WORKDIR /workspace

# Must copy the monorepo because workspace dependencies are required during build.
COPY . .

RUN corepack enable
RUN corepack pnpm install --frozen-lockfile
RUN corepack pnpm -C apps/brave-search-mcp run build
# --legacy is required because this workspace does not use inject-workspace-packages.
RUN corepack pnpm deploy --filter brave-search-mcp --prod --legacy /workspace/.deploy

FROM node:23.11-alpine AS release

WORKDIR /app

COPY --from=builder /workspace/.deploy/ ./

ENV NODE_ENV=production

ENTRYPOINT ["node", "dist/index.js"]
