

# How to log every query on MySQL

To know every SQL query being executed on your server, you have to:

1. set debug on;
2. specify the output;
3. know how to look at it.

Here are the commands that address these three issues. You must run them as superuser on your MySQL:


    SET GLOBAL general_log = 'ON';
    SET GLOBAL log_output = 'TABLE';
    select * from mysql.general_log;
