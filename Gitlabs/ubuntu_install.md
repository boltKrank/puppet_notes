##1. Install and configure the necessary dependencies

If you install Postfix to send email please select 'Internet Site' during setup. Instead of using Postfix you can also use Sendmail or configure a custom SMTP server and configure it as an SMTP server.

On Centos 6 and 7, the commands below will also open HTTP and SSH access in the system firewall.

```
sudo apt-get install curl openssh-server ca-certificates postfix
```

##2. Add the GitLab package server and install the package

```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
sudo apt-get install gitlab-ce
```

If you are not comfortable installing the repository through a piped script, you can find the entire script here and select and download the package manually and install using

```
curl -LJO https://packages.gitlab.com/gitlab/gitlab-ce/packages/ubuntu/xenial/gitlab-ce-XXX.deb/download
dpkg -i gitlab-ce-XXX.deb
```

##3. Configure and start GitLab

```
sudo gitlab-ctl reconfigure
```

##4. Browse to the hostname and login

On your first visit, you'll be redirected to a password reset screen to provide the password for the initial administrator account. Enter your desired password and you'll be redirected back to the login screen.

The default account's username is root. Provide the password you created earlier and login. After login you can change the username if you wish.
