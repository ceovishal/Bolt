### Basic inventory

This is an inventory for linux and windows systems, make sure you either create it in the default project folder or in a folder which has no bolt.yaml in it otherwise you will have a hard time to run your commands.

``` bash
groups:
  - name: linux
    targets:
      - centosa
    config:
      ssh:
      host-key-check: false
  - name: windows
    targets:
      - 2019A
    config:
      transport: winrm
      winrm:
        user: Administrator
	password: Rupture16
	ssl: false
```

After this is done you can run the following commands against your windows or linux groups or machines.

``` bash
bolt command run "mkdir C:\temp" --targets windows
#
bolt command run "whoami" --targets windows
#
bolt command run "whoami" --targets linux
```
