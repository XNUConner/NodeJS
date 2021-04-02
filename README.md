# NodeJS
Notes taken and code written while learning NodeJS

## Installation Process (linux-amd64)
```
wget https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz
sudo mkdir /usr/local/lib/node
tar -xvf node-v14.16.0-linux-x64.tar.xz
mv node-v14.16.0-linux-x64/ nodejs/
sudo mv nodejs/ /usr/local/lib/node/

echo NODEJS_HOME=/usr/local/lib/node/nodejs >> ~/.bashrc
echo PATH=$NODEJS_HOME/bin:$PATH

source ~/.bashrc 
```
