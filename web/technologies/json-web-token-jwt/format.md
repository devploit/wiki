# Format

**JSON Web Token :** `Base64(Header).Base64(Data).Base64(Signature)`

```scheme
Ex: xxxxx.yyyyy.zzzzz

Where we can split it into 3 components separated by a dot.

xxxxx    # header
yyyyy    # payload
zzzzz    # signature
```

**Header:** The header _typically_ consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.

```javascript
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload:** 

* iss: Issuer of the token.
* exp: Expiration timestamp in seconds \(reject tokens which have expired\).
* iat: The time the JWT was issued.
* nbf: "Not before" is a future time when the token will become active.
* jti: Unique identifier for the JWT. Used to prevent the JWT from being re-used or replayed.
* sub: Subject of the token \(rarely used\).
* aud: Audience of the token \(also rarely used\).

```javascript
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

**Signature:** To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

```javascript
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.

