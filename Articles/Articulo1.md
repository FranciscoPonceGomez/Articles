# Token authentication and token expiration time

## JSON Web Token (JWT) overview
JSON Web Token (JWT) is a compact token format intended for space constrained environments such as HTTP Authorization headers and URI query parameters. JWTs encode claims to be transmitted as a JSON object (as defined in RFC 4627 [RFC4627]) that is base64url encoded and digitally signed and/or encrypted. Signing is accomplished using JSON Web Signature (JWS) [JWS]. Encryption is accomplished using JSON Web Encryption (JWE) [JWE]. 
The suggested pronunciation of JWT is the same as the English word "jot".

The following example JWT Header declares that the encoded object is a JSON Web Token (JWT) and the JWT is signed using the HMAC SHA-256 algorithm:

```
{"typ":"JWT",
 "alg":"HS256"}
 ```

Base64url encoding the bytes of the UTF-8 representation of the JWT Header yields this Encoded JWS Header value, which is used as the Encoded JWT Header:

eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9

The following is an example of a JWT Claims Set: 

```{"iss":"joe",
 "exp":1300819380,
 "http://example.com/is_root":true}
 ```

Base64url encoding the bytes of the UTF-8 representation of the JSON Claims Set yields this Encoded JWS Payload, which is used as the JWT Second Part (with line breaks for display purposes only):

```eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly
9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
```

Signing the Encoded JWS Header and Encoded JWS Payload with the HMAC SHA-256 algorithm and base64url encoding the signature in the manner specified in [JWS], yields this Encoded JWS Signature, which is used as the JWT Third Part: 

```dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk```

Concatenating these parts in the order Header.Second.Third with period characters between the parts yields this complete JWT (with line breaks for display purposes only): 

```eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9
.
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt
cGxlLmNvbS9pc19yb290Ijp0cnVlfQ
.
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```
This computation is illustrated in more detail in [JWS], Appendix A.1.


## Authentication with the Bot Connector

The Bot Connector service utilizes OAuth 2.0. 

## Issues related to expiration time in C#

![GitHub Logo](C:\Users\v-frgome\Documents\Bot\Articles\images\auth_bot_to_bot_connector.png)
Format: ![Alt Text](url)


* [node](https://nodejs.org)
* [markdown-it](https://www.npmjs.com/package/markdown-it)
* [tasks.json](/docs/editor/tasks)

> This block quote is here for your information.