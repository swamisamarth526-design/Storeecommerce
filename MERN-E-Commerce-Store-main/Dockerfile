# syntax=docker/dockerfile:1
FROM node:20-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm install

COPY frontend/package*.json frontend/
RUN npm install --prefix frontend

COPY . .
RUN npm run build --prefix frontend

FROM node:20-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production

COPY package*.json ./
COPY package-lock.json ./
RUN npm install --production

COPY --from=builder /app/backend ./backend
COPY --from=builder /app/frontend/dist ./frontend/dist
COPY --from=builder /app/uploads ./uploads

EXPOSE 5000
CMD ["node", "backend/index.js"]
