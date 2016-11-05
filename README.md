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
hexo clean  # Not sure why
hexo deploy
```


Trobleshooting
--------------

```
fatal: unable to access 'https://github.com/possatti/possatti.github.io/': Could not resolve host: github.com
```

Try again in a few minutes. Really. You will know it worked when it asks for your username and password.

```
Template render error: (unknown path) [Line 1, Column 249]
  expected variable end
```

The above happened to me when I tried to use an Angular 2 expression like this `{{ wow }}`. It seems you can't use do that [due its internal template sintax](https://hexo.io/docs/troubleshooting.html#Escape-Contents).
