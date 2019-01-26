## Dockerfile 
``` 
FROM node as build-stage

RUN mkdir /usr/src/app
WORKDIR /usr/src/app

COPY package*.json /usr/src/app/
RUN npm install
RUN npm install -g @angular/cli@7.1.4

COPY ./ /usr/src/app

RUN ng build --prod

# nginx
FROM nginx:1.15

COPY --from=build-stage /usr/src/app/dist/[projectname] /usr/share/nginx/html

COPY ./nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

## nginx.conf  
```
server {
  listen 80;
  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
    try_files $uri $uri/ /index.html =404;
  }
}
```