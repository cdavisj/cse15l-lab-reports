# Remote Servers
A remote server is essentially just a computer located somewhere else, such as a work site. You can connect to such a server to do remote work from home using something called [ssh](https://en.wikipedia.org/wiki/Secure_Shell), standing for *secure shell*. Let us start with how to do this.

## Connecting to a server via ssh
Let it be known, this lesson is in the context of connecting to UCSD's ieng6 server as a student. 

### Setup
Firstly, if you are a Windows user, you will need to make sure that you have OpenSSH installed. To do this, open up the Windows PowerShell as an administrator. Running the following command to check if you have it.

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

If you don't have it installed, you will see something like this.
```
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Note, there are two separate components to OpenSSH needed, the first is the client, and the second is the server. You can run the following commands as needed depending on the above output.
```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Now you should be ready to connect!

### Connecting
Again, in Windows PowerShell, make sure you are an administrator. You can connect to your remote server via the command `ssh username@servername`. As a student, you can retrieve your username by looking yourself up [here](https://sdacs.ucsd.edu/~icc/index.php). As mentioned, we are going to be working with the ieng6 server, so the username will be `ieng6.ucsd.edu`. With this information, you can now get connected to the server by running the above command. Here is an example of what this should look like.
```PowerShell
ssh cs15lwi22___@ieng6.ucsd.edu
```
The underscores will be replaced by your unique number. Furthermore, depending on what year and quarter this is, you are going to likely have a different initial portion. Notice it is simply the course name followed by a shortened version of the quarter. In this case, it is Winter 2022.

Once you've run this command, you will be prompted to confirm that you want to connect, at which you can just say "yes". As long as this is a trusted server. You will then need to enter the password corresponding to your school login.

Congratulations, you are officially connected to the remote server! The next thing to do is try out some commands.

### Commands