# RFC 7519 JSON Web Token (JWT) - Summary

* [https://tools.ietf.org/html/rfc7519](https://tools.ietf.org/html/rfc7519)

JSON Web Token (JWT) is a compact claims representation format intended for space constrained environments such as HTTP Authorization headers and URI query parameters.

* ‘Compact’ refers to JSON in comparison to XML rather than smaller in comparison to the non-encoded data.
* ‘Claims’ are pieces of information asserted about a subject.
	* Claims are conveyed as key value pairs in JSON – “It’s just JSON, nothing fancy.”
	* 7 Registered claim names
		* Issuer, Subject, Audience, Expiration Time, Not Before, Issued At, JWT Identifier
		* All of them are optional. Yes, optional. It’s an awesome standard. ;-)
		* Abbreviated to three letters when used within JSON: iss, sub, aud, exp, nbf, iat, jti
	* Services commonly (almost always) use Private claim names.
		* “Private Claim Names are subject to collision and should be used with caution.”
		* Simple names if internal, collision resistant (example: FQDN URL) if exchanged with third parties.
	* There are only a few Public claim names at present -- see [http://www.iana.org/assignments/jwt/jwt.txt](http://www.iana.org/assignments/jwt/jwt.txt)
* JWTs are able to be replayed and forwarded.
* Client code is able to request scopes individually: `https://example.com/authorize;scope=openid+post-data`

## Anatomy of a JWT

![Anatomy of a JWT Image](./jwt-summary-anatomy.png)

## Usage Examples

### URL

```
https://example.com/resource/abc123?authorization=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJqb2huZG9lIiwiaHR0cDovL2V4YW1wbGUuY29tL25hbWUiOiJKb2huIERvZSIsImh0dHA6Ly9leGFtcGxlLmNvbS9oYXBweSI6dHJ1ZX0.649N7uXd9YJsdPSN5YR1GJUOAUXNk7_bo-86xnCTKR0
```

### HTTP Header

```
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJqb2huZG9lIiwiaHR0cDovL2V4YW1wbGUuY29tL25hbWUiOiJKb2huIERvZSIsImh0dHA6Ly9leGFtcGxlLmNvbS9oYXBweSI6dHJ1ZX0.649N7uXd9YJsdPSN5YR1GJUOAUXNk7_bo-86xnCTKR0
```

## Access Tokens and Tokens

* JWTs are tokens.
* Not all JWTs are access tokens.
* Not all access tokens are JWTs.

## Claims and Scopes

* Some claims are scopes.
	* ‘Scopes’ and ‘claims’ terminology is sometimes used interchangeably.
	* All scopes are defined as claims.
	* Not all claims are scopes.
* Scopes are how services define and determine what the caller is authorized to do.

```
  |-- Claim --| |----- Claim -----| |-- Claim+Scope --|
{ "sub": "ASH", "name": "Ashleigh", "scope": "chewtoy" } <-- My 16 year old daughter
{ "sub": "CHA", "name": "Charlie",  "scope": "kitten"  } <-- Me, her dad
```
* I can buy a kitten. My daughter can purchase a chew toy. Yay for scopes.

```
POST https://example.com/shopforpetstuff { "dollars": 40 }
```

* Services can define scopes for themselves.
* Services can share scopes with other services.
