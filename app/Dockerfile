# BASE IMAGE: Alpine Nginx version
FROM nginx:alpine

# METADATA
LABEL maintainer="rubenboopego@gmail.com"

# TASK: delete default Nginx web and use my index.html
COPY index.html /usr/share/nginx/html/index.html

# PORT
EXPOSE 80

