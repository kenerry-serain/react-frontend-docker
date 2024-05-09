FROM node:18-alpine3.17 as build
WORKDIR /nsse-frontend
COPY package*.json ./
RUN npm ci --silent --only=production
COPY . .
RUN npm run build

FROM nginx:1.25.2-alpine
COPY --from=build /nsse-frontend/build /usr/share/nginx/html
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/nginx.conf
HEALTHCHECK CMD curl --fail http://localhost || exit 1

EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]