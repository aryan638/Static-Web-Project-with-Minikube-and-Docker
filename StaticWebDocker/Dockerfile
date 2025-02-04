# Use an official Nginx image to serve the static content
FROM nginx:alpine

# Copy the web files into the Nginx server directory
COPY web /usr/share/nginx/html

# Expose port 80 for HTTP
EXPOSE 80

# Command to run Nginx (default)
CMD ["nginx", "-g", "daemon off;"]
