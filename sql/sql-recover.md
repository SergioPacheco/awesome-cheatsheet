## recovering root account:

- echo "CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';" > sql\*init_file.sql
- echo "GRANT ALL PRIVILEGES ON \*.\_ TO 'root'@'localhost' WITH GRANT OPTION;" >> sql_init_file.sql
- echo "FLUSH PRIVILEGES;" >> sql_init_file.sql

- killall mysqld
- mysqld_safe --init-file=\$PWD/sql_file.sql
