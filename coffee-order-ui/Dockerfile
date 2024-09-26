# Stage 1: Build the React app with the latest Node.js LTS version (18)
FROM node:18-alpine AS build-step

# Create the app directory
RUN mkdir /app
WORKDIR /app

# Install app dependencies
COPY package.json /app
RUN npm install

# Copy the app source code and build the app
COPY . /app
RUN npm run build

# Stage 2: Serve the app with Nginx
FROM nginx:1.17.1-alpine

# Copy the built app from the previous step to Nginx's HTML directory
COPY --from=build-step /app/build /usr/share/nginx/html

# Expose port 80 to the outside world
EXPOSE 80

# Remove default Nginx config and replace it with a custom one
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
