gcloud config set project steady-shard-402507

gcloud compute instance-groups managed create my-mig --template my-vm-template-with-custom-image-and-commands-global --size 1

gcloud compute instance-groups list
# gcloud compute instance-groups managed describe my-mig

# gcloud compute instance-groups managed delete my-mig

gcloud compute instance-groups managed set-autoscaling my-mig --max-num-replicas 5

gcloud compute instance-groups managed resize my-mig --size 1

gcloud compute instance-groups managed recreate-instances my-mig --instances my-mig-s3c1

# Will not work as size and template are mandatory sub-options
gcloud compute instance-groups managed create my-mig --size 1

gcloud compute instance-groups managed create my-mig --template my-vm-template-with-custom-image-and-commands-global

gcloud compute instance-groups managed create my-mig set-autoscaling my-mig --max-num-replicas=2

*** CODES ***

#!/bin/bash
sudo su\
apt update\
apt install apache2 -y\
service apache2 start\

v1

echo -e "<html>\n<head>\n<style type=\"text/css\">\nbody{font-family: \"Segoe UI\", Arial, sans-serif;background-color: #123456;}\n.box {position: relative;margin: -5px auto;width: 200px;background-color: black;}\ntable{background-color:#DCDCDC}\th {color:#708090}\ntbody {color:#191970}\n</style>\n</head>\n<body>\n<div class=\"box\"><table border=\"1\">\n<thead>\n<tr width="100"\nbgcolor='#C0C0C0'><center><td colspan="5"><font color="#000000"><b>COMPLETE STATUS OF APPLICATION</b></center></td>\n</h4></tr><tr><th>APP NAME</th><th>HOSTNAME</th><th>IP</th><th>STATUS</th><th>VERSION</th></tr><tr><td>VM Instance v1</td><td>$(hostname)</td><td>$(hostname -I)</td><td>ON</td><td>v1</td></tr></table></div>" > /var/www/html/index.html

v2

echo -e "<html>\n<head>\n<style type=\"text/css\">\nbody{font-family: \"Segoe UI\", Arial, sans-serif;background-color: #654321;}\n.box {position: relative;margin: -5px auto;width: 200px;background-color: black;}\ntable{background-color:#DCDCDC}\th {color:#708090}\ntbody {color:#191970}\n</style>\n</head>\n<body>\n<div class=\"box\"><table border=\"1\">\n<thead>\n<tr width="100"\nbgcolor='#C0C0C0'><center><td colspan="5"><font color="#000000"><b>COMPLETE STATUS OF APPLICATION</b></center></td>\n</h4></tr><tr><th>APP NAME</th><th>HOSTNAME</th><th>IP</th><th>STATUS</th><th>VERSION</th></tr><tr><td>VM Instance v2</td><td>$(hostname)</td><td>$(hostname -I)</td><td>ON</td><td>v2</td></tr></table></div>" > /var/www/html/index.html
