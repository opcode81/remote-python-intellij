# Remote Python Interpreter Configuration in IntelliJ/PyCharm

This document describes how to set up a remote Python interpreter in IntelliJ/PyCharm.
The use of remote interpreters is particularly useful in order to share computational resources (GPUs) or in order to execute jobs in remote clusters which provide access to data that is locally unavailable.

## Configure SSH Access with X11 Forwarding to Remote Machine

Configure an SSH connection to the remote machine using a key for authentication (no password).
Make sure X11 forwarding works for the connection, i.e. you are able to open GUI applications on the remote machine.

To support X11 on platforms that do not natively use it:
* On Windows, install and run an XServer such as [XMing](https://sourceforge.net/projects/xming/).
* On MacOS, install and run [XQuartz](https://www.xquartz.org/).

## Configure Remote Interpreter in IntelliJ

- Under File/Project Structure, select "SDKs"
- Click "+" to add a new interpreter and select "Python SDK"
- Select "SSH Interpreter"
- Enter host, port and user name and click "Next"
- Enter location of python interpreter on remote machine (ideally a virtualenv, e.g. `/usr/local/share/virtualenvs/odmalg/bin/python`)
- Deployment Configuration ("Sync Folders")
  - Choose a path for the storage of project files on the remote server, which all project files will be synchronised to, e.g. `/home/myuser/myproject-remote`
  - (The previous step will have created a new deployment, which will appear under Tools/Deployment.)

## Running Python Scripts Remotely 

To run a script remotely, simply choose the remote interpreter we configured above.

In order to make sure that interactive plots and other things that pop up windows will be transferred to your local machine, make sure that 
* you have an X11 server running on your local machine (e.g. XMing on Windows) 
* you have an open SSH connection to the remote machine with X11 forwarding enabled. Note down the value of the DISPLAY environment variable on the remote machine. 

Then, make sure IntelliJ is correctly configured:
* To make sure the the display is correctly addressed, from IntelliJ/PyCharm, *either*
    * set, in each run configuration, the environment variable DISPLAY to the value that we retrieved earlier 
    * configure the value of the DISPLAY environment variable in the Settings/"Python Console" and configure each run configuration to "Run with Python Console"
* To prevent plot windows to be captured by IntellIJ (which does not work), under Settings/Tools/Python Scientific, uncheck "Show plots in tool window".

## Reusing the Python Interpreter for Another Project

The interpreter can be reused (it is global) in a new IntelliJ/PyCharm project, but the deployment needs to configured and activated.

### Configure the Deployment in IntelliJ

- Under Tools/Deployment/Configuration, adjust the configuration of the pre-existing deployment for the server (which should be present as a result of the SSH interpreter creation), making sure it is the selected deployment (use the "tick" button). NOTE: Creating a totally new deployment (even if it contains the exact same settings) does *not* work in our experience.
- The deployment should contain the right type ("SFTP"), host name, and port and authentication information.
- Click "Test connection" to verify that the connection can be established.
- Under the "Mapping" tab, map the project root folder to a remote folder. Note that the folder specified is relative to the "Root path" under "Connection" (if it does not start with "/").
- Apply the changed configuration.
- Under Tools/Deployment, enable "Automatic Upload" (such that the menu item is ticked thereafter)

### Triggering Deployment

If "Automatic Upload" is enabled, deployments should be triggered automatically, but it is sometimes necessary to manually initiate deployment via the project context menu: Right click project title the select Deployment/"Upload to ...".

Also, if you use git to switch between states outside of IntelliJ and thus externally modify files, the deployment should configured to act on externally modified files as well: Under Tools/Deployment/Options, uncheck "Skip external changes".
