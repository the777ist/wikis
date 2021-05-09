# INSTALL MONGODB COMMUNITY EDITION IN WSL

Note: If you have already installed mongodb please remove all those before you install mongodb-org since it may cause some issues during installation

    sudo dpkg --remove --force-remove-reinstreq mongo-tools
    sudo dpkg --remove --force-remove-reinstreq mongodb-server-core
    sudo apt-get --fix-broken install


For installing mongodb community edition, I have added the commands below:

    wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
    
    sudo apt-get install gnupg
    
    wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
    
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list

    sudo apt-get update

    sudo apt-get install -y mongodb-org

Now, to get mongoDB running:

    sudo nano /etc/init.d/mongod

Paste the contents in this [link]() into the file and save it.

Give permissions

    sudo chmod +x /etc/init.d/mongod

Start the service:

    sudo service mongod start