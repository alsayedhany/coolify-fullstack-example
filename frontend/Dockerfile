# Build stage
FROM node:18-alpine AS build
WORKDIR /app

# انسخ package.json و package-lock.json لو موجود
COPY package.json ./
COPY package-lock.json ./ || true

# ثبت الاعتماديات
RUN if [ -f package-lock.json ]; then npm ci; else npm install; fi

# انسخ باقي الملفات وابني المشروع
COPY . .
RUN npm run build

# Serve stage
FROM nginx:stable-alpine
COPY --from=build /app/dist /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
