# SSL/TLS_certification

## Nginx Enable HTTPS

> HTTPS is a secure version of HTTP that encrypts data between your browser and a website, making it safe from hackers. 
It uses SSL/TLS to protect sensitive information like passwords and credit card details

**Steps**
- For Mac
    - brew install certbot
- For Windows
    - choco install certbot -y
- For Linux
    - Centos
        - sudo yum install epel-release -y
        - sudo yum install certbot python3-certbot-nginx -y 
    - Ubuntu
        - sudo apt install certbot python3-certbot-nginx -y

- First stop the nginx (if listening on port 80) as below command will use local port 80.

- To generate certificates
    - sudo certbot certonly --standalone -d yourdomain.com

- Files will be generated in
    - /etc/letsencrypt/live/yourdomain.com/

      - Certificate is saved at: /etc/letsencrypt/live/stackdev.live/fullchain.pem
      - Key is saved at:         /etc/letsencrypt/live/stackdev.live/privkey.pem
     
        - vi /etc/nginx/nginx.conf

> listen       443 ssl http2;

> ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;

> ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

> ssl_protocols TLSv1.2 TLSv1.3;

**EX:**
```
events {}

http {
    include mime.types;

    server {
        listen 8800;
        server_name stackdev.live www.stackdev.live;

        root /usr/share/nginx/html/demo;
        index index.html;

        error_page 404 /404.html;

        location / {
            try_files $uri $uri/ =404;
        }

        access_log /var/log/nginx/stackdev.live.access.log;
        error_log /var/log/nginx/stackdev.live.error.log;
    }

    server {
        listen 443 ssl;
        server_name stackdev.live www.stackdev.live;

        root /usr/share/nginx/html/demo2;
        index index.html;

        ssl_certificate /etc/letsencrypt/live/stackdev.live/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/stackdev.live/privkey.pem;
        ssl_protocols TLSv1.2 TLSv1.3;

        error_page 404 /404.html;

        location / {
            try_files $uri $uri/ =404;
        }

        access_log /var/log/nginx/stackdev.live.access.log;
        error_log /var/log/nginx/stackdev.live.error.log;
    }
}

```  

> check your website is ssl/tls certified **https://www.cdn77.com/tls-test**

