📌 HDFS dfs Command Table (Most Important for Exams)
Command	Purpose	Example
-ls	List files/directories	hdfs dfs -ls /user/hduser
-ls -R	Recursive listing	hdfs dfs -ls -R /data
-mkdir	Create directory	hdfs dfs -mkdir /data/input
-mkdir -p	Create parent dirs if needed	hdfs dfs -mkdir -p /data/2025/jan
-rmdir	Remove empty directory	hdfs dfs -rmdir /data/tmp
-rm	Remove file	hdfs dfs -rm /data/file.txt
-rm -r	Remove directory recursively	hdfs dfs -rm -r /data/old
-rm -skipTrash	Delete permanently	hdfs dfs -rm -skipTrash /data/file.txt
-mv	Move/Rename within HDFS	hdfs dfs -mv /data/a.txt /archive/a.txt
-cp	Copy inside HDFS	hdfs dfs -cp /data/a.txt /backup/
-put	Copy from local → HDFS	hdfs dfs -put local.txt /data/
-copyFromLocal	Same as put	hdfs dfs -copyFromLocal test.csv /data/
-get	Copy from HDFS → local	hdfs dfs -get /data/out.txt /home/user/
-copyToLocal	Same as get	hdfs dfs -copyToLocal /data/log /home/
-getmerge	Merge HDFS files to one local file	hdfs dfs -getmerge /data/out/ result.txt
-appendToFile	Append local file to HDFS file	hdfs dfs -appendToFile a.txt b.txt /data/big.txt
-cat	Display file contents	hdfs dfs -cat /data/file.txt
-tail	Show last 1 KB of file	hdfs dfs -tail /logs/app.log
-text	Convert to text (SequenceFile, etc.)	hdfs dfs -text /data/seqfile
-du	Show space usage	hdfs dfs -du /data
-du -h	Human readable	hdfs dfs -du -h /data
-dus	Disk usage summary	hdfs dfs -dus /data/input
-count	Count dirs, files, bytes	hdfs dfs -count /data
-stat	File metadata (size, mod time)	hdfs dfs -stat %n %b %y /data/file.txt
-chmod	Change permissions	hdfs dfs -chmod 755 /data/file.txt
-chown	Change owner	hdfs dfs -chown hduser /data/file.txt
-chgrp	Change group	hdfs dfs -chgrp hadoop /data/
-setrep	Change replication factor	hdfs dfs -setrep 3 /data/file.txt
-checksum	Show file checksum	hdfs dfs -checksum /data/file.txt
-test	Test for existence/files/dir	hdfs dfs -test -e /data/file.txt
-help	Show help	hdfs dfs -help


1️⃣ BASIC LEVEL
Task No.	Description	Hive Command	Example / Notes
1	Create database	create database employee_db;	Creates DB
	Switch DB	use employee_db;	—
2	Create simple employee table	create table employee_basic(emp_id int, emp_name string, age int);	3-column table
3	Insert sample data	insert into employee_basic values (1,'Amit',25),(2,'Riya',29);	Adds 2 rows
4	Simple select	select * from employee_basic;	View records
________________________________________
2️⃣ INTERMEDIATE LEVEL
Task No.	Description	Hive Command	Example / Notes
5	Create detailed employee table with delimiter	create table employee_details(emp_id int, emp_name string, age int, salary float, city string, department string) row format delimited fields terminated by ',';	CSV-friendly table
6	Load data from HDFS	load data inpath '/data/emp_details.csv' into table employee_details;	Loads CSV
7	Filter employees by city	select * from employee_details where city='Bangalore';	Filtering example
8	Max salary	select max(salary) from employee_details;	Aggregation
9	Count by city	select city, count(*) from employee_details group by city;	Grouping
________________________________________
3️⃣ ADVANCED LEVEL
Task No.	Description	Hive Command	Example / Notes
10	Create partitioned salary table	create table employee_salary(emp_id int, salary float) partitioned by (year int, month int);	Partitioned by year/month
11	Insert into partition	insert into table employee_salary partition(year=2025, month=1) select emp_id, salary from employee_details;	Partition load
12	Create department table	create table department(dept_id int, dept_name string);	—
	Insert dept values	insert into department values (1,'IT'),(2,'HR'),(3,'Finance');	—
13	Join employee + department	select e.emp_name, e.salary, d.dept_name from employee_details e join department d on e.department=d.dept_name;	Inner join
14	Avg salary by department	select department, avg(salary) from employee_details group by department;	Aggregation
________________________________________
4️⃣ EXPERT LEVEL
Task No.	Description	Hive Command	Example / Notes
15	Create bucketed table	set hive.enforce.bucketing=true; create table employee_bucketed(emp_id int, emp_name string, salary float) clustered by(emp_id) into 4 buckets stored as orc;	Bucketed + ORC
16	Insert data into bucketed table	insert into table employee_bucketed select emp_id, emp_name, salary from employee_details cluster by(emp_id);	Ensures bucket distribution
17	Enable ACID	set hive.txn.manager=org.apache.hadoop.hive.ql.lockmgr.DbTxnManager; set hive.support.concurrency=true;	For update/delete
18	Update employee salary	update employee_bucketed set salary = salary + 5000 where emp_id = 1;	Transactional update
19	Top 3 highest-paid per department	select emp_name, department, salary from (select emp_name, department, salary, dense_rank() over (partition by department order by salary desc) as rnk from employee_details) t where rnk <= 3;	Window function
20	Most experienced employee per city (custom question)	select emp_name, city, experience from (select emp_name, city, experience, row_number() over (partition by city order by experience desc) as rn from employee_details) x where rn=1;	Advanced analytics

