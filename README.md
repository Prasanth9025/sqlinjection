# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/83b2fd89-bb1e-4ea5-b5bd-767b7b2ff1c0)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/18b801a1-1da4-47e0-a0b3-61ad4b46a331)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![WhatsApp Image 2023-11-09 at 14 20 06](https://github.com/Prasanth9025/sqlinjection/assets/118343686/04c723aa-e06b-4929-b406-06d6270f329b)

Click on the menu Login/Register and register for an account

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/6bfa4010-0c88-4a8a-8184-cf57937f5814)

Click on the link “Please register here”
![WhatsApp Image 2023-11-09 at 14 20 06 1](https://github.com/Prasanth9025/sqlinjection/assets/118343686/66c98b2c-2151-41b5-87f8-cf62c67d6419)
Click on “Create Account” to display the following page:
![WhatsApp Image 2023-11-09 at 14 20 05 2](https://github.com/Prasanth9025/sqlinjection/assets/118343686/3434a8c1-4154-45f8-a086-68b8ad9480b3)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![WhatsApp Image 2023-11-09 at 14 20 06 3](https://github.com/Prasanth9025/sqlinjection/assets/118343686/3cb0db70-8464-417b-883a-ac8493dfde9e)

Click “Login”. The logged in page will show as below:
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/b9e117ab-e57b-496a-a723-f10dc1c320a0)
##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![WhatsApp Image 2023-11-09 at 14 20 05 4](https://github.com/Prasanth9025/sqlinjection/assets/118343686/93ad769e-ddc7-491f-8c58-3c1866f11027)

Click the login button and you will see it enter into the administrator page. 
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/e98b9168-b90b-47d6-9af1-45afcb34e1f7)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![WhatsApp Image 2023-11-09 at 14 20 06 5](https://github.com/Prasanth9025/sqlinjection/assets/118343686/ac94e83e-5140-4c3a-8c62-58518515807a)
![WhatsApp Image 2023-11-09 at 14 20 06  6](https://github.com/Prasanth9025/sqlinjection/assets/118343686/2fbde3d7-ef83-4254-a8a2-d2734eb6e61a)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/e482d2c6-79d4-425f-a751-64455f8dbec3)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/83ec9142-7624-42f7-b418-e2e1b9e41c9b)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/e83bf0f7-21c3-4df0-8562-ebb651c29289)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/0456c798-ff51-4210-9f38-b2a1711df910)

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/fcb705c3-4cd5-4d6e-954d-514ac381b558)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/36de9e23-def6-4437-ae49-33e11766ddb3)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/50e9895e-6a65-4d04-9a28-88f3a50b1e13)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/03e30bf7-4732-41d3-8353-1bf7b302b140)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/05b84bea-f41d-4052-8bff-affd3c916ec4)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).
http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Prasanth9025/sqlinjection/assets/118343686/a08bd123-bd1a-457d-82c4-66d73f1b2a43)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
