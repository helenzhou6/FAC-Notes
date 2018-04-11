# Week Seven

_Resource:_ [_week seven schedule_](https://github.com/foundersandcoders/master-reference/tree/master/coursebook/week-7), _the_ [_learning outcomes_](https://github.com/foundersandcoders/master-reference/blob/master/coursebook/week-7/learning-outcomes.md)

It's user authentication themed week!

## Day One

### Password management
_Resource:_ [_Presentation on password management_](https://docs.google.com/presentation/d/1MRdN1EvdLzFj44VafPsIXr_mh4FE_hIdorXCFFwn2vg/edit#slide=id.p7), _and_ [_password management workshop_](https://github.com/foundersandcoders/ws-password-management/blob/master/README.md), _and_ [_article the presentation and ws was based on_](http://dustwell.com/how-to-handle-passwords-bcrypt.html)

#### Passwords
* **Passwords** are used for user authentication - the most common way to prove your identity/via User accounts

* [How can companies protect you](http://plaintextoffenders.com/faq/devs)
    * Pass sensitive information through HTTPS. This essentially means a private conversation between two parties; no snooping by others since the conversation is encrypted. 
    * Don’t put limitations on the passwords people can use: characters, lengths
    * Don’t store passwords at all! Token based authentication e.g. Passwordless express module, delegate password management to other companies
    * **Hashing** – run the password through an algorithm that outputs a seemingly random string of characters. 

#### Encryption
*  ...is passing plaintext through an encryption algorithm to generate ’ciphertext’ which can only be read if decrypted. 
* Used to protect the confidentiality of digital data stored on computer systems or transmitted via the Internet. 
* Attributes:
    * Encrypted strings can be reversed back into their original decrypted form if you have the right key. (**Bidirecdtional**). 
    * [Encryption](https://danielmiessler.com/study/encoding-encryption-hashing-obfuscation/#gs.POkNU38 ) transforms data into another format in such a way that only specific individual(s) can reverse the transformation. It uses a key, which is kept secret, in conjunction with the plaintext and the algorithm, in order to perform the encryption operation. As such, the ciphertext, algorithm, and key are all required to return to the plaintext.

#### Plain text
When storing user passwords, the first consideration is to not store them in plaintext (ie storing the actual password). Some reasons for this:
* If your user data is compromised, the attackers will not only be able to log in with a user's password, but as people often reuse passwords, the users could also be compromised elsewhere. prevents escalation of a read only attack
* The passwords are visible to anyone who has access to the database.

#### STEP 1) Hashing
* ...is generating an irreversible output from a string of characters (known as the **message**) using a mathematical (hash) function. The result is a different string (the **digest**)
* A hash function should be fast to execute and slow (or impossible) to reverse.
* Used when you want to be able to compare values but not store them in plain text. E.g. passwords -- since it is is **deterministic**, meaning every time you run the same algorithm on the same string you will get the same result back.
* Hash functions: SHA-256, SHA-512, MD5 - which are algorithms.
* [Attributes of a hashing function](https://www.ssl2buy.com/wiki/difference-between-hashing-and-encryption)
    * It should not be possible to go from the output to the input (**unidirectional**)
    * Any modification of a given input should result in drastic change to the hash.

##### Using `crypto` to hash
* An example of hashing in Node.js using the built-in [`crypto` library](https://nodejs.org/api/crypto.html).
* An [article on how to use it](https://blog.ajduke.in/2016/05/28/creating-a-hash-in-node-js)
```js
const crypto = require(‘crypto’);
const hash = crypto.createHash(‘sha256’);
```
1. Creating the hash object with the specified algorithm

```js
const hash_update = hash.update(‘my super secret data’, ‘utf-8’);
```
2. Setting the data to be hashed on the hash object. 
    * This can be a string, file object, along with data we need to specify the encoding type for data, this usually utf-8 or can be binary, ascii. (The function will use utf-8 by default. )
```js
const generated_hash = hash_update.digest(‘hex’);
```
3. Creating the hash digest in the required format
    * Once we have set the data to be hashed, now we can use the digest function on the object which is returned from using the update() method. This digest function takes a parameter which asks for which format the hash to be generated should be. In our case we’ll use ‘hex’.
    * The encoding can be 'hex', 'latin1' or'base64'. If encoding is provided a string will be returned; otherwise a Buffer is returned.
    * Encoding transforms data into another format so that it can be properly (and safely) consumed by a different type of system. 

OR
```js
const { createHash } = require('crypto');

const hashedPassword = createHash('sha256').update('pa$$w0rd').digest('hex');
```
* And the comparison function:
```js
const comparePasswordWithHash = (password, hashedPassword) => {
  return hashedPassword === createHash('sha256').update(password).digest('hex');
};

```
#### Hacking
* Hashing is slightly better in that the passwords are no longer plaintext AND it’s practically impossible to reverse a one way hash function
    * BUT if you have the SHA-256 function and a database of stolen hashes, you could guess the passwords by **brute force**
    * Most hash functions can be executed very quickly -- one computer can literally try billions of combinations each second. That means most passwords can be figured out in under 1 cpu-hour.
    * Also notice that we are effectively attacking all of the passwords at the same time. It doesn't matter if there are 10 users in your database, or 10 million, it doesn't take the hacker any longer to guess a matching password.  All that matters is how fast the hacker can iterate through potential passwords.
* Years ago it would have taken ages to perform a brute force attack, so hackers used **rainbow tables** (huge databases of pre-computed hashes of the most common passwords). 

#### STEP 2) Salt
* **Salt**: a long random string of characters added to the password before hashing, to alter the resulting hash.. Salts cannot be sequential for each consecutive user.
* For fixed salt:
    ```js
    crypto.createHash('sha256').update('3c82766e7fe083d96eff7f7a' + 'pa$$w0rd').digest('hex');
    ```
    * If the hacker gains access to a table of hashes but not the salt then it makes it more difficult
    * Makes rainbow tables redundent (since they assume there is no salt)
    * Using the brute force all combinations method, they not only need to guess the password but also the long string that is the salt. 
    * BUT if they’ve broken into your server, they’ll probably have access to the salt too and if they have a match through brute force, then they know the salt for all passwords
* SO instead create a new random unique salt for each user.
    ```js
    const randomString = crypto.randomBytes(12).toString('hex');
    crypto.createHash('sha256').update(randomString + 'pa$$w0rd').digest('hex');
    ```
    * By having a per-user-salt, we get one huge benefit: the hacker can't attack all of your user's passwords at the same time.
    * This means that even in the event of an attacker getting a database dump, each password would have to be brute forced individually.
* BUT the other issue is that the security parameters length and entropy (randomness) of user chosen passwords does not scale at all with computing power. One if linear and the other logarithmic. 
    * We need a solution that is not only secure through using ‘one-way’ hash functions, but they need to be able to tackle the issue of ever more powerful computers. 

#### STEP 3) `Bcrypt`
* `Bcrypt` (paper [here](http://www.openbsd.org/papers/bcrypt-paper.ps)): a password hashing function specifically designed for passwords.
* bcrypt is an adaptive function: over time, the iteration count (work factor) can be increased to make it slower, so it remains resistant to brute-force search attacks even with increasing computation power (future proof).
    *  bcrypt() takes about 100ms to compute, which is about 10,000x slower than sha1(). 100ms is fast enough that the user won't notice when they log in, but slow enough that it becomes less feasible to execute against a long list of likely passwords.
    * To slow it down, it does this by executing an internal encryption/hash function many times in a loop. 
    * How long bcrypt takes to execute can actually be configured, by telling it how many 'rounds' of its internal hash function to execute. This number is logarithmic so the execution time increases quite sharply. This makes bcrypt future proof, as while computers get faster, the number of rounds can be increased.
* Because bcrypt() is so slow, it makes the idea of rainbow tables attractive again, so a per-user-salt is built into the Bcrypt system.
    * And the salt is added to the result string, so there's no need to store the salt and the hashed password separately.


![bcrypt image](https://i.imgur.com/oOQlNK6.png)
* As you can see, it stores both the salt, and the hashed output in the string. It also stores the log_rounds parameter that was used to generate the hash, which controls how much work (i.e. how slow) it takes to compute.
* This is great, because it means you never have to store, parse, or handle any salt values yourself -- the only value you need to deal with is that single hashed string which contains everything you need.

### Using `bcrypt` for passwords
_Resource:_ [_password management workshop_](https://github.com/foundersandcoders/ws-password-management/blob/master/README.md)

```js
bcrypt.hash('string', 10, function(err, hash) {
    // Store hash in your password DB. 
});
```

Similar to:
```js
bcrypt.genSalt(10, function(err, salt) {
    bcrypt.hash([password], salt, function(err, hash) {
        // Store hash in your password DB. 
    });
});
```
So in practice:
```js
"use strict";

const bcrypt = require("bcryptjs");

const hashPassword = (password, callback) => {
  bcrypt.genSalt(10, (err, salt) => {
    if (err) callback(new Error("salt error"));
    bcrypt.hash(password, salt, callback);
  });
};

const comparePasswords = (password, hashedPassword, callback) => {
  bcrypt.compare(password, hashedPassword, callback);
};
```
* NB: No need to do the callback bit because it is handled in a different file (i.e. the test file)

* Example of tests:

```js
test("password is being hashed correctly", t =>
  hashPassword("wehey", (err, res) => {
    t.equal(err, null, "error should be null");
    t.equal(res.substring(0, 4), "$2a$");
    t.end();
  }));

test("passwords are being validated correctly - correct password", t =>
  hashPassword("pa$$w0rd", (err, hashedPw) => {
    t.equal(err, null, "error should be null");

    comparePasswords("pa$$w0rd", hashedPw, (err, correct) => {
      t.equal(err, null, "error should be null");
      t.equal(correct, true);
      t.end();
    });
  }));

test("passwords are being validated correctly - incorrect password", t =>
  hashPassword("pa$$w0rd", (err, hashedPw) => {
    t.equal(err, null, "error should be null");

    comparePasswords("WRONG", hashedPw, (err, correct) => {
      t.equal(err, null, "error should be null");
      t.equal(correct, false);
      t.end();
    });
  }));
```

### Cookies - used for user sessions
_Resource:_ [_cookies workshop_](https://github.com/foundersandcoders/ws-cookies)
* > A [cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) is a **piece of data** that your server, hosted on a certain domain (`localhost`, `google.com` etc) sends back to the browser, which the browser will then keep, and attach to every future request _to that domain_.
    * (If you open up DevTools, and go to the 'Application' tab you will be able to see all the cookies attached to the domain you are currently on.)

* Cookies are useful as they allow us to store information about a client. As the client keeps hold of the cookie the server can simply check the cookies has the correct information.
    * You send cookies to the frontend via the response object (i.e. they are attached to the server response) in the ```'Set-Cookie'``` header 
        ```js
        res.writeHead(
            302,
            {
            'Location': '/',
            'Set-Cookie': 'logged_in=true; HttpOnly'
            }
        );
        res.end();
        ```
        * To set multiple: `'Set-Cookie': ['logged_in=true;','dfdafds=false']`
    * You read a clients cookie on server using ```request.headers.cookie```.
        * So to authenticate:
        ```js
        if (req.headers.cookie && req.headers.cookie.match(/logged_in=true/)) {
            const message = 'success!';
            res.writeHead(
            200,
            {
                'Content-Type': 'text/plain',
                'Content-Length': message.length
            }
            );
            return res.end(message);
        } else {
            const message = 'fail!';
            res.writeHead(
            401,
            {
                'Content-Type': 'text/plain',
                'Content-Length': message.length
            }
            );
            return res.end(message);
        }
        ```
* NOTE!! - Security issues
    * If you have multiple users, there is no way of telling the difference between them.
    * There is NO SECURITY in place. Cookies can very easily be edited in Devtools. For example, if you have a cookie of `admin=false`, it is very easy to change that to `admin=true`!

#### Cookie flags
You can also add 'flags' to the cookie header to enable certain behaviour. Some of the more important ones are:

Flag | Description
---|---
`HttpOnly` | This stops your cookie being accessed by the browser's Javascript (**including your own front-end code**). Without this flag, an attacker could intercept the user's cookies in a process known as Cross-Site Scripting (XSS). With the cookies of the legitimate user at hand, the attacker is able to act as the user in his/her interaction with a website - effectively identity theft.
`Secure` | This means the cookie will only be set over a HTTPS connection. This prevents a man-in-the-middle attack.
`Max-Age` | This sets the cookie lifetime in seconds.

More flags can be found [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie).

* NB: We've not included ```Secure``` as this repo is running a HTTP server. If we were running a HTTPS server this would be a great flag to include!

#### Removing cookies
* Can overwrite the value
* And set the `Max-Age` to 0.
    ```js
    res.writeHead(
        302,
        {
            'Location': '/',
            'Set-Cookie': 'logged_in=0; Max-Age=0'
        }
    );
    res.end();
    ```

## Day Two

### Using HMAC for 'signing' cookies
_Resource:_ [_Jwt Stateless Session Workshop_](https://github.com/foundersandcoders/ws-jwt-stateless-session) _and_ [_medium article on jwts and interacting with the server_](https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec)

* Pieces of information about a user within an application (that would cover most of the requests) that would be useful inside a cookie would be e.g.:
    ```js
    userInfo = {
        userId: 45,
        accessPrivileges: {
            user: true,
            admin: false
        }
    };

    const cookieValue = JSON.stringify(userInformation);

    req.setHeader('Set-Cookie', `data=${cookieValue}; HttpOnly; Secure`);
    ```

* So when the user starts a session, they will login and the server would authenticate them (using their password etc)
    * The server then generates a cookie to be able to identify ther user so the user doesn't need to provide credentials all the time/stay 'logged in'
        * Cookie would have an expiration (like a month) - so that if the old one is compromised it is considered invalid anyway.
    * When the user navigates around the site (to user-type-restricted areas)/makes API calls to the server, the server will use the cookie to validate them and enable access/processes the API call. (Example [here](https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec))
        *  Only the authentication server and the application server knows the secret key. On receiving the signed cookie, the application can perform the same signature algorithm on the unencoded userInfo, and if the hashes matches - the application knows the cookie is valid.

#### HMAC
* But security and how can you tell cookie has not been tampered with?
    * SO a [HMAC](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code) (Hash-based message authentication code) is a way to hash a message in order to verify its integrity  (not been changed) and authentication (we created it).
    * A HMAC requires a **secret** (random string), a **value** (the string you want to protect) and **mathematical algorithm** to apply to them. - known as **signing**
        * Since it is a hash, a hacker couldn't alter the encoded information since the signature has would therefore be changed and no longer match. 
        * Since using a **secret**, hacker can’t go to another client and use JWT since they dont know your secret (in your `.env` file).
        * You use HTTPS to prevent access to the JWT (and even if hacker has they can only access that specific info base on the JWT briefly)
            *  > Having HTTPS helps prevents unauthorized users from stealing the sent JWT by making it so that the communication between the servers and the user cannot be intercepted .
        * Since encoding is reversible for the payload/userInfo it allows you to create the HASH on the server side to compare it with, and be able to use the data.
* Note: When protecting a cookie, defence against brute force attacks (like `bcrypt`) is not necessary - since the data is **encoded** (to transform the data's structure) and **signed** (allowing the data receiver to verify the authenticity of the source of the data), not **encrypted** (which secures the data and prevents unauthroised access). And:
    * Just gaining access to a valid cookie will give you access to all of that users privileges, without any further work. - Since the `userInfo` is encoded and can be reversed by anyone (so shouldn't have sensitive information)
    * If you use a long, random string as a **secret**, with a modern hashing algorithm, there is not enough computing power to crack that.

#### `crypto`
* Node.js has a built-in [HMAC function - `crypto`](https://nodejs.org/dist/latest-v8.x/docs/api/crypto.html#crypto_class_hmac).
    * Similar to what has been mentioned, to make the HMAC: 
    ```js
    function sign(userInfo) {
        crypto.createHmac('sha256', secret).update(userInfo).digest('hex');
    }
    ```
    * And to validate it:
    ```js
    function(userInfo, receivedHash) {
      const correctHash = functions.sign(userInfo);
      return correctHash === receivedHash;
    }
    ```

#### JWT = JSON Web Tokens
* There is an entire open standard associated with 'signed JSON' known as [**JSON Web Tokens**](https://jwt.io/) - **JWT** for short.
    * It is a JSON object that is defined in [RFC 7519](https://tools.ietf.org/html/rfc7519) as a safe way to represent a set of information between two parties.

* JWT uses [**base64**](https://en.wikipedia.org/wiki/Base64) **encoding** which is a way of converting binary data into plain text. Encoding _is not_ the same as encrypting so sensitive information should not be stored within a JWT. We use JWTs for authentication and transferring data that you don't want to be tampered with.

* The stucture of a JWT is a string, composed of three sections, joined together by full stops. The sections are: [header].[payload].[signature]
    * **Header**  - base64 encoded object about the type of token (jwt) and the type of hashing algorithm (ie HMAC SHA256).
    ```js
    {
        alg: 'SHA256' // hashing algorithm
        type: 'JWT' // specifies that the object is a JWT
    }
    ```
    * The HMAC-SHA256 algorithm, a hashing algorithm that uses a secret key,
    * **Payload** - base64 encoded object with your **claims** in it. Mostly just a fancy name for the `userInfo` you want to store in your JWT, but it can also hold **reserved claims**, which are some useful standard values, such as `iss (issuer)`, `exp (expiration time)`, `sub (subject)`, and `aud (audience)` (that are optional).
    * **Signature** - a has of header and payload joined by a full stop.
    ```js
    hashFunction(`${encodedHeader}.${encodedPayload}`);
    ```
* So to build it in Node.js:
```js
const base64Encode = str =>
  Buffer.from(str).toString('base64');

const base64Decode = str =>
  Buffer.from(str, 'base64').toString();

// Usually two parts:
const header = {
  alg: 'SHA256', // The hashing algorithm to be used
  typ: 'JWT' // The token 'type'
};

// Your 'claims'
const payload = {
  userId: 99,
  username: 'ada'
};

const encodedHeader = base64Encode(JSON.stringify(header));
const encodedPayload = base64Encode(JSON.stringify(payload));

const SECRET = 'dafad';
const data = `${encodedHeader}.${encodedPayload}`;
const signature = hashFunction(data, SECRET);

// 'Udcna0ETPpRw5m3po3COjicb_cGJvgtnoLZyLnftaaI'

const jwt = `${encodedHeader}.${encodedPayload}.${signature}`;

// Result!
// 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJvayI6dHJ1ZSwiaWF0IjoxNTAxOTY2MjY5fQ.Udcna0ETPpRw5m3po3COjicb_cGJvgtnoLZyLnftaaI'

```
* After creating the three components (the base64url encoded versions of the header and of the payload, and the signature), combine them with periods (.) separating them.

##### Using `jsonwebtoken`
Using [`jsonwebtoken`](https://www.npmjs.com/package/jsonwebtoken), to create JWTs. - add `const { sign, verify } = require('jsonwebtoken'); const SECRET = 'dafad`
* To generate and send the cookie: `const cookie = sign(userInfo, SECRET);`
    * Then:
    ```js
    'Set-Cookie': `jwt=${cookie}; HttpOnly`
    ```
    * NB: the header information mentioned earlier are set as defaults.
* To verify incoming cookie (at a specified end point):
    ```js
      const send401 = () => {
        const message = 'fail!';
        res.writeHead(
          401,
          {
            'Content-Type': 'text/plain',
            'Content-Length': message.length
          }
        );
        return res.end(message);
      }

      if (!req.headers.cookie) return send401();

      const { jwt } = parse(req.headers.cookie);

      if (!jwt) return send401();

      return verify(jwt, SECRET, (err, jwt) => {
        if (err) {
          return send401();
        } else {
          const message = `Your user id is: ${jwt.userId}`;
          res.writeHead(
            200,
            {
              'Content-Type': 'text/plain',
              'Content-Length': message.length
            }
          );
          return res.end(message);
        }
      });
    ```
    * NB: the `verify` function from the `jsonwebtoken` module handles the verification for you (including generating the hash using the decoded userInfo)

#### Parsing cookies
[A cookie parsing npm module](https://www.npmjs.com/package/cookie) which gives you the cookies as a series of key/value pairs on an object:
```js
var cookies = cookie.parse('foo=bar; equation=E%3Dmc%5E2');
// { foo: 'bar', equation: 'E=mc^2' }
```