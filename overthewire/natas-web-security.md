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
Next Password:    
URL:              http://natas3.natas.labs.overthewire.org
```
