## curl 

simple get request
```sh
curl example.com
```

simple post request 
```sh
curl -X POST -d "usernae=user&password=pass" example.com
```

save the file with custom name 
```sh
curl -o bigfile.zip http://example.com/bigfile.zip
```

save the file with its url  as name 
```sh
curl -O example.com 
```

adding multiple header with post request  
```sh
curl -X POST -H "Authorization: Bearer <token>" -H "Content-Type: application/json" -d '{"name": "harish"}' example.com 
```

resume downloading a file from where it left off
```sh
curl -C - -o bigfile.zip http://example.com/bigfile.zip
```

make a proxy request using curl
```sh
curl -x http://proxyserver:port http://example.com
```









## wget

download file 
```sh
wget http://example.com/file.zip
```

You can save a downloaded file with a different name using the -O option:
```sh
wget -O newname.zip http://example.com/file.zip
```

If your download was interrupted, you can resume it using the -c option:
```sh
wget -c http://example.com/largefile.zip
```

To create a local copy of a website, use the -m (mirror) option:


If you want to suppress output (useful in scripts), use the -q option:
```sh
wget -q http://example.com/file.zip
```



Download with resume capability and quiet mode
```sh
wget -c -q -O example.com/file.zip downloaded_file.zip
```









# tar

To create a .tar using multiple files or folder 
```sh
tar -cvf archive.tar /path/to/folder1  /path/to/folder2
```
```sh
tar -cvf archive.tar file1  file2
```


extract tar file
```sh
tar -xvf archive.tar
```

extract tar file to sepecific location
```sh
tar -xvf archive.tar -C /path/to/destination_folder
```


creating .tar.gz(compressed) archive from a folder
```sh
tar -czvf archive.tar.gz /path/to/folder
```

extracing .tar.gz file use -C to specify a folder
```sh
tar -xzvf archive.tar.gz
```

creating a .tar.bz2 Archive
```sh
tar -cjvf archive.tar.bz2 /path/to/folder
```

To extract a .tar.bz2 file
```sh
tar -xjvf archive.tar.bz2
```

To list the files inside a .tar archive without extracting them
```sh
tar -tvf archive.tar
```

To extract only certain files:
```sh
tar -xvf archive.tar file1.txt file2.txt
```


-c: Create a new archive.         
-z: Compress the archive with gzip.      
-v: Verbose mode.        
-f: Specifies the archive name.        
-j: Compress the archive with bzip2.        














## ping 
To check if a host is reachable and measure the round-trip time:
```sh
ping example.com
```

To send a specified number of ping requests, use the -c option:
```sh
ping -c 4 example.com
```

The -i option allows you to set the interval (in seconds) between each packet sent:
```sh
ping -i 2 example.com
```





## netstat

To list all active network connections (both TCP and UDP):
```sh
netstat -a
```


To view only the ports that are in the listening state:
```sh
netstat -l
```

To view listening ports for specific protocols (e.g., TCP or UDP):
```sh
netstat -lt   # For TCP ports
netstat -lu   # For UDP ports
```



## htop
The htop command is an interactive process viewer for Unix systems, similar to the classic top command but with a more user-friendly interface. It allows you to monitor system performance, including CPU, memory, and process usage
```sh
htop
```

## du
To get a summary of the total disk usage of the current directory:
```sh
du -sh
```
Display Disk Usage Including Hidden Files
```sh
du -ah
```
```sh
du -h /path/to/directory
```




## df used to check complete system disk usage

To show the output in a human-readable format
```sh
df -h
```


## systemctl 
The systemctl command is used in Linux systems to control and manage the systemd system and service manager. systemd is an init system and service manager that initializes system components, manages services, and handles system states. The systemctl command provides a unified interface for managing system services, sockets, devices, mounts, and more.


```sh
systemctl status nginx
```
```sh
systemctl start service_name
```
```sh
systemctl stop nginx
```
```sh
systemctl restart nginx
```
To enable a service to start automatically at boot time:
```sh
systemctl enable nginx
```
```sh
sudo systemctl disable service_name
```

To list all active (running) services:
```sh
systemctl list-units --type=service
```






## ssh , sshpass, scp 
```sh
ssh user@hostname
```

If the SSH server runs on a non-default port (default is 22):
```sh
ssh -p PORT user@hostname
```


Execute a Command on a Remote Server:
```sh
ssh user@hostname 'ls -l /path/to/directory'
```



Use SSH Key Authentication: To use an SSH key for authentication:
```sh
ssh -i /path/to/private_key user@hostname
```



### sshpass
sshpass is a tool that helps you use passwords for SSH (Secure Shell) connections automatically. Normally, when you connect to a remote server using SSH, you need to enter a password manually. This can be inconvenient if you want to automate tasks, like running scripts that connect to servers.
```sh
sshpass -p 'your_password' ssh user@hostname
```


Execute a Command Remotely:
```sh
sshpass -p 'your_password' ssh user@hostname 'command'
```




### scp 
Copy a File from Local to Remote: use (-r to copy directory)
```sh
scp /path/to/local_file user@hostname:/path/to/remote_directory
```


Copy a File from Remote to Local:
```sh
scp user@hostname:/path/to/remote_file /path/to/local_directory
```

