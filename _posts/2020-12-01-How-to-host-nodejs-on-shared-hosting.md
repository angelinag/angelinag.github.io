---
layout: post
title: How to host a NodeJS app on shared hosting plan (GoDaddy or other) - working as of Dec 2020
---

#### This post is in the writing.....

1. Get SSH access
PuTTY
Use superuser (check cpanel)
Ip - cpanel

2. Install nvm
Check ur hosting version (x32 or x64)
Node only x64 after v10
Ensure nvm and node work
run commmand in root dir

wget -qO- https://cdn.rawgit.com/creationix/nvm/master/install.sh | bash

3. Config .htaccess
if it doesnt exist, create it
dir should be root of project (example here)

RewriteEngine On
DirectoryIndex disabled
RewriteRule ^$ http://127.0.0.1:3000/ [P,L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ http://127.0.0.1:3000/$1 [P,L]

4. node app.js &

5. npm i pm2 -g
pm2 start index.js --watch
