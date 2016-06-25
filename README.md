Blog
====

Install and run
---------------

```sh
# Ubuntu dependencies
sudo apt-get -y install nodejs
sudo apt-get -y install nodejs-legacy
sudo apt-get -y install npm

# Install dependencies
npm install
npm install hexo-cli -g

# Download and configure Hueman theme
git submodule init
git submodule update
cd themes/hueman
git checkout master
cd ../..
ln _config.hueman.yml themes/hueman/_config.yml

# Run server
hexo server

# Deploy when everything is Ok
hexo deploy
```
