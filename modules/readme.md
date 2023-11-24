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