Specify a Port: If the SCP server is running on a non-default port:
```sh
scp -P PORT /path/to/local_file user@hostname:/path/to/remote_directory
```




## ps, kill
The ps command displays information about active processes on your system.
```sh
ps aux
```
```sh
ps aux | grep python
```
```sh
kill PID
```
```sh
kill -9 PID
```





## rsync 

rsync is a powerful file synchronization and transfer tool used in Unix/Linux systems. It efficiently copies and synchronizes files and directories between two locations, whether on the same machine or between different machines over a network.


copying files locally
```sh
rsync -av /path/to/source /path/to/destination
```
Copying Files to a Remote Server:
```sh
rsync -av /path/to/local_file user@remote_host:/path/to/remote_directory
```

Copying Files from a Remote Server:
```sh
rsync -av user@remote_host:/path/to/remote_file /path/to/local_directory
```

Synchronizing Directories: To ensure the destination directory is identical to the source directory, you can use:
```sh
rsync -av --delete /path/to/source/ /path/to/destination/
```
--delete: Deletes files in the destination that are not present in the source.


Using Compression: For faster transfers, especially over slow connections, you can enable compression:
```sh
rsync -avz /path/to/source /path/to/destination
```









## jq
create a.json file 
{
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "555-1234"
    },
    {
      "type": "work",
      "number": "555-5678"
    }
  ],
  "hobbies": ["reading", "swimming", "coding"],
  "isEmployed": true
}

Print the Entire JSON Document:
```sh
jq '.' file.json
```


Extract a Specific Field:
```sh
jq '.name' profile.json
```

Extract Nested Fields:
```sh
jq '.address.city' profile.json
```


Extract Array Elements:
```sh
jq '.array_name[index]' file.json
```
List All Elements in an Array:
```sh
jq '.array_name[]' file.json
```

Filter Objects in an Array:
```sh
jq '.phoneNumbers[] | select(.type == "home")' profile.json
```


Count Elements:
```sh
jq '.array_name | length' file.json
```


Add a New Field:
```sh
jq '. + {gender: "male"}' profile.json
```

Modify an Existing Field:
```sh
jq '.name = "Jane Doe"' profile.json
```
Remove a Field:
```sh
jq 'del(.age)' profile.json
```



## sed 
create a.txt file 
    # Sample Configuration File
    # This file contains some example settings.

    server_address=old.server.com
    port=8080
    timeout=30

    # The following are some user settings
    username=admin
    password=secret
    # End of file


Replace the server address:
```sh
sed 's/old.server.com/new.server.com/g' sample_config.txt
```
Delete comment lines:
```sh
sed '/^#/d' sample_config.txt
```
Change the port number:
```sh
sed 's/port=8080/port=9090/g' sample_config.txt
```

Edit the file in place (modify the original file):
```sh
sed -i 's/old.server.com/new.server.com/g' sample_config.txt
```







# awk 
awk is a powerful text processing tool that is used for pattern scanning and processing. It operates on a line-by-line basis, allowing you to manipulate and analyze text files and streams easily. Here’s a breakdown of how awk works and some useful examples:

create a.txt 

username=johndoe
email=johndoe@example.com
age=30
location=New York
profession=Software Developer
salary=80000



Print all fields:
```sh
awk '{print $0}' a.txt
```

Print only the first and third fields:
```sh
awk -F'=' '{print $1, $3}' a.txt
```

Use = as a field separator and print the first field:
```sh
awk -F'=' '{print $1}' a.txt
```

Print lines containing "email":
```sh
awk '/email/ {print $0}' a.txt
```

Count the total number of lines:
```sh
awk 'END {print NR}' a.txt
```

create b.txt file 
username=johndoe
email=johndoe@example.com
age=30
location=New York
profession=Software Developer
salary=80000
username=janedoe
email=janedoe@example.com
age=28
location=Los Angeles
profession=Data Scientist
salary=90000


Sum all salaries:
```sh
awk -F'=' '/salary/ {sum += $2} END {print sum}' a.txt
```

Print lines where age is greater than 25:
```sh
awk -F'=' '$2 > 25 {print $0}' a.txt
```

Replace "New York" with "San Francisco" and print modified lines:
```sh
awk '{sub("New York", "San Francisco"); print}' a.txt

```












## open ssl
```sh
sudo apt-get install openssl
```

To create a private RSA key, use the following command:

Create Private Key
```sh
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
```
Create Public Key
```sh
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
Create Self-Signed Certificate
```sh
openssl req -new -x509 -key private_key.pem -out certificate.pem -days 365
```
Sign a File
```sh
openssl dgst -sha256 -sign private_key.pem -out file.sig file.txt
```
Verify Signed File
```sh
openssl dgst -sha256 -verify public_key.pem -signature file.sig file.txt
```
Encrypt a File
```sh
openssl rsautl -encrypt -inkey public_key.pem -pubin -in file.txt -out file.enc
```
Decrypt a File
```sh
openssl rsautl -decrypt -inkey private_key.pem -in file.enc -out file.dec
```
Decode a Certificate
```sh
openssl x509 -in certificate.pem -text -noout
```
