# Step 1: Build Angular App
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .
RUN npm run build --prod

# Step 2: Serve with Nginx
FROM nginx:alpine

COPY --from=builder /app/dist/crud-dd-task-mean-app/ /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
