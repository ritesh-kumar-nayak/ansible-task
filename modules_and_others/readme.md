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

Ansible Roles
==============
Role_Task-1
--------
Introduction: In the previous Exercises we moved tasks to a separate tasks file and used include statements to include tasks. Now we will start using Roles to organize and reuse our work. We have created a new role using the following command 'ansible-galaxy init mysql_db'. This has created a folder structure for us.  See the folder structure in the attached image. 

Task: First, move (simply cut and paste) the below tasks into roles/mysql_db/tasks/main.yml file. 

- Install MySQL database 
- Start Mysql Service 
- Create Application Database 
- Create Application DB User 
Note: We will leave 'Install Dependencies' there, since it is not specific to database or web. We will add roles statement in a later Exercise.

Role_Task-2
----------
Introduction: We have now created a new role using the following command 'ansible-galaxy init flask_web'. This has created a folder structure for us. 

Task: Move the web related tasks to roles/flask_web/tasks/main.yml file. 

- Install Python Flask dependencies 
- Copy web-server code 
- Start web-application 
Note: We will leave Install Dependencies there, since it is not specific to database or web. We will add roles statement in a later exercise.

Role_Task-3
------------
Introduction: Let us also create a 3rd role called Python to install Python and its dependencies. A command has been run to create a new role called python and the required files are in place. 

Task: Move the first task to "Install dependencies" into "roles/python/tasks/main.yml" file. Remove the "tasks" directive from the play and leave it empty. 

Note: We will add roles statement in a later exercise.

Role_Task-4
------------
Task: To tie it all together, add roles directive to the playbook and add the roles in the following order:- 

- python 
- mysql_db 
- flask_web


Role_Task-5
------------
Introduction: We are now tasked to re-use our work to split the Architecture of our application from a monolithic (all-in-one) to distributed. Note that the inventory file is now updated with two separate servers - db_server and web_server.  Also separate host_vars file have been created for each of them. 

Task: Update the current play to target "db_server" host and apply roles "python" and "mysql_db" to it. 

Note: We need python dependencies on the sql server as well.

Role_Task-6 (Refer ansible-task\modules_and_others\playbook.yaml)
------------
Introduction: We only have db server right now. Let us create a new play to install and configure web application on the web server. 

Task: Create a new play called "Deploy a web server" and target host "web_server". Apply the roles "python" and "flask_web" to it.

Asynchronous Actions
=====================
Asynchronous Actions - 1
-------------------------
Introduction: We have added a new play at the end of our playbook to monitor the web application for 6 minutes to ensure its running OK. However, we don't want to hold the SSH Connection for the duration of this execution. 

Task: Make the monitoring task Asynchronous by adding async option to the task to wait for 6 minutes. 

The default polling interval is 10 seconds. We think that is too often. 

Task: Change it to poll every 30 seconds. 

Asynchronous Actions - 2
-------------------------------
Introduction: We have now added a new monitoring play to monitor the database. However, our playbook spends 6 minutes monitoring web application and once that completes monitors database for 6 minutes. We would like to make this parallel. 

Task: Update poll value for both tasks to 0 to "fire and forget" the monitoring tasks. 

 We do not want to just "fire and forget", we would like to "check on it later" too. 

Task: Register the results of the monitoring tasks into variables "webapp_result" and "database_result".


Error Handling
=================
Error Handling - 1
--------------------
Introduction: When ever a host fails execution of a task, Ansible removes that host from the list and continues execution of playbook with the remaining hosts. We would like Ansible to stop execution of the entire playbook if a single server was to fail. 

Task: Set the any_errors_fatal option to True for the playbook.

Error Handling - 2
---------------------
Introduction: We have added a new task at the end to send a notification email, once all tasks are complete. However, the SMTP server is not very stable and these emails are not critical. We would like Ansible to ignore even if the email task fails 

Task: Set the ignore_errors option on the mail task to "yes" to ignore errors.

