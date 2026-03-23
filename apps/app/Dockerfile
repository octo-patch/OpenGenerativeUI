FROM node:20-alpine AS builder

WORKDIR /app

RUN npm install -g pnpm@9

# Copy workspace config and lockfile
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml turbo.json ./
COPY apps/app/package.json apps/app/

# Install dependencies
RUN pnpm install --frozen-lockfile

# Copy app source
COPY apps/app apps/app

# Build the Next.js app
RUN pnpm --filter app build

# Production image
FROM node:20-alpine

WORKDIR /app

# Copy standalone build output
COPY --from=builder /app/apps/app/.next/standalone ./
COPY --from=builder /app/apps/app/.next/static ./apps/app/.next/static
COPY --from=builder /app/apps/app/public ./apps/app/public

ENV PORT=3000
ENV HOSTNAME=0.0.0.0
EXPOSE 3000

CMD ["node", "apps/app/server.js"]
