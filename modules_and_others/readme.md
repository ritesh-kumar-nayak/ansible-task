WebApp-Task2,3
--------------
Introduction: Now let us add a new task to the play to install MySQL database. 

Task: Create a new task similar to the first one and use "apt" to install two more packages: 

        - mysql-server 
        - mysql-client 
Documentation: Refer to apt module - https://docs.ansible.com/ansible/2.9/modules/apt_module.html

Task: 3
========
Now let us add a new task to start the mysql service.
Task: Create a new task and use "service" module to start the "mysql" service. Ensure you set enabled to true so that the service starts automatically every time the server restarts.

Task: 5
-------
Task: Create a new task and use "mysql_db" module to create a new database by the name "employee_db". 

Documentation: Refer to mysql_db module - https://docs.ansible.com/ansible/2.9/modules/mysql_db_module.html

Task: 6
------
Task: Create a new task and use "mysql_user" module to create a new user. Use the details below:-

user name: db_user 
user password: Passw0rd 
user privilege: *.*:ALL 
host: '%' 
Documentation: Refer to mysql_user module - https://docs.ansible.com/ansible/2.9/modules/mysql_user_module.html

Task: 7
--------
Introduction: Now let us install python dependencies for the Flask Application.

Task: Create a new task and use "pip" module to install python dependencies. 

Remember to use loop with with_items to install below dependencies:-

- flask 
- flask-mysql
Documentation: Refer to pip module - https://docs.ansible.com/ansible/2.9/modules/pip_module.html



Task: 8
-------
Introduction: Now we copy our application code. As you can see our application code is located right next to our playbook. Feel free to explore it, but do not modify it. 

Task: Use the "copy" module to copy the application code file (app.py) to the location - /opt/app.py on the target server.

Documentation: Refer to copy module - https://docs.ansible.com/ansible/2.9/modules/copy_module.html

Task- 9
=======
Introduction: Let us now start the web application using the below command:-

   FLASK_APP=/opt/app.py nohup flask run --host=0.0.0.0 &  

Task: Use the "shell" module to execute the above command on the target host.

Documentation: Refer to shell module - https://docs.ansible.com/ansible/2.9/modules/shell_module.html

Task-10
-------
Introduction: Let us now improvise a little bit. Let us move some of the variable details out to a vars section at the top. 

Task: Add a vars section under the playbook and define db_name, db_user and db_password. Then replace the static definitions to use variable name.

File Separation and Organising them 
===============================
Fileseparation-1
---------
Introduction: Let us improvise our playbook a little bit more. We have created a folder called host_vars and created a file under it called "db_and_web_server.yml". 

Task: Move the connection information such as the IP and password to the new file and remove it from the inventory file.

Fileseparation-2
---------
Task: Now let us move the database parameters from the playbook and add them into the host_vars file.

Fileseparation-3
---------
Introduction: We will now separate the tasks into different files. We have now created a folder called "tasks" and created two files under it called "deploy_db.yml" and "deploy_web.yml". We will now move the tasks from the main playbook into respective tasks files - deploy_db.yml and deploy_web.yml 

Task: First, move (simply cut and paste) the below tasks into deploy_db.yml file 

- Install MySQL database 
- Start Mysql Service 
- Create Application Database 
- Create Application DB User 
Note: We will leave Install Dependencies there, since it is not specific to database or web. We will add the include statement in the next exercise.

Fileseparation-4
---------
Introduction: Let us now add an include statement in the playbook to include the tasks into this playbook. 

Task: After the first task - "Install dependencies" and before the current second task  "Install Python Flask dependencies", add include statement to include the file "tasks/deploy_db.yml"

Fileseparation-5
---------
Introduction: Do the same for all the tasks under web application deployment. A new file is created under tasks folder named - deploy_web.yml. 

Task: Move the below tasks to the new file and add include statement in the main playbook to include the file "tasks/deploy_web.yml"

- name: Install Python Flask dependencies
- name: Copy web-server code
- name: Start web-application