# assignment
NOTE: CI-CD approach step is added in CI-CD-Approach.docx .Please download this file to have a look. 

#manual deployment Steps

#clone repo
git clone https://github.com/nitishNec/assignment.git

#cd to assignment directory
cd "assignment"

#list all files and directory using below commands 
ls -ltrh

Output:
-rw-r--r-- 1 root root   12 Feb 14 17:43 README.md
drwxr-xr-x 3 root root 4.0K Feb 14 17:43 mediawiki
drwxr-xr-x 3 root root 4.0K Feb 14 19:34 database

#you will see two directory "database" and "mediawiki". These directory is divided based backend(mariaDB database for mediawiki) and frontend(mediawiki)

database:

#Move to database directory now by using below commands 
cd database

#Again list all files and directory of database using below commands
ls -ltrh
Output:
-rw-r--r-- 1 root root    0 Feb 14 17:43 README.md
-rw-r--r-- 1 root root  246 Feb 14 17:43 init.sql
drwxr-xr-x 3 root root 4.0K Feb 14 18:36 chart
-rwxr-xr-x 1 root root   51 Feb 14 19:33 run.sh
-rw-r--r-- 1 root root  509 Feb 14 19:34 Dockerfile

>> Dockerfile : Docker file to create image of mariaDB
>> init.sql: Sql query to create database and provide required 
>> chart/wikidatabase: Helm chart for database
>> run.sh: db sql query executor script 

Steps to create images for database:
1. make sure you are inside database directory
2. Docker build . -t "IMAGE_NAME:TAG"
3. tag docker images based on the repo name (docker tag xxxx:v1 xyz/xxxx:v1)
4. push image to your registery (docker push xyz/xxxx:v1)

steps to Deploy helm chart of database
1. make sure you are inside database/chart
2. Edit the image name and tag name of image in values.yaml
3. Deploy helm chart (helm install wikidatabase --generate-name)
4. Check release (helm ls )
5. check pod status (kubectl pod status/ kubectl get svc )
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

mediawiki:

#Move to mediawiki directory now by using below commands 
cd mediawiki

#Again list all files and directory of mediawiki using below commands
ls -ltrh
Output:
-rw-r--r-- 1 root root    0 Feb 14 17:43 README.md
-rw-r--r-- 1 root root 1.3K Feb 14 17:43 Dockerfile
drwxr-xr-x 3 root root 4.0K Feb 14 17:43 chart
-rw-r--r-- 1 root root  34M Feb 14 17:43 mediawiki-1.31.0.tar.gz
-rw-r--r-- 1 root root  382 Feb 14 17:43 virtualhost.conf
-rw-r--r-- 1 root root  109 Feb 14 17:43 php.ini

>> Dockerfile : Docker file to create image mediawiki 
>> mediawiki-1.31.0.tar.gz: source code for mediawiki
>> virtualhost.conf : apache virtual hosting file for media wiki
>> php.ini: Basic php configuration file 

Steps to create images for mediawiki:
1. make sure you are inside mediawiki directory
2. Docker build . -t "IMAGE_NAME:TAG"
3. tag docker images based on the repo name (docker tag xxxx:v1 xyz/xxxx:v1)
4. push image to your registery (docker push xyz/xxxx:v1)

steps to Deploy helm chart of mediawiki
1. make sure you are inside mediawiki/chart/
2. Edit the image name and tag name of image in values.yaml
3. Deploy helm chart (helm install mediawiki --generate-name)
4. Check release (helm ls )
5 .Access mediawiki app using loadbalancer DNS (http://{DNS loadbalancer:80/mediawiki/}) 
