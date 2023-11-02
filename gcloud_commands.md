gcloud config list
gcloud config configurations list
gcloud projects list

gcloud compute instances list
gcloud compute instance-templates list
# gcloud compute instance-groups list-instances
# gcloud compute instance-groups managed instance-configs list
# gcloud alloydb instances list
# gcloud app instances list
# gcloud bigtable instances list
# gcloud bms instances list
# gcloud compute instances list
# gcloud filestore instances list
# gcloud looker instances list
# gcloud memcache instances list

# --template and --size are mandatory for below command
gcloud compute instance-groups managed create my-mig --template my-instance-template-v1 --size 1

gcloud compute health-checks list
gcloud compute instance-groups managed describe my-mig

# --max-num-replicas is mandatory for below command
gcloud compute instance-groups managed set-autoscaling my-mig --max-num-replicas 3 --min-num-replicas 1
gcloud compute instance-groups managed stop-autoscaling my-mig

gcloud compute instance-groups managed recreate-instances my-mig --instances my-mig-zfj9
gcloud compute instance-groups managed update-instances my-mig --minimal-action restart --all-instances
gcloud compute instance-groups managed update-instances my-mig --minimal-action refresh --most-disruptive-allowed-action restart

gcloud compute instance-groups managed delete my-mig

# Running notes
gcloud compute instance-groups managed create my-inst-grp --template my-instance-template-v1 --size 1 --zone asia-south1-c
gcloud compute instance-groups managed set-autoscaling my-inst-grp --zone asia-south1-c --max-num-replicas 5 --min-num-replicas 2

gcloud compute instance-groups managed resize my-inst-grp --zone asia-south1-c --size 3

gcloud compute instance-groups managed delete my-inst-grp --zone asia-south1-c


# Instance creation using custom automation script using gcloud
gcloud compute instances create instance-v1 \
    --project=steady-shard-402507 \
    --zone=asia-south1-c \
    --machine-type=e2-micro \
    --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default \
    --metadata=^,@^startup-script=\#\!/bin/bash$'\n'sudo\ \
su$'\n'apt\ update$'\n'apt\ install\ apache2\ -y$'\n'service\ apache2\ start$'\n'echo\ -e\ \"\<html\>\\n\<head\>\\n\<style\ type=\\\"text/css\\\"\>\\nbody\{font-family:\ \\\"Segoe\ UI\\\",\ Arial,\ sans-serif\;background-color:\ \#123456\;\}\\n.box\ \{position:\ relative\;margin:\ -5px\ auto\;width:\ 200px\;background-color:\ black\;\}\\ntable\{background-color:\#DCDCDC\}\\th\ \{color:\#708090\}\\ntbody\ \{color:\#191970\}\\n\</style\>\\n\</head\>\\n\<body\>\\n\<div\ class=\\\"box\\\"\>\<table\ border=\\\"1\\\"\>\\n\<thead\>\\n\<tr\ width=\"100\"\\nbgcolor=\'\#C0C0C0\'\>\<center\>\<td\ colspan=\"5\"\>\<font\ color=\"\#000000\"\>\<b\>COMPLETE\ STATUS\ OF\ APPLICATION\</b\>\</center\>\</td\>\\n\</h4\>\</tr\>\<tr\>\<th\>APP\ NAME\</th\>\<th\>HOSTNAME\</th\>\<th\>IP\</th\>\<th\>STATUS\</th\>\<th\>VERSION\</th\>\</tr\>\<tr\>\<td\>VM\ Instance\ v1\</td\>\<td\>\$\(hostname\)\</td\>\<td\>\$\(hostname\ -I\)\</td\>\<td\>ON\</td\>\<td\>v1\</td\>\</tr\>\</table\>\</div\>\"\ \>\ /var/www/html/index.html$'\n' \
    --maintenance-policy=MIGRATE \
    --provisioning-model=STANDARD \
    --service-account=122875706440-compute@developer.gserviceaccount.com \
    --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \
    --tags=http-server \
	--create-disk=auto-delete=yes,boot=yes,device-name=instance-v1,image=projects/debian-cloud/global/images/debian-11-bullseye-v20231010,mode=rw,size=10,type=projects/steady-shard-402507/zones/us-west4-b/diskTypes/pd-balanced \
    --no-shielded-secure-boot \
    --shielded-vtpm \
    --shielded-integrity-monitoring \
    --labels=goog-ec-src=vm_add-gcloud \
    --reservation-affinity=any

# Image creation using gcloud
gcloud compute images create image-v1 \
    --project=steady-shard-402507 \
    --family=dev \
    --source-disk=instance-v1 \
    --source-disk-zone=asia-south1-c \
    --labels=env=dev \
    --storage-location=asia

# Instance template creation using gcloud
gcloud compute instance-templates create my-instance-template-v1 --project=steady-shard-402507 --machine-type=e2-micro --network-interface=network=default,network-tier=PREMIUM --metadata=^,@^startup-script=\#\!/bin/bash$'\n'sudo\ su$'\n'apt\ update$'\n'apt\ install\ apache2\ -y$'\n'service\ apache2\ start$'\n'$'\n'echo\ -e\ \"\\n\\n\<style\ type=\"text/css\"\>\\nbody\{font-family:\ \"Segoe\ UI\",\ Arial,\ sans-serif\;background-color:\ \#123456\;\}\\n.box\ \{position:\ relative\;margin:\ -5px\ auto\;width:\ 200px\;background-color:\ black\;\}\\ntable\{background-color:\#DCDCDC\}\\th\ \{color:\#708090\}\\ntbody\ \{color:\#191970\}\\n\</style\>\\n\\n\\n\<div\ class=\"box\"\>\<table\ border=\"1\"\>\\n\\n\<tr\ width=\"100\"\\nbgcolor=\'\#C0C0C0\'\> --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=122875706440-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --create-disk=auto-delete=yes,boot=yes,device-name=my-instance-template-v1,image=projects/debian-cloud/global/images/debian-11-bullseye-v20231010,mode=rw,size=10,type=pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --reservation-affinity=any

