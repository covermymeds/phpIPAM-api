# phpipamapi

A few php scripts to automate interaction with the phpIPAM IP tracking software.
phpIPAM can be found at http://phpipam.net.

## Configuration

0. Copy the contents of the ```api``` folder found here to the ```api``` folder in your
phpIPAM installation.  

0. Using the WebUI, log in as administrator and create an api endpoint.

0. Record the api name and token for later use with the scripts included here.

## Reserving or requesting an IP
Using curl, or some other suitable tool, send a request of the following format to getFreeIP.php.

```
https://[url of your install]/api/getFreeIP.php?apiapp=[api name]&apitoken=[api token]&subnet=[subnet in dotted quad]&host[host name]&user=[user name]&desc=[description]
```

The user and description parameters are optional, api name and api token should be taken from what was configured when creating the api in the WebUI.  If successful the reserved IP address will be returned.  You can only request an IP address in a subnet that exists, if the subnet requested does not exist, an error will be returned.  If the host name or dns name submitted with the request exists in phpIPAM, the IP address assigned to that name will be returned.

## Removing an IP address

```
https://[url of your install]/api/removeHost.php?apiapp=[api name]&apitoken=[api token]&host=[host name]
```

This will remove from phpIPAM the host name passed to it.

## Dumping domain or subnet information

```
https://[url of your install]/api/removeHost.php?apiapp=[api name]&apitoken=[api token]&domain=[domain or subnet]
```

You can pass either a domain name, such as example.com, or a subnet defined in phpIPAM.  When passing a domain the script will return a JSON array of fully qualified host names and IP addresses.  

```
curl https://phpipam.dev/api/removeHost.php?apiapp=myapi&apitoken=blahblahla456789&domain=example.com"
{"pete.example.com":"10.1.60.100","bob.example.com":"10.1.40.106",......}
```
```
curl https://phpipam.dev/api/removeHost.php?apiapp=myapi&apitoken=blahblahla456789&domain=10.1.16.0"
{"showy.example.com":"10.1.16.32","silly.example.com":"10.1.16.31",....}
```
