
# JWT Signature Validation in Go

Validating the signature of a JWT (JSON Web Token) is crucial for ensuring the integrity and authenticity of the token. The signature ensures that the token hasn't been altered after it was issued. This document outlines the steps involved in signature validation and provides a sample code snippet for doing so in a Go application using the `github.com/golang-jwt/jwt` library.

## How JWT Signature Validation Works

1. **Extract and Decode**: A JWT consists of three Base64-URL encoded parts separated by dots (`.`): Header, Payload, and Signature. The server extracts these components from the incoming token.

2. **Verify Algorithm**: The Header part of the JWT specifies the algorithm used for signing. The server must verify that this algorithm matches one it expects and supports.

3. **Recompute the Signature**: The server recomputes the signature by concatenating the encoded Header and Payload (separated by a dot `.`), and then applying the cryptographic algorithm specified in the Header using a secret key.

4. **Compare Signatures**: The server compares the newly computed signature with the signature part of the JWT. If they match, the token is confirmed to be authentic and unaltered.

## Code Example

Below is a Go function that demonstrates how to validate a JWT, focusing on the signature validation:

```go
package main

import (
	"fmt"
	"github.com/golang-jwt/jwt"
	"time"
)

var SecretKey = []byte("your-256-bit-secret")

func ValidateToken(tokenString string) (*jwt.Token, error) {
	// ParseWithClaims parses the token and ensures the signature is valid.
	token, err := jwt.ParseWithClaims(tokenString, &jwt.StandardClaims{}, func(token *jwt.Token) (interface{}, error) {
		if _, ok := token.Method.(*jwt.SigningMethodHMAC); !ok {
			return nil, fmt.Errorf("unexpected signing method: %v", token.Header["alg"])
		}
		return SecretKey, nil
	})

	if err != nil {
		return nil, err
	}

	if claims, ok := token.Claims.(*jwt.StandardClaims); ok && token.Valid {
		// Here, you could also validate the time-based claims such as `ExpiresAt`, `NotBefore`, etc.
		// For example:
		if time.Unix(claims.ExpiresAt, 0).Before(time.Now()) {
			return nil, fmt.Errorf("token has expired")
		}
	} else {
		return nil, fmt.Errorf("invalid token")
	}

	return token, nil
}

func main() {
	tokenString := "your.jwt.token.here"

	token, err := ValidateToken(tokenString)
	if err != nil {
		fmt.Println("Token validation error:", err)
		return
	}

	fmt.Println("Token is valid:", token.Valid)
}
```

### Key Points

- The `ParseWithClaims` method is critical for parsing the token string and validating its signature against the provided secret.
- Ensuring the algorithm in the JWT's header matches the expected algorithm is crucial for security.
- The `StandardClaims` struct includes fields for standard JWT claims, such as `exp` (ExpiresAt), which you may want to validate.
- Managing the secret key securely is paramount to the integrity of the signature validation process.
