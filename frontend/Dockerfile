#############################
######## APPLICATION ########
#############################


# Name the node stage "builder"
FROM node:15 AS builder

# Set working directory
WORKDIR /app

# Copy all files from current directory to working dir in image
COPY . .

# install node modules and build assets
RUN npm install
RUN npx npm-force-resolutions
RUN npm install
RUN npm run build


#############################
########## SERVER ###########
#############################


# nginx state for serving content
FROM nginx:alpine

# Nginx config
RUN rm -rf /etc/nginx/conf.d
COPY docker/nginx /etc/nginx

# Set working directory to nginx asset directory
WORKDIR /usr/share/nginx/html

# Remove default nginx static assets
RUN rm -rf ./*

# Copy static assets from builder stage
COPY --from=builder /app/build .

# Copy env bash file
COPY ./env.sh .
COPY .env .

# Convert to LF
RUN sed -i 's/\r$//' ./env.sh
RUN sed -i 's/\r$//' ./.env

# Add bash
RUN apk add --no-cache bash

# make env bash runable
RUN chmod +x env.sh

EXPOSE 80

CMD ["/bin/bash", "-c", "/usr/share/nginx/html/env.sh && nginx -g \"daemon off;\""]
