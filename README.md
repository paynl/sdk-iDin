# PAY. PHP SDK-iDIN

---

- [About](#about)
- [Installation](#installation) 
- [Installation without composer](#installation-without-composer) 
- [Setting up](#setting-up)
	- [1. The autoloader](#step-1-the-autoloader)  
	- [2. Your API token](#step-2-your-apitoken)
- [Examples](#examples)
	- [1. getIssuers](#getissuers)  
	- [2. Authenticate](#authenticate)	
	- [3. Status](#status)

---

### About
In order to use this SDK, you'll need to have iDIN enabled for your PAY. account.

This SDK extends the standard [PAY. SDK](https://github.com/paynl/sdk), so all functions from the original SDK are also available.

### Installation

This SDK uses composer.

Composer is a tool for dependency management in PHP. It allows you to declare the libraries your project depends on and it will manage (install/update) them for you.

For more information on how to use/install composer, please visit: [https://github.com/composer/composer](https://github.com/composer/composer)

To install the PAY. PHP SDK-iDIN into your project, simply

	$ composer require paynl/sdk-idin


### Installation without composer 
Coming soon..

### Setting up
To communicate with the API of PAY, you'll need to authenticate.
PAY. uses a token to authenticate you. You can find your token in the PAY.admin. On the bottom of the [API Tokens](https://admin.pay.nl/company/tokens) page.

##### Step 1 the autoloader
Composer generates an autoloader for your application.
To be able to access the classes of the SDK, all you have to do is include the composer autoloader.
The autoloader is located here: vendor/autoload.php

```php
require_once('path_to/vendor/autoload.php');
```

##### Step 2 Your APItoken
To let the SDK know what your API token is, you'll have to register the TokenCode (AT-code belonging to the token) and API token as follows:

```php
\Paynl\Config::setTokenCode('AT-....-....');
\Paynl\Config::setApiToken('YOUR_API_TOKEN');
```

Now you're ready to make some calls



### Examples

The full list of functions can be found in the [samples](https://github.com/paynl/sdk-iDIN/tree/master/samples) folder.

##### getIssuers
Gets an array with issuers
```php
$issuers = Paynl\Idin\Issuers::get();
var_dump($issuers);
```

##### Authenticate
Starts an iDIN transaction based on issuer and merchant reference.
```php
$result = Paynl\Idin\Authenticate::start(array(
    // Required     
    'reference' => 'MER.REF.1234.5678', 
    'issuerId' => 'ABNANL2A',
    'data' => array(
        'name' => '1',
        'address' => '1',
        'isEighteen' => '1',
        'dateOfBirth' => '1',
        'gender' => '1',
        'email' => '1',
        'phone' => '1'
    ),
    'language' => 'nl',
    'returnUrl' => 'https://www.pay.nl',
    'exchangeUrl' => 'https://www.pay.nl'        
));
```

##### Status
Get the current status of an iDIN transaction
possible statuses:
- Init: The request is created; no iDIN status available (yet);
- Open: Final result not yet known;
- Pending: Transaction has not yet been completed;
- Success: Positive result; the transaction is or has been executed;
- Cancelled: Negative result due to cancellation by Consumer; the transaction will not be executed;
- Expired: Negative result due to expiration of the transaction; the transaction will not be executed;
- Failure: Negative result due to other reasons; the transaction will not be executed;
- Error: Error message received from issuer;
```php
$result = Paynl\Idin\Status::get(array(
    // Required      
    'trxid' => 'DA-1234-5678-9012'       
));
```








