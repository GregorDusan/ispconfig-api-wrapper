# ISPConfig 3 remoting API Wrapper

## Introduction

A simple wrapper for ispconfig3 remote API.

Based on repository: [pemedina/ispconfig-wrapper](https://github.com/pemedina/ispconfig-wrapper).

Designed to interoperate with ISPConfig 3, it aims to provide an expressive yet simple interface to perform all actions provided by the API.

[![Latest Stable Version](http://poser.pugx.org/gregordusan/ispconfig-api-wrapper/v?style=for-the-badge)](https://packagist.org/packages/gregordusan/ispconfig-api-wrapper) [![Total Downloads](http://poser.pugx.org/gregordusan/ispconfig-api-wrapper/downloads?style=for-the-badge)](https://packagist.org/packages/gregordusan/ispconfig-api-wrapper) [![Latest Unstable Version](http://poser.pugx.org/gregordusan/ispconfig-api-wrapper/v/unstable?style=for-the-badge)](https://packagist.org/packages/gregordusan/ispconfig-api-wrapper) [![License](http://poser.pugx.org/gregordusan/ispconfig-api-wrapper/license?style=for-the-badge)](https://packagist.org/packages/gregordusan/ispconfig-api-wrapper?style=for-the-badge) [![PHP Version Require](http://poser.pugx.org/gregordusan/ispconfig-api-wrapper/require/php?style=for-the-badge)](https://packagist.org/packages/gregordusan/ispconfig-api-wrapper)

## Requirements

* PHP >= 7.4 (with [soap](http://se2.php.net/soap) support)

## Getting started

The library acts as a proxy between ISPConfig 3 SOAP server and your app. All functions are renamed to a more expressive (IMHO) camelCase syntax. IT *doesn't* do any validation, just proxies every request to the related SOAP call.
The *only* change is that every response is returned as a json encoded array.

 +  Exceptions are trapped and converted to json, wrapped as `errors`.
 +  Single value responses are converted to json , wrapped as `result`.
 +  Array responses are converted to json.

Create remote user in ISPConfig.

## Composer

```bash
$ composer require gregordusan/ispconfig-api-wrapper dev-main
```

## Usage

The wrapper can be included & used on any PHP application.

## Examples

### Expressive syntax.

``` php
<?php

require_once 'vendor/autoload.php';

use ISPConfigWrapper\ISPConfigWS;

$webService = new ISPConfigWS([
    'host' => 'https://domain.com:8080', // ispconfig url address
    'user' => 'username',
    'pass' => 'password',
]);

// Get websites by username (using two calls below)

// get client_id by username
$result = $webService
    ->with([
        'username' => 'username', // ispconfig client username
    ])
    ->getClientByUsername()
    ->response();
$client = json_decode($result);

// get list of client websites
$result = $webService
    ->with([
        'user_id' => $client->userid,
        'group_id' => null,
    ])
    ->getClientSites()
    ->response();

print_r(json_decode($result));


// Single call (change password for client_id = 1)

$result = $webService
    ->with([
        'client_id' => 1,
        'password' => 'newPassword',        
    ])
    ->changeClientPassword()
    ->response();

print_r(json_decode($result));
```

## Feedback and questions

Found a bug or missing a feature? Don't hesitate to create a new issue here on GitHub.




