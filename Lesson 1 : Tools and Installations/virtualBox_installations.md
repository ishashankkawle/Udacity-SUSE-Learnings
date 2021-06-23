# Tools, Environment & Dependencies

## Install VirtualBox

### First, add the key for the repository. You can download and add the key using this single command.

```
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
```

### Now add the Oracle VirtualBox repository to the list of repositories using this command:

According to your distribution, replace `'<mydist>'` with 'eoan', 'bionic', 'xenial', 'buster', 'stretch', or 'jessie' (older versions of VirtualBox supported different distributions):

```
sudo add-apt-repository "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian <mydist> contrib"
```

### Now that you have the correct repository added, refresh the list of packages available through these repositories and install VirtualBox.

```
sudo apt update && sudo apt install virtualbox-6.1
```

### Check version:

```
virtualbox --version
```

# Contributors:

1. Navin Khandeparkar : [LinkdIn](www.linkedin.com/in/navinkhandeparkar) [Github](https://github.com/Navinkhandeparkar)
