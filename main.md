CHEF essentials commands:

=========================================================================================
# Knife Commands:

```knife --version  # get knife version```

#### Node:
```
  knife node list  # list all nodes

  knife node show <node_name>  # show node information

  knife node edit <node_name>

  knife node delete <node_name>
```

#### Databag:
```
  knife data bag list  # show databag list

  knife data bag show <data_bag_name>  # show content of databag

  knife data bag show <data_bag_name> <variable> # show content of the databag
```
Example: users databag:
```
   knife data bag create users anilshrish
   knife data bag delete users anilshrish
```
#### Cookbook:
```
  knife cookbook create <cookbookName>

  knife cookbook list
```
## Bootstraping a Node
```
  knife bootstrap <node-ip/hostname> -x <user-name> -P <password> -N <node_name> --sudo

  knife bootsrap <nodeip/hostname> -x root -P <password> -N <node_name> -r "receipe[apache]"       # Bootstrap with recipie
```

#### Role:
```
  knife role create <role_name>  # Create role
  knife role delete <role_name>  # delete role
  knife role edit <role_name>    # edit role
  knife role show <role_name>    # show role
```

#### Extras:

Rackspace integration:
```
  knife rackspace image list  # show images present for your account
```
use databag in recipie:
```
  Example:  vdc_databag = Chef::DataBagItem.load('vdc', vdc)
```
Run selected cookbook,recipie in client:
```
  sudo chef-client -r mt_users::access
```
=========================================================================================
