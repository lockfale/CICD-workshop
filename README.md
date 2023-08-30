# CICD-workshop
CICD Goat setup


Since Acloudguru is used for training and this is training, we'll be using the following setup in their (Cloud Playground)[https://learn.acloud.guru/cloud-playground/cloud-servers]:

![Server setup](image.png)

**Note:** You can use the large setup if you want just a little more performance.

The instance will only last for 4 hours before being auto shut down, so either set it a day or two before the workshop (no more than a week before), or 20-30 minutes before the workshop starts. The server will then only be retained for two weeks.

```
sudo apt install -y python3-pip && pip3 install ansible
```

```
curl https://raw.githubusercontent.com/lockfale/CICD-workshop/main/ansible-setup.yml
```

```
sudo ansible-playbook ansible-setup.yml
```

Go to `http://<serverhostname>:8000`