## Installing on windows.

You should grab the [MSI](https://downloads.puppet.com/windows/puppet6/puppet-bolt-x64-latest.msi?_ga=2.187246625.820453849.1583158162-704715941.1573508620) installer.

## Installing on CentOS

You need to issue the following commands given that you have sudo access.

``` bash
sudo rpm -Uvh https://yum.puppet.com/puppet-tools-release-el-7.noarch.rpm 
sudo yum install puppet-bolt -y 
```

## Installing on MacOS

You can grab the [dmg](https://downloads.puppet.com/mac/puppet6/10.14/x86_64/puppet-bolt-latest.dmg?_ga=2.114239502.820453849.1583158162-704715941.1573508620) file, then extract and install it. 

> After each case you need to make sure the *bolt* executable is in your PATH variable, if not add it. 

Remember, *bolt --help* is your friend!
