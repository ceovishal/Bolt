## Creating tasks

There might be cases when the ad-hoc command do not satisfy your need for automation and you want to collect them into tasks which you can use to run.

You can retrieve the module locations where bolt is looking for tasks to execute with the following command **bolt task show**.

You should create the following folder structure the define tasks.

``` bash
└───bolt
    └───modules
        └───application
            └───tasks
```

After this you can specify the tasks which need an interpreter to run.

You can reference your tasks the following way **application::<taskname>**. 

Our assumption is that a specific gitrepo is the application we either want to update, deploy or undeploy.

Let's define three tasks.

### deploy_app

``` bash
#!/usr/bin/bash
mkdir -p /home/ansible/apps/
rm -rf /home/ansible/apps/$PT_repo
cd /home/ansible/apps/
git clone https://github.com/$PT_user/$PT_repo
```

### undeploy_app

``` bash
#!/usr/bin/bash
rm -rf /home/ansible/apps/$PT_repo
```

### update_app

``` bash
#!/usr/bin/bash
cd /home/ansible/apps/$PT_repo
git pull
```

You may have noticed the **$PT_repo** variable. Every argument passed to tasks manifests as an environment variable, and get's the **$PT_** prefix.


Now if we were to deploy the app we can use the following command.

``` bash
bolt task run application::deploy_app repo="<reponame>" user="gituser" --targets centosa
```

The output would look like this.

``` bash
λ bolt task run application::deploy_app repo="vagrantseries" user="r3ap3rpy" --targets centosa
Started on centosa...
Finished on centosa:
  Cloning into 'vagrantseries'...
Successful on 1 target: centosa
Ran on 1 target in 6.67 sec
```

We can update and undeploy the app with the following commands.

``` bash
#update
bolt task run application::deploy_app repo="vagrantseries" --targets centosa
#undeploy
bolt task run application::deploy_app repo="vagrantseries" --targets centosa
```

Note how the **user** argument is not needed anymore.