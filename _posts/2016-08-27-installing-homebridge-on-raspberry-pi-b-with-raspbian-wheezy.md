---
layout: blog
category: blog
published: true
title: Installing homebridge on Raspberry Pi B+ with Raspbian Wheezy
---
## Getting started
After installing node on the RPi with [nvm](https://github.com/creationix/nvm), I tried installing [homebridge](https://github.com/nfarina/homebridge) but ran into some errors when trying to install the mdns dependency:
```sh
    gyp ERR! build error
    gyp ERR! stack Error: `make` failed with exit code: 2
    gyp ERR! stack     at ChildProcess.onExit (/home/pi/.nvm/versions/node/v6.4.0/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:276:23)
    gyp ERR! stack     at emitTwo (events.js:106:13)
    gyp ERR! stack     at ChildProcess.emit (events.js:191:7)
    gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:204:12)
    gyp ERR! System Linux 3.12.22+
    gyp ERR! command "/home/pi/.nvm/versions/node/v6.4.0/bin/node" "/home/pi/.nvm/versions/node/v6.4.0/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
    gyp ERR! cwd /home/pi/.nvm/versions/node/v6.4.0/lib/node_modules/homebridge/node_modules/mdns
    gyp ERR! node -v v6.4.0
    gyp ERR! node-gyp -v v3.3.1
    gyp ERR! not ok
    /home/pi/.nvm/versions/node/v6.4.0/lib
    └── (empty)

    npm ERR! Linux 3.12.22+
    npm ERR! argv "/home/pi/.nvm/versions/node/v6.4.0/bin/node" "/home/pi/.nvm/versions/node/v6.4.0/bin/npm" "install" "-g" "homebridge" "--unsafe-perm"
    npm ERR! node v6.4.0
    npm ERR! npm  v3.10.3
    npm ERR! code ELIFECYCLE
```

After a lot of googling, (esp [this](https://github.com/cflurin/homebridge-punt/wiki/Running-Homebridge-on-a-Raspberry-Pi) page), I found out the gcc g++ installed by default (4.6.3) was out of date, and I needed at least version 4.8 
Update that with 
`sudo apt-get install gcc-4.8 g++-4.8`

Then set the new default gcc version with 

	sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8
	sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 40 --slave /usr/bin/g++ g++ /usr/bin/g++-4.6

Check your version with `gcc -v`:

`gcc version 4.8.2 (Raspbian 4.8.2-21~rpi3rpi1)`

And retry 
`npm install -g homebridge` 
It should work now!