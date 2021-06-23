# Tools, Environment & Dependencies

## Go Install
[Official Documentation](https://golang.org/doc/install)

### Resource
Before installing we need prepare Go installation file. You can download it here, please choose based on Computer Operating System:

**go version 1.16.5**
|OS|Link|
|---|---|
|Windows|https://golang.org/dl/go1.16.5.windows-amd64.msi|
|Linux|https://golang.org/dl/go1.16.5.linux-amd64.tar.gz|
|Mac|https://golang.org/dl/go1.16.5.darwin-amd64.pkg|

to download another version you can check it here https://golang.org/dl/

---

### Installation for Windows
1. Open the MSI file you downloaded and follow the prompts to install Go. 
   > By default, the installer will install Go to Program Files or Program Files (x86). You can change the location as needed. After installing, you will need to close and reopen any open command prompts so that changes to the environment made by the installer are reflected at the command prompt. 
2. Verify that you've installed Go. 
   1. In Windows, click the Start menu. 
   2. In the menu's search box, type cmd, then press the Enter key. 
   3. In the Command Prompt window that appears, type the following command:
        ```sh
        $ go version
        ```
   4. Confirm that the command prints the installed version of Go.
   
---

### Installation for Linux
1. Extract the archive you downloaded into /usr/local, creating a Go tree in /usr/local/go. 
   >  **Important:** This step will remove a previous installation at /usr/local/go, if any, prior to extracting. Please back up any data before proceeding.

   > For example, run the following as root or through sudo: 
   ```sh
   rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.5.linux-amd64.tar.gz
   ```
2. Add /usr/local/go/bin to the PATH environment variable.
   > You can do this by adding the following line to your $HOME/.profile or /etc/profile (for a system-wide installation): 
   ```sh
    export PATH=$PATH:/usr/local/go/bin
   ```
   > **Note:** Changes made to a profile file may not apply until the next time you log into your computer. To apply the changes immediately, just run the shell commands directly or execute them from the profile using a command such as source $HOME/.profile. 
3. Verify that you've installed Go by opening a command prompt and typing the following command: 
   ```sh
   $ go version
   ```
4. Confirm that the command prints the installed version of Go.
   
----

### Installation for Mac
1.  Open the package file you downloaded and follow the prompts to install Go. 
    > The package installs the Go distribution to /usr/local/go. The package should put the /usr/local/go/bin directory in your PATH environment variable. You may need to restart any open Terminal sessions for the change to take effect. 
2.   Verify that you've installed Go by opening a command prompt and typing the following command: 
        ```sh
        $ go version
        ```
3.   Confirm that the command prints the installed version of Go.


# Contributors

1. Purna Satria Nugraha : [LinkedIn](www.linkedin.com/in/purnasatria) [Github](https://github.com/repoerna)
