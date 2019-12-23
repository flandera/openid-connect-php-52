PHP OpenID Connect Basic Client for PHP 5.2
========================
A simple library that allows an application to authenticate a user through the basic OpenID Connect flow.
This library hopes to encourage OpenID Connect use by making it simple enough for a developer with little knowledge of
the OpenID Connect protocol to setup authentication.

Based on https://github.com/jumbojett/OpenID-Connect-PHP and reworked for PHP 5.2
More information about client functions on Gitbub https://github.com/jumbojett/OpenID-Connect-PHP

# Requirements #
 1. PHP 5.2 or greater
 2. CURL extension
 3. JSON extension

## Install ##
 1. Install library using composer
```
composer require flandera/openid-connect-php-52
```
 2. Include composer autoloader
```php
require __DIR__ . '/vendor/autoload.php';
```

## Example 1: Basic Client ##

```php
$oidc = new JumbojettOpenIDConnectClient('https://id.provider.com', 'ClientIDHere', 'ClientSecretHere');
$oidc->setCertPath('/path/to/my.cert');
```

[See openid spec for available user attributes][1]

## Example 2: Authenticate ##

```php
$oidc = new JumbojettOpenIDConnectClient('https://id.provider.com',
                                'ClientIDHere',
                                'ClientSecretHere');
$oidc->providerConfigParam(array('token_endpoint'=>'https://id.provider.com/connect/token'));
$oidc->setRedirectURL('path/where_to_send/response');
$oidc->addScope('my_scope');
//Send authorization information i.e. username and password via form to auth endpoint
//get code and state from GET response of auth endpoint if success
$_SESSION['openid_connect_state'] = $_GET['state'];
$oidc->authenticate();
//if autentization is succesfull retrive token from response
$token = $oidc->getTokenResponse();
//get identity from response JWT payload
$identity = $oidc->getIdTokenPayload()->identity;
//Or use other workflow according to OpenID documentation
```

## Development Environments ##
In some cases you may need to disable SSL security on on your development systems.
Note: This is not recommended on production systems.

```php
$oidc->setVerifyHost(false);
$oidc->setVerifyPeer(false);
```
