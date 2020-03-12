## Metadata 

This guide will show you what metadata you can provide to your tasks, and plans.

The metadata file should have the same name as the task or plan with **.json** extension.

For example the metadata for the **deploy_app** will be called **deploy_app.json**.

Let's populate our json file with the following content.

``` bash
{
    "description": "This tasks will deploy the application",
    "supports_noop": true,
    "input_method": "stdin",
    "parameters": {
        "user": {
            "description": "The github user whos repo we would like to deploy.",
            "type": "String"
        },
        "repo": {
            "description": "The repository of the github user we would like to have deployed.",
            "type": "String"
        }
    }
}
```


If we save it in the same folder as the task. We can issue the **bolt task show application::deploy_app** command, and it will return this output.

``` bash
application::deploy_app - This tasks will deploy the application

USAGE:
bolt task run --targets <node-name> application::deploy_app user=<value> repo=<value> [--noop]

PARAMETERS:
- user: String
    The github user whos repo we would like to deploy.
- repo: String
    The repository of the github user we would like to have deployed.

MODULE:
C:/Users/dszabo/.puppetlabs/bolt/modules/application
```