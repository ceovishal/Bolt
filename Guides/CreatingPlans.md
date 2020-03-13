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

A very basic plan looks like this.

``` ruby
plan application::hello(
        TargetSpec $targets,
){
        run_command("uptime",$targets)
        run_command("whoami",$targets)
        return run_command("pwd", $targets)

}
```


We would like to create an appdeploy plan with the **pp** extension under plans.

``` ruby
plan application::hello(
        TargetSpec $targets,

        String $user,
        String $repo,
){
        run_command("mkdir -p /home/ansible/apps",$targets)
        run_command("cd /home/ansible/apps && git clone https://github.com/$user/$repo",$targets)
        return run_command("ls -l /home/ansible/apps/$repo", $targets)

}
```

Once this is done we can call the appdeploy plan with the following command.

``` bash
bolt plan run application::appdeploy user="r3ap3rpy" repo="vagrantseries" --targets=centosa
```

It should produce the following output.

``` bash
Starting: plan application::appdeploy                                                                                                
Starting: command 'mkdir -p /home/ansible/apps' on centosa                                                                           
Finished: command 'mkdir -p /home/ansible/apps' with 0 failures in 0.4 sec                                                           
Starting: command 'cd /home/ansible/apps && git clone https://github.com/r3ap3rpy/vagrantseries' on centosa                          
Finished: command 'cd /home/ansible/apps && git clone https://github.com/r3ap3rpy/vagrantseries' with 0 failures in 1.54 sec         
Starting: command 'ls -l /home/ansible/apps/vagrantseries' on centosa                                                                
Finished: command 'ls -l /home/ansible/apps/vagrantseries' with 0 failures in 0.33 sec                                               
Finished: plan application::appdeploy in 2.27 sec                                                                                    
Finished on centosa:                                                                                                                 
  STDOUT:                                                                                                                            
    total 8                                                                                                                          
    drwxrwxr-x. 2 ansible ansible   41 Mar 13 19:32 arguments                                                                        
    drwxrwxr-x. 3 ansible ansible   57 Mar 13 19:32 chefmodule                                                                       
    -rw-rw-r--. 1 ansible ansible   26 Mar 13 19:32 _config.yml                                                                      
    drwxrwxr-x. 7 ansible ansible   93 Mar 13 19:32 custommachines                                                                   
    drwxrwxr-x. 2 ansible ansible   25 Mar 13 19:32 helloworld                                                                       
    drwxrwxr-x. 2 ansible ansible   48 Mar 13 19:32 multimachine                                                                     
    drwxrwxr-x. 3 ansible ansible   66 Mar 13 19:32 puppetmodule                                                                     
    -rw-rw-r--. 1 ansible ansible 1672 Mar 13 19:32 README.md                                                                        
    drwxrwxr-x. 2 ansible ansible   45 Mar 13 19:32 shellmodule                                                                      
    drwxrwxr-x. 2 ansible ansible   83 Mar 13 19:32 vagrantboxformat                                                                 
    drwxrwxr-x. 2 ansible ansible   25 Mar 13 19:32 vboxdebian                                                                       
Successful on 1 target: centosa                                                                                                      
Ran on 1 target                                                                                                                      
```

If we would like to we can convert the rest of the tasks into plan steps or build more complex plans.

Let's call a task from our plan something that undeploy the app.

``` ruby
plan application::appundeploy(
        TargetSpec $targets,
        String $repo,
){
        run_task(
                'application::undeploy_app',
                $targets,
                repo => $repo
)
}
```

The outut should look something like this.

``` bash
Starting: plan application::appundeploy
Starting: task application::undeploy_app on centosa
Finished: task application::undeploy_app with 0 failures in 0.62 sec
Finished: plan application::appundeploy in 0.63 sec
Plan completed successfully with no result
```