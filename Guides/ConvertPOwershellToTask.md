### Powershell script to task

This guide will show you how you can convert your powershell scripts to bolt tasks that you can reuse.

First we create our script called **content.ps1** and place it under the **tasks** folder. It will take three mandatory parameters and basically it will create a folder with a specific file and set it's content as desired.

This script should look like this. 

``` powershell
[CMDLetbinding()]
param(
    [Parameter(Mandatory = $true, Position = 1)]
    [System.String]$Folder,
    [Parameter(Mandatory = $true, Position = 2)]
    [System.String]$Name,
    [Parameter(Mandatory = $true, Position = 3)]
    [System.String]$Message
)
BEGIN{
if(-not(Test-Path -Path $Folder))
    {
        Write-Output "Creating folder, becuse it does not exist!"
        New-Item -Path $Folder -ItemType Directory -Force 
    }
else
    {
        Write-Output "Folder is present, setting content!"
    }
}
PROCESS{
    Set-Content -Value $Message -Path ($Folder + "\" + $Name) -Verbose
}
END{
    Write-Output "The end!"
    exit 0
}
```

Then we create our **content.json** task metadata, this will define a nice help to our task.

``` JSON
{
        "description" : "This script is going to create a file in a given folder with a specific folder!",
        "supports_noop" : true,
        "input_method": "powershell",
        "parameters": {
                "folder" : {
                        "description" : "The folder to use!",
                        "type" : "String"
                        },
                "name" : {
                        "description" : "The name of the file!",
                        "type" : "String"
                },
                "message" : {
                        "description" : "The message we want in our file!",
                        "type" : "String"
                }
}
}
```

We can now execute the **bolt task show application::content** command which will produce the following output.

``` bash
application::content - This script is going to create a file in a given folder with a specific folder!

USAGE:
bolt task run --targets <node-name> application::content folder=<value> name=<value> message=<value> [--noop]

PARAMETERS:
- folder: String
    The folder to use!
- name: String
    The name of the file!
- message: String
    The message we want in our file!

MODULE:
C:/Users/dszabo/.puppetlabs/bolt/modules/application
```

Upon executing the script we can see the following ouptut.

The command.

``` bash
bolt task run application::content folder="C:\temp\bolt" name="test" message="Whatever" --targets 2019A
```

The output.

``` bash
Started on 2019A...
Finished on 2019A:
  Creating folder, becuse it does not exist!


      Directory: C:\temp


  Mode                LastWriteTime         Length Name

  ----                -------------         ------ ----

  d-----        3/15/2020   5:45 AM                bolt

  Verbose: Performing the operation "Set Content" on target "Path: C:\temp\bolt\test".
  The end!


Successful on 1 target: 2019A
Ran on 1 target in 2.53 sec
```