# Step 1: Build Angular App
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build

# Step 2: Nginx to serve built Angular app
FROM nginx:alpine

COPY --from=builder /app/dist/angular-15-crud/ /usr/share/nginx/html



EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
