### Projects

A default project directory lives like this.

``` bash
project/ 
├── Puppetfile 
├── bolt.yaml 
├── data
│   └── common.yaml
├── inventory.yaml
└── site-modules
    └── project
    ├── manifests
    │   └── my_class.pp
    ├── plans
    │   ├── deploy.pp
    │   └── diagnose.pp
    └── tasks
        ├── init.json
        └── init.py
```

We have two commands, one is for initialization of a new project, the other is for migrating from v1 to v2.

``` bash
bolt project init
#
bolt project migrate
```

> If there is a Boltdir and bolt.yaml, the Boltdir has precedence.


