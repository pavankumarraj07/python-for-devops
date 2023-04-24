```
#!/usr/bin/env python3

import os
import subprocess

# Install necessary packages
subprocess.run(['sudo', 'apt-get', 'update'])
subprocess.run(['sudo', 'apt-get', '-y', 'install', 'nginx', 'postgresql', 'python3', 'python3-pip'])

# Create new user and group for Python application
subprocess.run(['sudo', 'groupadd', 'myapp'])
subprocess.run(['sudo', 'useradd', '-m', '-s', '/bin/bash', '-g', 'myapp', 'myapp'])
subprocess.run(['sudo', 'passwd', 'myapp'])
subprocess.run(['sudo', 'usermod', '-aG', 'sudo', 'myapp'])

# Set appropriate permissions for new user and group
os.makedirs('/var/www/myapp')
os.chown('/var/www/myapp', user='myapp', group='myapp')
os.makedirs('/var/www/myapp/logs')
os.makedirs('/var/www/myapp/static')
os.makedirs('/var/www/myapp/media')
os.chown('/var/www/myapp/logs', user='myapp', group='myapp')
os.chown('/var/www/myapp/static', user='myapp', group='myapp')
os.chown('/var/www/myapp/media', user='myapp', group='myapp')

# Configure firewall rules
subprocess.run(['sudo', 'ufw', 'allow', '22'])
subprocess.run(['sudo', 'ufw', 'allow', '80'])
subprocess.run(['sudo', 'ufw', 'allow', '5432'])
subprocess.run(['sudo', 'ufw', 'enable'])

# Optional: Set up virtual environment
subprocess.run(['sudo', 'pip3', 'install', 'virtualenv'])
subprocess.run(['sudo', '-u', 'myapp', 'virtualenv', '/var/www/myapp/env'])
subprocess.run(['sudo', '-u', 'myapp', '/var/www/myapp/env/bin/pip', 'install', 'django'])

# Optional: Clone Git repository and install dependencies
subprocess.run(['sudo', '-u', 'myapp', 'git', 'clone', 'https://github.com/myapp/myapp.git', '/var/www/myapp/app'])
subprocess.run(['sudo', '-u', 'myapp', '/var/www/myapp/env/bin/pip', 'install', '-r', '/var/www/myapp/app/requirements.txt'])


```
### This script performs the following tasks:

1. Installs Nginx, PostgreSQL, Python3, and pip using apt-get.
2. Creates a new user and group for the Python application, sets its password, and gives it sudo privileges.
3. Sets appropriate permissions on directories and files for the new user and group.
4. Configures firewall rules to allow traffic to the server on ports 22, 80, and 5432.

Optionally, sets up a virtual environment for the Python application using virtualenv and installs Django.

Optionally, clones a Git repository containing the Python application and installs its dependencies using pip.


To use this script, you can save it to a file (e.g. configure_server.py) on your Ubuntu server, make it executable using chmod +x configure_server.py, and then run it using sudo ./configure_server.py. Note that this script assumes that you have sudo privileges on your server and are logged in as a user with those privileges.

Of course, this script is just one possible solution to the mini project, and there are many ways you could modify or improve it to fit your specific needs. However, it should give you a good starting point for automating server configuration using Python.
