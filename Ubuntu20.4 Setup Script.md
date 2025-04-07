Ubuntu 20.4 setup script provided by Sophie (Voror) in Feb 2021

`create-endeavour-base-ami-U20.sh`
```bash
#!/bin/bash -x

###############################################
## Software installation and System tweaking ##
###############################################

## Set up proper locale (AWS Ubuntu has some problems with locale)
locale-gen en_US.UTF-8

#apt-get update && apt-get -yq upgrade
apt-get update && DEBIAN_FRONTEND=noninteractive apt-get upgrade -yq

## Install base necessities
apt-get -y install \
    htop \
    iotop \
    tcptrack \
    jq \
    unzip \
    ntp \
    collectd \
    openjdk-8-jre-headless \
    default-jre \
    fail2ban \
    unzip \
    net-tools

## Install AWS CLI
#pip install awscli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
./aws/install

####################
## Setup CollectD ##
####################
aws s3 cp s3://live-deployment-endeavour/CollectD/collectd.conf /etc/collectd/
service collectd restart

####################
##   Performance  ##
####################

## Set up kernel to avoid using SWAP on VMs (EBS is slow)
echo "vm.swappiness = 0" >> /etc/sysctl.conf

## Boost limit of open files (to support many concurrent connections)
echo "* soft    nofile  1000000" >> /etc/security/limits.conf
echo "* hard    nofile  1000000" >> /etc/security/limits.conf

## IMPORTANT! Apply limits for all types of sessions
for common_session in `ls /etc/pam.d/common-session*`; do echo "session required pam_limits.so" >> $common_session; done

###########################
## Setup Cloudwatch Logs ##
###########################
wget -O /home/ubuntu/amazon-cloudwatch-agent.deb https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

dpkg -i -E /home/ubuntu/amazon-cloudwatch-agent.deb

aws s3 cp s3://live-deployment-endeavour/cloudwatchlogs/file_amazon-cloudwatch-agent.json /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/amazon-cloudwatch-agent.json
chmod 755 /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/amazon-cloudwatch-agent.json

export AWS_DEFAULT_REGION=eu-west-2
INSTANCE_ID=$(ec2metadata --instance-id)
SHORT_NODE_NAME=$(aws ec2 describe-tags --filter "Name=resource-id,Values=$INSTANCE_ID" | jq -r '.Tags[] | select(.Key == "Name") | .Value')
HOSTNAME="$SHORT_NODE_NAME"
sed -i -e "s/HOSTNAME/$HOSTNAME/g" /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/amazon-cloudwatch-agent.json

/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.d/amazon-cloudwatch-agent.json -s

systemctl daemon-reload
systemctl enable amazon-cloudwatch-agent
systemctl start amazon-cloudwatch-agent

#echo "dns-search discoverydataservice.net" >> /etc/network/interfaces.d/eth0.cfg

###################
##  JAVA         ##
###################

# echo 'JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"' >> /etc/environment
echo 'JAVA_HOME="/usr/lib/jvm/java-11-openjdk-amd64"' >> /etc/environment
source /etc/environment
echo "$JAVA_HOME"
update-alternatives --set java /usr/lib/jvm/java-11-openjdk-amd64/bin/java
ln -s "$JAVA_HOME"/lib/security/cacerts /usr/lib/jvm/truststore
keytool -storepasswd -storepass changeit -new 8UmSntZxbty5xKyv -keystore /usr/lib/jvm/truststore

wget https://s3.amazonaws.com/rds-downloads/rds-ca-2019-root.pem
wget https://s3.amazonaws.com/rds-downloads/rds-ca-2019-eu-west-2.pem
keytool -import -trustcacerts -keystore /usr/lib/jvm/truststore -storepass 8UmSntZxbty5xKyv -noprompt -alias rds-ca-2019-root.pem -file rds-ca-2019-root.pem
keytool -import -trustcacerts -keystore /usr/lib/jvm/truststore -storepass 8UmSntZxbty5xKyv -noprompt -alias rds-ca-2019-eu-west-2.pem -file rds-ca-2019-eu-west-2.pem

#############################
## Auto Clean Old Packages ##
#############################

echo '// Custom DDS settings' >> /etc/apt/apt.conf.d/50unattended-upgrades
echo 'Unattended-Upgrade::Remove-Unused-Dependencies "true";' >> /etc/apt/apt.conf.d/50unattended-upgrades

##############
## Security ##
##############

#Shared Memory Security
echo "tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0" >> /etc/fstab

#SSH HARDENING
#sed -e "/HostKey /etc/ssh/ssh_host_dsa_key/c\#HostKey /etc/ssh/ssh_host_dsa_key" \
#   -e "/HostKey /etc/ssh/ssh_host_ecdsa_key/c\#HostKey /etc/ssh/ssh_host_ecdsa_key" \
#   -i /etc/ssh/sshd_config
echo "ClientAliveInterval 1800" >> /etc/ssh/sshd_config
echo "ClientAliveCountMax 2" >> /etc/ssh/sshd_config
echo "KexAlgorithms curve25519-sha256@libssh.org" >> /etc/ssh/sshd_config
echo "Ciphers aes256-ctr" >> /etc/ssh/sshd_config
echo "MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-256" >> /etc/ssh/sshd_config

#Failed Login Banning
echo "##To block the failed login attempts on the SSH server, use the below jail." >> /etc/fail2ban/jail.local
echo "[sshd]" >> /etc/fail2ban/jail.local
echo "enabled = true" >> /etc/fail2ban/jail.local
echo "banaction = iptables-multiport" >> /etc/fail2ban/jail.local
echo "maxretry = 5" >> /etc/fail2ban/jail.local
echo "findtime = 43200" >> /etc/fail2ban/jail.local
echo "bantime = 86400" >> /etc/fail2ban/jail.local

systemctl enable fail2ban
systemctl restart fail2ban

################
## Networking ##
################

echo "net.core.somaxconn = 40000" >> /etc/sysctl.conf
echo "net.core.wmem_default = 8388608" >> /etc/sysctl.conf
echo "net.core.rmem_default = 8388608" >> /etc/sysctl.conf
echo "net.ipv4.tcp_keepalive_intvl = 30" >> /etc/sysctl.conf
echo "net.ipv4.tcp_max_syn_backlog = 40000" >> /etc/sysctl.conf
echo "net.ipv4.tcp_fin_timeout = 15" >> /etc/sysctl.conf
echo "net.ipv4.tcp_tw_reuse = 1" >> /etc/sysctl.conf
echo "vm.dirty_background_ratio = 0" >> /etc/sysctl.conf
echo "vm.dirty_background_bytes = 209715200" >> /etc/sysctl.conf
echo "vm.dirty_ratio = 40" >> /etc/sysctl.conf
echo "vm.dirty_writeback_centisecs = 100" >> /etc/sysctl.conf
echo "vm.dirty_expire_centisecs = 200" >> /etc/sysctl.conf

echo "search discoverydataservice.net" >> /etc/resolv.conf

###############################################
## Prepare AMI image of the DDS base image   ##
###############################################

export AWS_DEFAULT_REGION=eu-west-2

DATE_NOW=$(date +"%Y-%m-%d_%H-%M-%S")
LINUX_DISTRIBUTION_DESCRIPTION=$(lsb_release -ds)

ENDEAVOUR_BASE_AMI_NAME="DDS-Ubuntu20-Hardened-$DATE_NOW"
THIS_INSTANCE_ID=$(ec2metadata --instance-id)

CREATE_AMI_OUTPUT=\
$(aws ec2 create-image \
    --instance-id "$THIS_INSTANCE_ID" \
    --name "$ENDEAVOUR_BASE_AMI_NAME" \
    --description "Endeavour $LINUX_DISTRIBUTION_DESCRIPTION")

echo "Output from AWS: $CREATE_AMI_OUTPUT"
```