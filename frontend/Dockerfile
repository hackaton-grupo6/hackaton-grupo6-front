# build environment
FROM node:14-alpine as react-build

WORKDIR /app

COPY ./frontend/package.json /app

COPY ./frontend /app

RUN yarn
RUN yarn build

# server environment
FROM nginx:alpine

COPY ./frontend/nginx.conf /etc/nginx/conf.d/configfile.template

COPY --from=react-build /app/build /usr/share/nginx/html

ENV PORT 3000
ENV HOST 0.0.0.0
ENV BACKEND_URL localhost:8080
EXPOSE 3000
CMD sh -c "envsubst '\$PORT' < /etc/nginx/conf.d/configfile.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"
