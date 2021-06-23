# Tools, Environment & Dependencies

## Install Vagrant

```
$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
$ sudo apt-get update && sudo apt-get install vagrant
```

## Check version:

```
$ vagrant --version
```

Typing `vagrant` from the command line will display a list of all available commands.
Be sure that you are in the same directory as the Vagrantfile when running these commands!

## Creating a VM

`vagrant init` -- Initialize Vagrant with a Vagrantfile and ./.vagrant directory, using no specified base image. Before you can do vagrant up, you'll need to specify a base image in the Vagrantfile.

## Starting a VM

- `vagrant up` -- starts vagrant environment (also provisions only on the FIRST vagrant up)

## Getting into a VM

- `vagrant ssh` -- connects to machine via SSH

## Stopping a VM

- `vagrant halt` -- stops the vagrant machine

# Contributors:

1. Navin Khandeparkar : [LinkdIn](www.linkedin.com/in/navinkhandeparkar) [Github](https://github.com/Navinkhandeparkar)
