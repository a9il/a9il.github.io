---
layout: post
title: Docker Snippet Command
---

Docker is like Virtual Machine but 'better'.

Dockerfile
```
FROM node:11-alpine

ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}

WORKDIR /src/

# Expose the app port
EXPOSE 1030

# Start the app
CMD npm start
```

run docker container from image with mount volume and expoe to local IP only in Windows 10 OS
```
docker run --name container_name  \
-v c:/Users/username/path_to_nodejs_app/:/src/ \
-p 192.186.100.10:91:1030 \
-d \
--restart always \
-e "NODE_ENV=production" \
node_11_alpine_dev
```

