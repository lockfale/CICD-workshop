# CICD-workshop
CICD Goat setup


Since Acloudguru is used for training and this is training, we'll be using the following setup in their [Cloud Playground](https://learn.acloud.guru/cloud-playground/cloud-servers).

Go to: https://learn.acloud.guru/cloud-playground/cloud-servers

Click New server and use the following options

![Server Setup](https://github.com/lockfale/CICD-workshop/assets/913856/ed3224ac-8c20-406a-ad16-ab50efdad361)

The instance will only last for 4 hours before being auto shut down, so either set it a day or two before the workshop (no more than a week before), or 20-30 minutes before the workshop starts. The server will then only be retained for two weeks.

Once the server is ready, expand the details on the right side on how you can login, you can connect through the Terminal interface, or if logging in off network you can ssh directly in.  The first time logging in, you will be asked to verify the current password once more and change the password. For this lab, it can be simple and easy to remember since security of the instance for the short period of the workshop isn't a high priority and you will need to use it often for sudo commands.

Log in again after changing your password and run the following commands.

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
