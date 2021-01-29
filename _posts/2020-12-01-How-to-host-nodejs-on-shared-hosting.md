---
layout: post
title: How to host a NodeJS app on shared hosting plan (GoDaddy or other) - working as of Dec 2020
---

### Please make sure to check step 5 for additional details on this whole idea.

## Steps

### 1. **Get SSH access**  
You'll need the tool PuTTY in order to complete that step.  
First, make sure to login into your shared hosting provider, and find the superuser. Also get the IP from the cpanel side menu.  
In PuTTY, create a new session using the superuser credentials and the ip.  

### 2. **Install nvm**   
First thing in this step is to check ur hosting version (x32 or x64).
Node is only x64 after v10, so pick a good Node version.
Install node and nvm. One way to do it would be:  

`wget -qO- https://cdn.rawgit.com/creationix/nvm/master/install.sh | bash`  

More info on the nvm and node installation [here](https://gist.github.com/barbietunnie/81be24258cd6e2a85ac6).

### 3. **Config your .htaccess file**  
*If it doesnt exist yet, create it in the root directory.*
The contents should be:

```RewriteEngine On
DirectoryIndex disabled
RewriteRule ^$ http://127.0.0.1:3000/ [P,L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ http://127.0.0.1:3000/$1 [P,L]
```

### 4. **Start your app in the server shell**  
The regular way to do that would be:  

`node app.js &`  

However, we want this process to be constantly running and checking for failures on behalf of the hosting solution. In case there are some,
it needs to re-up the server ASAP. So, this command can be useful for one-time testing whether your configuration works or not.  
To be able to finalize our setup, run:  

`npm i pm2 -g`  *(installs the process manager)*  
`pm2 start index.js --watch` 

### 5. **Occasional checks for failures**  
So, this worked with Godaddy. It was up like this for well over a week, and then... Well, their servers just failed whatsoever, meaning that process manager we started and assigned the task to watch over our app failed as well. So, after a reset using the same steps, I was able to recover the working state of the app. But it got me thinking, how reliable is this, really?  
Since we're not using any tool to automate these restarts, the occassional manual work becomes mandatory. Maybe its just time to buy a dedicated server....
