---
layout: post
title: PHP - Encryption, Decryption and Password Hashing
description: It is very important to keep confidential data encrypted and implement secure one-way password hashing in PHP.
keywords: php encryption function, php decryption function, secure password hashing using php
tags: [PHP, Encryption, Decryption, Password Hashing]
comments: true
---

Majority of people these days are doing their business in risky situation when they are using websites or web applications that are weakly built in protecting its user data, especially user confidential information. It doesn't matter how simple or complicated, small or big the application is, when it comes to security, YOU as the developer should be responsible to do the best in delivering a safer product or service mainly when keeping your user data.

Most of the websites or web applications these days are developed in one of popular web programming called PHP. To secure the data, one of the technical methods is by encrypting it. In PHP, there are few methods you can use to encrypt the data which you have ability to decrypt it or getting back the original data. These code snippets are the ones that I usually use when developing web application or building a website to ensure user confidential information are not easily been disclosed.

### Data encryption

This is a PHP function I use to encrypt any data string.

```php
<?php
function Encrypt($password, $data)
{

    $salt = substr(md5(mt_rand(), true), 8);

    $key = md5($password . $salt, true);
    $iv  = md5($key . $password . $salt, true);

    $ct = mcrypt_encrypt(MCRYPT_RIJNDAEL_128, $key, $data, MCRYPT_MODE_CBC, $iv);

    return base64_encode('Salted__' . $salt . $ct);
}
?>
```

**Example:**

```php
<?php
echo Encrypt('myPass123', 'Welcome to Flippancy 25');
// Output: U2FsdGVkX19LYv5Y5EDmFbjH8bGMDFwlid30h2x1ybibT1Dwp0vekJ0OT4tb7/j6
?>
```

Please note that, every time I execute this `Encrypt()` function, by using the **same password** and **same data string**, the encrypted string (output) is always changed as `mt_rand()` function is used for generating the salt.

Here are the example outputs of the encrypted string after I executed it for three (3) times:-

```
// Output 1: U2FsdGVkX19LYv5Y5EDmFbjH8bGMDFwlid30h2x1ybibT1Dwp0vekJ0OT4tb7/j6
// Output 2: U2FsdGVkX1/3zxJCcE8p89t67nJNp8blNkezNxTVn4IDFQLM755K2+OSfFHewDLI
// Output 3: U2FsdGVkX18OQ8puUN8BBi+d6vAjEzDTZqM2WaKQD1atOykkYl9MY7NQM1DqI4Kw
```

Hence, even the encrypted string is changed each time I executed the function for the same input data, but it can be decrypted to get back the original data string.

### Data decryption

To get back the original data string, this function will do the work.

```php
<?php
function Decrypt($password, $data)
{

    $data = base64_decode($data);
    $salt = substr($data, 8, 8);
    $ct   = substr($data, 16);

    $key = md5($password . $salt, true);
    $iv  = md5($key . $password . $salt, true);

    $pt = mcrypt_decrypt(MCRYPT_RIJNDAEL_128, $key, $ct, MCRYPT_MODE_CBC, $iv);

    return $pt;
}
?>
```

**Example:**

```php
<?php
echo Decrypt('myPass123', 'U2FsdGVkX19LYv5Y5EDmFbjH8bGMDFwlid30h2x1ybibT1Dwp0vekJ0OT4tb7/j6');
echo Decrypt('myPass123', 'U2FsdGVkX1/3zxJCcE8p89t67nJNp8blNkezNxTVn4IDFQLM755K2+OSfFHewDLI');
echo Decrypt('myPass123', 'U2FsdGVkX18OQ8puUN8BBi+d6vAjEzDTZqM2WaKQD1atOykkYl9MY7NQM1DqI4Kw');
// All of three above will output the same decrypted data: Welcome to Flippancy 25
?>
```

Please note that, for each encryption and decryption, they requires the **same password** to work. Else, you can't get back the original data string.

### Practical use of data encryption and decryption

**Encryption** means we want to hide something, not just in plain text view. **Decryption** means we have the ability to get back the original data (in plain text view) that we hid. Basically this will be useful to work with user confidential information such as _social security number, phone number, bank account number, credit card information, etc._, but NOT FOR PASSWORD.

### Securing password by using hashing algorithm

Other than data encryption and decryption, password hashing is the most important security factor in developing application that needs user access. The best implementation in password hashing is to use **one-way hashing technique** which means it can't be decrypted and irreversible. Unlike the encryptions, they are formulated to be able to be decrypted. However, in password hashing method we can do "encrypt" it and at the end we must hash it. You may want to check the built-in functionality [password_hash](http://php.net/manual/en/function.password-hash.php) or [crypt](http://php.net/manual/en/function.crypt.php) for PHP one-way hashing algorithms.

Most of the time, in some of my projects I'm more likely to use this custom hashing function to hash the user password:

```php
<?php
function hashPassword($password, $salt) {
    $hash_password = hash("SHA512", base64_encode(str_rot13(hash("SHA512", str_rot13($salt . $password)))));
    return $hash_password;
}
?>
```

**Example:**

```php
<?php
echo hashPassword('myPa55w0rd', 'Flipp@ncy25');
// Output: 815890bb72e10a75a52087513a931afb6641a5d8d105365fa6f389f038dd81b45290a44cf94bb61e7741e073c6f4d59a16e9896bd197cc320f84f3a4d27cfb50
?>
```

Those are how I do to keep the user data secured at the maximum level and most importantly despite of whatever methods you're using, security is always the top priority. If you are keen to learn more about password hashing, I recommend you to read [this article](https://crackstation.net/hashing-security.htm). It is a very good article talking about salted password hashing.
