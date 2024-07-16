`sudo apt install samba` install samba

`sudo systemctl stop smbd` stop samba

`sudo groupadd --system smbgroup` add samba group

`sudo useradd --system --no-create-home --group smbgroup -s /bin/false smbuser` add samba user on the server

`sudo mkdir -p /share/public_files /share/protected_files` create public and protected folders

`sudo chown -R smbuser:smbgroup /share` change user and group ownership

`sudo chmod -R g+w /share` modify group level permission to write

`sudo systemctl start smdb` start samba


create `smb.conf` and `shares.conf` in /etc/samba/
> Note! rename existing smb.conf to smb.conf.bak


`smb.conf`
```bash
[global]
server string = File Server
workgroup = WORKGROUOP
security = user
map to guest = Bad User
name resolve order = bcast host
include = /etc/samba/shares.conf
```

`shares.conf`
```bash
[Public Files]
path = /share/public_files
force user = smbuser
force group = smbgroup
create mask = 0664
force create mode = 0664
directory mask = 0775
force directory mode = 0775
public = yes
writable = yes

[Protected Files]
path = /share/protected_files
force user = smbuser
force group = smbgroup
create mask = 0664
force create mode = 0664
directory mask = 0775
force directory mode = 0775
public = yes
writable = no
```
