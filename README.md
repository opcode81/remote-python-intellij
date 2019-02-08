# Remote Python Interpreter Configuration in IntelliJ/PyCharm

This document describes how to set up a remote Python interpreter in IntelliJ/PyCharm.
The use of remote interpreters is particularly useful in order to share computational resources (GPUs) or in order to execute jobs in remote clusters which provide access to data that is locally unavailable.

## Configure SSH Access with X11 Forwarding to Remote Machine

Configure an SSH connection to the remote machine using a key for authentication (no password).
Make sure X11 forwarding works for the connection.

## Configure Remote Interpreter in IntelliJ

- Under File/Project Structure, select "SDKs"
- Click "+" to add a new interpreter
- Select "SSH Interpreter"
- Enter host, port and user name and click "Next"
- Enter location of python interpreter on remote machine (ideally a virtualenv, e.g. `/usr/local/share/virtualenvs/odmalg/bin/python`)
- Deployment Configuration
  - Enter path for storage of temporary files (all relevant project files to be synchronised), e.g. `/home/myuser/myproject-remote`
  - This last step will have created a new deployment, which will appear under Tools/Deployment

## Running Python Scripts Remotely 

To run a script remotely, simply choose the remote interpreter we configured above.

In order to make sure that interactive plots and other things that pop up windows will be transferred to your local machine, make sure that 
* you have an X11 server running on your local machine (e.g. XMing on Windows) 
* you have an open SSH connection to the remote machine with X11 forwarding enabled. Note down the value of the DISPLAY environment variable on the remote machine. 

Then, to make sure the the display is correctly addressed, from IntelliJ/PyCharm, *either*
* set, in each run configuration, the environment variable DISPLAY to the value that we retrieved earlier 
* configure the value of the DISPLAY environment variable in the Settings/"Python Console" and configure each run configuration to "Run with Python Console"

## Reusing the Python Interpreter for Another Project

The interpreter can be reused (it is global), but a new deployment needs to be created and activated.

### Configure a New Deployment in IntelliJ

- Under Tools/Deployment/Configuration, click "+" to add a new Deployment.
- Choose a name and select type "SFTP"
- Set host name, port and root path (absolute path on remote machine).
  Set user name (for SSH access) and choose, for example, "Key pair" as authentication type, setting your private key and (optionally) passphrase.
- Click "Test connection" to verify that the connection can be established.

### Activate the Deployment

In the list of deployments, activate the one that is to be used for your project by using the "tick" button.
