```bash
#step 1: Update your system
sudo pacman -Syu

#step 2: Installing nginx and enabling.
sudo pacman -Sy nginx
sudo systemctl enable nginx

#step 3: Create a new directory /web/html/nginx-2420
mkdir -p web/html/nginx-2420

#step 4: Make changes to the nginx.config file
sudo vim /etc/nginx/sites-available/nginx-2420.conf
#add the following on the HTTP block. This step is important..
include /etc/nginx/sites-enabled/*;

#step 5: Configuring nginx 
sudo vim /etc/nginx/nginx.conf
#if vim is not availble, run the following command and retry this step.
sudo pacman -Sy vim 


#when we are inside the file, type/copy-paste the following:
server {
    listen 80;
    server_name #your ip address;

    location / {
        root /web/html/nginx-242;
        index index.html;
    }
}

#If an error occures such as Error 212, please run the following commands
sudo mkdir -p /etc/nginx/sites-available
sudo mkdir -p /etc/nginx/sites-enabled




#Enable the server block and create a symbolic link to sites-enabled directory
sudo ln -s /etc/nginx/sites-available/nginx-2420 /etc/nginx/sites-enabled/

#step 6: In this step, we will configure the html file we have that we created in step 3.
#create an index file inside nginx-2420
touch /web/html/nginx-2420/index.html

#NOTE: The root path and the index file name needs to be the same and exist as the configuration file for nginx.

#Vim the index.html file
sudo vim /web/html/nginx-2420/index.html
#paste the following
#=============================================
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        * {
            color: #db4b4b;
            background: #16161e;
        }
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>All your base are belong to us</h1>
</body>
</html>
```
```bash
#=============================================
#step 7: 
#Type in the IP address of your droplet on a browser, and you will see the following results.
```
