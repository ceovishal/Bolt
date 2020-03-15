### Powershell script to task

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