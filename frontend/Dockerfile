# Use the ultra-lightweight Alpine Linux version of Nginx
FROM nginx:alpine

# Remove default Nginx static assets
RUN rm -rf /usr/share/nginx/html/*

# Copy our custom Nginx proxy configuration
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy the pre-compiled Flutter web assets from the host into the container
COPY build/web/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
