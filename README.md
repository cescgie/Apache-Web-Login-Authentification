# Apache Web Login Authentification On OS X Yosemite

Adding password protection to a web site using Apache web server authentication.
--------------------------------------------------------------------------------

<p> I'll show you how to do authentication approaches using nothing more than the Apache Web Server's native capabilities. This tutorial requiring a bit of additional server configuration and a MySQL database, although you'll gain some additional flexibility along the way.</p>

<p>Using Apache's native <code>.htpasswd</code> capabilities, you can password-protect a directory in mere minutes. However, maintaining user accounts can be difficult, particularly in situations where account subscriptions are regularly created, ending, or renewed. A more flexible solution is managing the account credentials within a MySQL table and configuring Apache to compare the provided credentials against this repository. You can then create a Web-based interface to manage these accounts, or even simply manage them using a utility such as <a href="http://www.phpmyadmin.net/home_page/index.php">phpMyAdmin</a>.</p>

<p>Begin by creating the table used to manage the account credentials. At a minimum, this table should contain columns for storing the account username and password. I'll call this table <code>accounts</code>:</p>

<code>CREATE TABLE accounts ( username VARCHAR(100) NOT NULL, password CHAR(41) NOT NULL, PRIMARY KEY(username) );</code>

<p>Apache's default behavior is to use <a href="http://webopedia.com/TERM/D/DES.html">DES</a> for password encryption. However, you can also use MySQL's native <code>password()</code> function. I've opted to use the latter and so have adjusted the password column width so it can manage 41 characters, which is the size of a string encrypted using the <code>password()</code> function.</p>

<p>When the table has been created, add a few test accounts. As I mentioned previously you could use a utility such as phpMyAdmin to perform this task, but in any case the SQL query will look something like this:</p>

<code>INSERT INTO accounts VALUES('jason', password('secret')); </code>

<p>Next you'll need to configure Apache so it can communicate with the accounts table. This is done by installing the mod_auth_mysql module. In this tutorial i use OSX Yosemite.</p>
