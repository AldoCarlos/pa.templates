# Stage 1: Compile and Build angular codebase

# Use official node image as the base image
FROM node:16.14 as build

# Set the working directory
WORKDIR /usr/local/app

# Add the source code to app
COPY ./ /usr/local/app/

# Install all the dependencies
RUN npm install

# Generate the build of the application
RUN npm run build -- --configuration prod --base-href

# Stage 2: Serve app with nginx server

# Use official nginx image as the base image
FROM nginx:1.21.6

# Copy the build output to replace the default nginx contents.
COPY --from=build /usr/local/app/dist/Empresa-Apm-Angular /usr/share/nginx/html


# Expose port 80
EXPOSE 80

ENTRYPOINT sh -c "envsubst < /usr/share/nginx/html/assets/app-config-template.json \
 > /usr/share/nginx/html/assets/app-config.json && \
 nginx -g \"daemon off;\""


# Comando p/ teste local
# 1> docker build -t apm-angular:latest . 
# 2> docker run -d -p 8080:80 --env-file env.list apm-angular:latest
# 3> abrir no browser http://localhost:8080/