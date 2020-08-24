# SSH

## Permission issues

For SSH to work properly the permissions must be set as follows, according to [this](https://superuser.com/a/215506):

+ .ssh directory: 700 (drwx------)
+ public key (.pub file): 644 (-rw-r--r--)
+ private key (id_rsa): 600 (-rw-------)
+ the HOME should not be writeable by the group or others: at most 755 (drwxr-xr-x)
