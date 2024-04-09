Tutorial on how to create a firewall, a backend to place binary systems, a new service files to run the backend, editting serber block and adding a proxy to the back end, and testing connection to your backend. 

STEP 1:
Ensure your systems are up to date. Run these Commands
```bash
sudo pacman -Syu 
```

STEP 2:
Obtain UFW to enable the firewall. Follow these sub-steps

    Step 2.1:
    Download the package UFW

    ```bash 
    sudo pacman -S ufw
    ```
    Step 2.2 
    Enable Service

    ```bash
    sudo systemctl enable --now ufw.service
    ```

    Step 2.3 
    Deny incoming and Allow outgoing

    ```bash 
    sudo ufw deny incoming
    sudo ufw allow outgoing
    ```

    Step 2.4 
    This allows IP address to ANY port of 22

    ```bash
    sudo allow from 203.0.113.0/24 to any port 22
    sudo ufw allow from 203.0.113.0/24 to any port 22
    sudo ufw deny from 203.1.113.4
    ```

    Step 2.5 
    Type all of these in your terminal to allow access. Don't worry about any warnings shown. Focus on it saying "Updates Changed" or any relating phrases.

    ```bash
    sudo ufw lmit ssh 
    sudo ufw allow ssh or sudo ufw allow SSH 
    sudo ufw allow HTTP
    ```

    Step 2.6
    Now we will reboot the system to apply the changes. 

    ```bash
    sudo reboot
    ```

    Your system/droplet will be restarted, wait for it to reactivate and then sign in again.

    Run the next following commands when they are back on. When status verbose is run, you are expected to see a list of ports that are currently connected to the server.

    ```bash
    sudo ufw enable
    sudo ufw status verbose
    ```    
STEP 3:
This is to set up a backend binary on your system

    Step 3.1 
    Transfering the backend-Binary to your server using SFTP
    Follow these steps. Do not do them all at once please.

    ```bash
    sftp  -i .ssh/do-key name@ServerIPAddress
    put hello-server #Make sure when you sftp, you are initialize it in the directory where hello-server is located.
    exit
    ```

    Step 3.2 
    Now move the path to the local bin directory.
    This requires a sudo command as it will be transferred to the root directory. 


    ```bash
    sudo mv hello-server /usr/local/bin/backend #You retain the name. "backend" just made sense because it IS a backend. 
    ```

    Step 3.3 
    Now we have to make the backend file executable. When executed, make sure to nano the file and enter the configuration.

    Making it Executable. 

    ```bash
    sudo chmod u+x /usr/local/bin/backend
    ```
    WARNING: DO NOT CAT IT.

    Now you will have to configure the file using VIM. Make sure you have Vim installed. 

    ```bash
    #Ensuring that VIM is installed
    sudo pacman -Syu vim

    #Now vim the service for backend.
    sudo vim /etc/systemd/system/backend.service
    ```

    Copy and paste the following into the service file.

    ```
    [Unit]
    Description=Backend Service
    After=network.target

    [Service]
    Type=Simple
    #This is where the backend file is located.
    ExecStart=/usr/local/bin/backend
    Restart=always

    [Install]
    WantedBy=multi-user.target
    ```

    Save and Exit the editor with :wq

    Step 3.4 
    Start and Enable the service. 

    ```bash
    sudo systemctl start backend
    sudo systemctl enable backend
    ```


STEP 4:
Adding a Reverse Proxy to Backend

    Step 4.1 
    Open the Configuration file for the Nginx Server Block and add the following CODE inside the SERVER block.

    ```bash
    sudo vim /etc/nginx/sites-available/nginx-2420
    ```

    And Copy and Paste this into the server block.
    You are going to add a site http:/127.0.0.1:8080

    ```
    location /hey {
        proxy_pass http://127.0.0.1:8080;
    }
    
    location /echo {
        proxy_pass http://127.0.0.1:8080;
    }
    ```

    Step 4.2 
    After doing this, type in the following steps
    

    ```bash
    sudo nginx -t  #Outcome is supposed to have all configuration file OK and test is SUCCESSFUL, if any error occurs, redo the steps.
    sudo systemctl restart nginx
    ```

STEP 5:
CURL COMMANDS
Curl commands are located in PowerSHell, not in Command Prompt. This is to TEST your server. 
Make sure you are testing this in your LOCAL MACHINE, NOT YOUR SERVER.

    Step 5.1 
    Type in the commands in PowerShell

    ```powershell
    curl http://"Your IP ADDRESS"/hey
    #Outcome should be Hey There

    curl -X POST -H "Content-Type: application/json" \
    -d '{"message": "Hello from your server"}' \
    http://"Your IP ADDRESS"/echo
    #outcome should be {"message": "Hello from your server"}

    ```

YOU have successfuly done this

