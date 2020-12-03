# Huami-token 

[![Build Status](https://travis-ci.org/argrento/huami-token.svg?branch=master)](https://travis-ci.org/argrento/huami-token)

Script to obtain watch or band bluetooth acess token from Huami servers. 
It will also download AGPS data packs `cep_alm_pak.zip` and `cep_7days.zip`.

## About

To use new versions of Amazfit and Xiaomi watches and bands with Gadgetbridge you need special unique key. 
Read more here: https://codeberg.org/Freeyourgadget/Gadgetbridge/wiki/Huami-Server-Pairing.

## Preparation

1. Ensure that you login in Amazfit App with Amazfit or Xiaomi account -- 
because only this login methods are supported. If not, create new Amazfit account
with e-mail and password.
2. Pair, sync and update your watch with Amazfit App. Your pairing key will be stored on
Huami servers.
3. Clone this repo:
```git clone https://github.com/argrento/huami-token.git```
4. Install requirements: `pip install -r requirements.txt`

## Usage
```
usage: huami_token.py [-h] -m {amazfit,xiaomi} [-e brazilruivo@gmail.com] [-p clps0511] [-b]
                      [-g] [-a] [-n]

Obtain Bluetooth Auth key from Amazfit servers and download AGPS data.

optional arguments:
  -h, --help            show this help message and exit
  -m {amazfit,xiaomi}, --method {amazfit,xiaomi}
                        Login method
  -e EMAIL, --email EMAIL
                        Account e-mail address
  -p PASSWORD, --password PASSWORD
                        Account Password
  -b, --bt_keys         Get bluetooth tokens of paired devices
  -g, --gps             Download A-GPS files
  -a, --all             Do everything: get bluetooth tokens, download A-GPS
                        files
  -n, --no_logout       Do not logout, keep active session and display app
                        token and access token
```


## Logging in with Amazfit account
Run script with your cridentials: `python huami_token.py --method amazfit --brazilruivo@gmail.com --clps0511 --bt_keys`.

Sample output:
```bash
> python huami_token.py --method amazfit --email my_email --password password --bt_keys
Getting access token with amazfit login method...
Token: ['UaFHW53RJVYwqXaa7ncPQ']
Logging in...
Logged in! User id: 1234567890
Getting linked wearables...
+----------------------------------------------------------+
|        MAC        |               auth_key               |
|-------------------+--------------------------------------|
| AB:CD:EF:12:34:56 |   0xa3c10e34e5c14637eea6b9efc06106   |
+----------------------------------------------------------+

Logged out.
```

Here the `auth_key` is the unique pairing key for your watch.
    
### Logging in with Xiaomi account
This is a little bit harder to use, since you need to login manually on the Xiaomi web site.

1. Run script `python huami_token.py --method xiaomi --bt_keys`.
2. Script will ask you to open Xiaomi login web page. https://account.xiaomi.com/oauth2/authorize?skip_confirm=false&client_id=2882303761517383915&pt=0&scope=1+6000+16001+20000&redirect_uri=https%3A%2F%2Fhm.xiaomi.com%2Fwatch.do&_locale=en_US&response_type=code
3. Login with your credentials there.
4. If your login is successful, browser will show the error that connection is not secured. 
On this stage address will look like this: `https://hm.xiaomi.com/watch.do?code=ALSG_CLOUDSRV_9B8D87D0EB77C71B45FF73B2266D922B`. 
5. Copy this address.
6. Return to script, paste this address and press `enter`.

Sample output:
```bash
> python huami_token.py --method xiaomi --bt_keys
Getting access token with xiaomi login method...
Copy this URL to web-browser

https://account.xiaomi.com/oauth2/authorize?skip_confirm=false&client_id=2882303761517383915&pt=0&scope=1+6000+16001+20000&redirect_uri=https%3A%2F%2Fhm.xiaomi.com%2Fwatch.do&_locale=en_US&response_type=code

and login to your Mi account.

Paste URL after redirection here.
https://hm.xiaomi.com/watch.do?code=ALSG_CLOUDSRV_9B8D87D0EB77C71B45FF73B2266D922B
Token: ['ALSG_CLOUDSRV_9B8D87D0EB77C71B45FF73B2266D922B']
Logging in...
Logged in! User id: 3000654321
Getting linked wearables...
+----------------------------------------------------------+
|        MAC        |               auth_key               |
|-------------------+--------------------------------------|
| 12:34:56:AB:CD:EF |   0x3c10e34e5c1463527579996fa83e6d   |
| BA:DC:FE:21:43:65 |                 0x00                 |
+----------------------------------------------------------+

Logged out.
```

Here the `auth_key` is the unique pairing key for your watch. 

_In this example I have two devices: the first one is my Amazfit Bip S watch, 
the second one is my Xiaomi Mi Smart Scale._


## Dependencies

* Python 3.7.7
* argparse
* requests
* urllib
* random
* uuid
* json
* shutil
* rich

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/argrento/huami-token/tags). 

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
