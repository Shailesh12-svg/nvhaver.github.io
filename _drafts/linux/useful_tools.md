### Nginx

Copy the configuration to working directory.
Check configuration using `nginx -c config -t`
After copying the configuration back, it can be loaded into nginx: `nginx -s reload`

/usr/local/nginx/conf/nginx.conf or /etc/nginx

### incron

sudo apt install incron
sudo echo nvhaver >> /etc/incron.allow
incrontab -e
/var/upload/articles IN_CREATE mdtohtml -c article -o /var/www/website/articles $@/$#

##  Scripts

The script that incron calls can be found in the development folder.
