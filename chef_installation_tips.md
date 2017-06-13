## Install chef-server, chefdk, chefclient

- Download package from the below link:
```
https://downloads.chef.io/chef-server/
```

- Run the installer:
```
yum -Uvh <package_name>.rpm
dpkg -i <package_name>.deb
```
- Reconfigure the server package:
```
chef-server-ctl reconfigure
```

- Now create one adminstration account and one orginization:

### Create admin user:
```
chef-server-ctl user-create USER_NAME FIRST_NAME LAST_NAME EMAIL 'PASSWORD' --filename FILE_NAME
```

### Create organization:
```
chef-server-ctl org-create short_name 'full_organization_name' --association_user user_name --filename ORGANIZATION-validator.pem
```

## Install chef-manage

- Download package from below link:
```
https://downloads.chef.io/manage/2.4.4
```
- Install using dpkg:
```
sudo dpkg -i chef-manage_2.4.4-1_amd64.deb 
```

- After install reconfigure the chef-manage package:
```
sudo chef-manage-ctl reconfigure
```

- Bummer, didn't find any user to login, here's how to create one user for you to login.

Create user:

```
sudo chef-server-ctl user-create admin Admin admin admin anil.shrish@gmail.com 'password'
```

syntax:
```
$ chef-server-ctl user-create USER_NAME FIRST_NAME [MIDDLE_NAME] LAST_NAME EMAIL 'PASSWORD' (options)
```
Reference:

https://docs.chef.io/server_users.html


## Configure CHEFDK


Login to chef-manage portal:
```
https://10.0.3.176/organizations/<org-name>/getting_started
```

Download starterkit, it will have following directory structure:

```
.
├── .chef
│   ├── admin.pem
│   ├── client.rb
│   ├── knife.rb
│   ├── trusted_certs
│   │   └── chef-server.crt
│   └── validation.pem
├── cookbooks
│   ├── chefignore
│   └── starter
│       ├── attributes
│       │   └── default.rb
│       ├── files
│       │   └── default
│       │       └── sample.txt
│       ├── metadata.rb
│       ├── recipes
│       │   └── default.rb
│       └── templates
│           └── default
│               └── sample.erb
├── .gitignore
├── README.md
└── roles
    └── starter.rb

11 directories, 14 files
```

During node boostrap, node will communicate with chef-server using https so we need to pass self-signed certificate to node while bootstraping. So before that we will fetch ssl certificate to our chefdk setup

```
knife ssl fetch
```

## Node Bootstraping process


- Knife bootstrap node:

```
  knife bootstrap <node-ip> -x <user-name> -P <password> -N <node_name> --sudo
```


Download varlidator.pem from the chef-manage:

```
https://10.0.3.176/organizations/  --> choose org and then click on "Reset validation key" if you don't have it saved.
```

- now run knife configure which will create client.pem [ this is only required for the first time to validate chef-client with server ]
```
knife configure client ~/.chef  
```

This files are required for client validation:
```
/etc/chef/validation.pem
/etc/chef/client.rb
```


# Extras

Setting up Chef Development environment:

- After installation of ChefDK or chef-client, it is recommended to add environment variables:


```
echo 'eval "$(chef shell-init bash)"' >> ~/.bashrc
```




Bash Alias order of execution:

```
The order of execution of files is 

/etc/environment

/etc/profile

/etc/bash.bashrc

/home/<user>/.profile

/home/<user>/.bashrc
```



Command with sudo doesn't work since the env variable is reset when using sudo command, it is a security feature.

to include our cutom env directory, let's add it in the "secure_path" variable.

```
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin:/opt/chef/embedded/bin"
```


Check environment variable set for different users:

```
echo 'echo $PATH' | sh
```

With Sudo
```
echo 'echo $PATH' | sudo sh
```


Or, You can always run this way. 
```
sudo env "PATH=$PATH"  <command>
eval "$(chef shell-init bash)"
```

