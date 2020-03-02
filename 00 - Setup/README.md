### Setup Builder Node

Add the 80, 443, 6443, 22 port inbound rule on the NSG

To install Docker on Ubuntu 18.09 run the following command.

```
curl https://releases.rancher.com/install-docker/18.09.sh | sh
```

```
sudo usermod -aG docker ubuntu
```

 >Start and new session

```
ssh-keygen -b 2048 -t rsa -f \
/home/ubuntu/.ssh/id_rsa -N ""
cat /home/ubuntu/.ssh/id_rsa.pub \
>> /home/ubuntu/.ssh/authorized_keys
```