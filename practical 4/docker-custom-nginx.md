# Day 04 - Building Custom NGINX Image with Dockerfile

## Objective

- Create custom Docker image
- Understand Dockerfile instructions
- Replace default NGINX configuration
- Build and run custom image
- Debug build errors
- Understand image layers

---

## Project Structure

nginx-app/
│
├── index.html
├── default.conf
└── Dockerfile

---

## index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>Custom NGINX App</title>
</head>
<body>
    <h1>Welcome to My Custom NGINX App</h1>
</body>
</html>
default.conf
server {
    listen 80;
    server_name localhost;

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
COPY default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
Build Image
docker build -t custom-nginx:v1 .

Important:
Dockerfile must be named exactly "Dockerfile"
(not Dockerfile.txt)

Run Container
docker run -d -p 8080:80 --name nginx-container custom-nginx:v1

Access in browser:
http://localhost:8080

Key Learnings

Dockerfile defines custom image build steps

COPY adds files into image layers

EXPOSE documents container port

-p publishes port to host

Build context is current directory (.)

Windows may save Dockerfile as .txt (common mistake)

Custom images are reproducible and production-ready

Architecture

Host → Docker Build → Custom Image → Container → NGINX → index.html

Conclusion

Today I moved from running prebuilt images to building custom Docker images with Dockerfile.
This is foundational for real-world deployments.