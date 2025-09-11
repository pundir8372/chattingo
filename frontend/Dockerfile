FROM node:18-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build


FROM nginx:1.25-alpine AS runtime
COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
HEALTHCHECK --interval=30s CMD wget -q --spider http://localhost || exit 1
CMD ["nginx", "-g", "daemon off;"]

