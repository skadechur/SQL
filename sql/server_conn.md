# Connect MySQL to remote server

## Steps
On local machine
```
1. ssh-add ~/.ssh/<lcalkey>
2. ssh -L <your_port_no>:localhost:3306 -N <linuxusername>@13.127.234.41
```

Login to Server
```
1. mysql --host=127.0.0.1 -p<your_port_no> -u<user_name> -p<password>
```