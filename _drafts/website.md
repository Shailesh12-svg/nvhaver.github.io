# Website

## Infrastructure

Raspberry Pi

Which SD card? Partitioning?
OS Partition
/var partition
/log partition

## Data Flow

Articles in MarkDown format (.md) are put on the server in a "to process" folder. By doing so, the incron triggers a script. The script uses pandoc to convert the article into an HTML5 partial. This partial is then pasted into the HTML5 article template. To clean up the formatting and to check for errors, the tidy command is run. Finally, the article page is put in the articles folder to be published on the website. The contents of this folder are served by Nginx as static pages.

## Configuration

### Dependencies

Are the following available on the RPI?

- pandoc
- incron
- nginx
- tidy

### Folder structure
```
/var/www/website/
├── articles
├── css
├── images
└── js
```
- /var/upload/templates
- /var/upload/templates/article_template.html
- /var/upload/articles

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
