# Instagram Provider for OAuth 2.0 Client

[![Build Status](https://travis-ci.org/thephpleague/oauth2-instagram.svg?branch=master)](https://travis-ci.org/thephpleague/oauth2-instagram)
[![Latest Stable Version](https://poser.pugx.org/league/oauth2-instagram/v/stable.svg)](https://packagist.org/packages/league/oauth2-instagram)

This package provides Instagram OAuth 2.0 support for the PHP League's [OAuth 2.0 Client](https://github.com/thephpleague/oauth2-client).

## Installation

To install, use composer:

```
composer require league/oauth2-instagram
```

## Usage

Usage is the same as The League's OAuth client, using `\League\OAuth2\Client\Provider\Instagram` as the provider.

### Authorization Code Flow

```php
$provider = new League\OAuth2\Client\Provider\Instagram([
    'clientId'          => '{instagram-client-id}',
    'clientSecret'      => '{instagram-client-secret}',
    'redirectUri'       => 'https://example.com/callback-url',
]);

if (!isset($_GET['code'])) {

    // If we don't have an authorization code then get one
    $authUrl = $provider->getAuthorizationUrl();
    $_SESSION['oauth2state'] = $provider->state;
    header('Location: '.$authUrl);
    exit;

// Check given state against previously stored one to mitigate CSRF attack
} elseif (empty($_GET['state']) || ($_GET['state'] !== $_SESSION['oauth2state'])) {

    unset($_SESSION['oauth2state']);
    exit('Invalid state');

} else {

    // Try to get an access token (using the authorization code grant)
    $token = $provider->getAccessToken('authorization_code', [
        'code' => $_GET['code']
    ]);

    // Optional: Now you have a token you can look up a users profile data
    try {

        // We got an access token, let's now get the user's details
        $userDetails = $provider->getUserDetails($token);

        // Use these details to create a new profile
        printf('Hello %s!', $userDetails->firstName);

    } catch (Exception $e) {

        // Failed to get user details
        exit('Oh dear...');
    }

    // Use this to interact with an API on the users behalf
    echo $token->accessToken;
}
```

## Testing

``` bash
$ ./vendor/bin/phpunit
```

## Contributing

Please see [CONTRIBUTING](https://github.com/thephpleague/oauth2-instagram/blob/master/CONTRIBUTING.md) for details.


## Credits

- [Steven Maguire](https://github.com/stevenmaguire)
- [All Contributors](https://github.com/thephpleague/oauth2-instagram/contributors)


## License

The MIT License (MIT). Please see [License File](https://github.com/thephpleague/oauth2-instagram/blob/master/LICENSE) for more information.
