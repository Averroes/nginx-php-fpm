## Introduction
This is a Dockerfile to build a container image for nginx and php-fpm, with the ability to pull website code from git. The container also has the ability to update templated files with variables passed to docker in order to update your settings. There is also support for lets encrypt SSL support.

### Git repository
The source files for this project can be found here: [https://github.com/Averroes/nginx-php-fpm](https://github.com/Averroes/nginx-php-fpm)

If you have any improvements please submit a pull request.


## Versions
| Tag | Nginx | PHP | Alpine |
|-----|-------|-----|--------|
| latest | 1.11.3 | 5.6.25 | 3.4 |
| php5 | 1.11.3 | 5.6.25 | 3.4 |
| php7 | 1.11.3 | 7.0.10 | 3.4 |

## Building from source
To build from source you need to clone the git repo and run docker build:
```
git clone https://github.com/Averroes/nginx-php-fpm
.git
docker build -t nginx-php-fpm:latest .
```

## Running
To simply run the container:
```
sudo docker run -d Averroes/nginx-php-fpm
```

You can then browse to ```http://<DOCKER_HOST>``` to view the default install files. To find your ```DOCKER_HOST``` use the ```docker inspect``` to get the IP address.

### Available Configuration Parameters
The following flags are a list of all the currently supported options that can be changed by passing in the variables to docker with the -e flag.

 - **EMAIL** : Set your email
 - **SSH_KEY** : Private SSH deploy key for your repository base64 encoded (requires write permissions for pushing)
 - **WEBROOT** : Change the default webroot directory from `/var/www/html` to your own setting
 - **HIDE_NGINX_HEADERS** : Disable by setting to 0, default behaviour is to hide nginx + php version in headers
 - **PHP_MEM_LIMIT** : Set higher PHP memory limit, default is 128 Mb
 - **PHP_POST_MAX_SIZE** : Set a larger post_max_size, default is 100 Mb
 - **PHP_UPLOAD_MAX_FILESIZE** : Set a larger upload_max_filesize, default is 100 Mb
 - **DOMAIN** : Set domain name for Lets Encrypt scripts
 - **RUN_SCRIPTS** : Set to 1 to execute scripts


#### SSH keys

##### Preparing your SSH key
The container has the option for you to pass it the __SSH_KEY__ variable with a **base64** encoded private key. First generate your key and then make sure to add it to github and give it write permissions if you want to be able to push code from the container. Then run:
```
base64 -w 0 /path_to_your_key
```
**Note:** Copy the output, but be careful not to copy your prompt

##### Running with SSH Keys

To run the container then make sure you have also supplied your base64 version of your ssh deploy key:
```
sudo docker run -d -e 'SSH_KEY=BIG_LONG_BASE64_STRING_GOES_IN_HERE' averroes/nginx-php-fpm:latest
```

### Custom Nginx Config files
Sometimes you need a custom config file for nginx to achieve this read the [Nginx config guide](https://github.com/Averroes/nginx-php-fpm/blob/master/docs/nginx_configs.md) 

### Scripting and Templating
Please see the [Scripting and templating guide](https://github.com/Averroes/nginx-php-fpm/blob/master/docs/scripting_templating.md) for more details.

### Lets Encrypt support
This container includes support to easily manage lets encrypt certificates. Please see the [Lets Encrypt guide](https://github.com/Averroes/nginx-php-fpm/blob/master/docs/lets_encrypt.md) for more details.

## Special Features
Specify the ```EMAIL```  variables for this to work. They are used to set up git correctly and allow the following commands to work.


### Logging
All logs should now print out in stdout/stderr and are available via the docker logs command:
```
docker logs <CONTAINER_NAME>
```
### WebRoot
You can set your webroot in the container to anything you want using the ```WEBROOT``` variable e.g -e "WEBROOT=/var/www/html/public". By default code is checked out into /var/www/html/ so if your git repository does not have code in the root you'll need to use this variable.

