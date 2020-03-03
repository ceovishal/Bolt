## Running commands

We can run against hosts without them being in the *inventory.yaml*

``` bash
bolt command run "whoami" --targets ssh://192.168.0.150 -u ansible -p ansible --no-host-key-check
```

We can use a jolly-joker called *all*

``` bash
bolt command run "whoami" --targets all
```

We create these two scripts, *my_script.sh* and *test.ps1*.

``` bash
#!/bin/bash

echo "This is running",$0,$1
```

``` bash
write-host "$psversiontable"
write-output "This is cooool!"
```

We execute the bash script with arguments this way.

``` bash
bolt run script my_script.sh "first argument" --targets linux
```

We execute the powershell script this way.

``` bash
bolt script run test.ps1  --targets windows
```

In order to upload files we need to make sure we provide the filename aswell. We also need to make sure the user has the appropriate acces level to drop the files.

``` bash
bolt file upload ./get-pip.py /home/ansible/test/get-pip.py --targets linux
```

For windows the path is a bit tricky.

``` bash
bolt file upload ./get-pip.py C:\\temp\\get-pip.py --targets windows
```