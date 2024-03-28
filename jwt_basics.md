# JSON Web Token (JWT) Example

A JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties. The claims in a JWT are encoded as a JSON object that is digitally signed or integrity-protected with a Message Authentication Code (MAC) and/or encrypted.

## Structure of a JWT

A JWT is composed of three parts, each separated by a dot (`.`):

1. **Header**
2. **Payload**
3. **Signature**

These parts are base64url encoded. Here's an example of a JWT:
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvZSBEb2UiLCJhZG1pbiI6dHJ1ZSwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

### 1. Header

The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256.

- Example (encoded): `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`
- Decoded: `{"alg":"HS256","typ":"JWT"}`

### 2. Payload

The second part of the token is the payload, which contains the claims. Claims are statements about an entity (usually the user) and additional metadata.

- Example (encoded): `eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvZSBEb2UiLCJhZG1pbiI6dHJ1ZSwiaWF0IjoxNTE2MjM5MDIyfQ`
- Decoded: `{"sub":"1234567890","name":"Joe Doe","admin":true,"iat":1516239022}`

### 3. Signature

To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign it.

- Example: `SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

The signature is used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

## Important Note

While the payload of a JWT is base64url encoded, it is not encrypted. This means that the information contained in the JWT is easily decodable and should not contain sensitive information unless it is encrypted.



