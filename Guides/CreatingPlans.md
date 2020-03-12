### Creating plans

There might be cases when you are not able to complete a specific task but rather your would like to chain together either custom tasks or call tasks from plans itself.

You can retrieve the list of plans installed based on your modulepath with the following command **bolt plan show**.

You should use the previous folder structure where we defined our tasks.

``` bash
└───bolt
    └───modules
        └───application
            ├───plans
            └───tasks
```

You reference your plans the same way you reference your tasks **application::<plan>**

We would like to create an appdeploy plan with the **pp** extension under plans.

``` ruby
plan application::appdeploy(
  TargetSpec $targets,
  String     $user,
  String     $repo,

){
  run_task(
    'application::deploy_app',
    $targets,
    user => $user,
    repo => $repo,
  )
  return run_command("ls -l /home/ansible/apps",$targets)
}
```

Once this is done we can call the appdeploy plan with the following command.

``` bash
bolt plan run application::appdeploy user="r3ap3rpy" repo="vagrantseries" --targets=centosa
```

It should produce the following output.

``` bash
Starting: plan application::appdeploy
Starting: task application::deploy_app on centosa
Finished: task application::deploy_app with 0 failures in 6.48 sec
Starting: command 'ls -l /home/ansible/apps' on centosa
Finished: command 'ls -l /home/ansible/apps' with 0 failures in 1.89 sec
Finished: plan application::appdeploy in 8.38 sec
Finished on centosa:
  STDOUT:
    total 0
    drwxrwxr-x. 12 ansible ansible 230 Mar 12 13:02 vagrantseries
Successful on 1 target: centosa
Ran on 1 target
```

If we would like to we can convert the rest of the tasks into plan steps or build more complex plans.