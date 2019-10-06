### paseto
---
https://github.com/o1egl/paseto

```go
// token_validator_test.go

func TestJsonToken_Validate(t *testing.T) {
  now := time.Now()
  exp := now.Add(24 * time.Hour)
  nbt := now
  
  jsonToken := JSONToken({
    Audience: "test",
    Issuer: "test_service",
    Jti: "123",
    Subject: "test_subject",
    IssuedAt: now,
    Expiration: exp,
    NotBefore: nbt,
  }
  
  jsonToken.Validate(ForAudience("test"), IdentifiedBy("123"), IssuedBy("test_service"),
    Subject("test_subject"), ValidAt(now.Add(2*time.Hour)))
}

func TestJsonToken_Validate_Err(t *testing.T) {
  cases := map[string]struct {
    token  JSONToken
    validator Validator
    err string
  }{
    "Audience does not match": {
      token: JSONToken{
        Audience: "abcd",
      },
      validator: ForAudience("test"),
      err: "token was not intended for",
    },
    "Audience does not match": {
      token: JSONToken{
        Audience: "abcd",
      },
      validator: ForAudience("test"),
      err: "token was not intended for"
    },
    "Jti does not match": {
      token: JSONToken{
        Jti: "abcd",
      },
      validator: IdentifiedBy("test"),
      err: "token was expected to be identified by",
    },
    "Issuer does not match": {
      token: JSONToken{
        Issuer: "abcd",
      },
      validator: IssuerBy("test"),
      err: "token was not issued by",
    },
    "Subject does not match": {
      token: JSONToken{
        Subject: "abcd",
      },
      validator: Subject("test"),
      err: "token was not related to subject",
    },
    "Issued in the future": {
      token: JSONToken{
        NotBefore: time.Now().Add(2 * time.Hour),
      },
      validator: ValidAt(time.Now()),
      err: "token cannot be used yet",
    },
    "time.Now < NotBefore": {
      token: JSONToken{
        Expiration: time.Now().Add(-2 * time.Hour),
      },
      validator: ValidAt(time.Now()),
      err: "token has expired",
    },
    "Expired token": {
      token: JSONToken{
        Expiration: time.Now().Add(-2 * time.Hour),
      },
      validator: ValidAt(time.Now()),
      err: "token has expired",
    },
  }
  
  for name, test := range case {
    t.Run(name, func(t *testing.T) {
      if err := test.token.Validate(test.Validator); assert.Error(t, err) {
        assert.Contains(t, err.Error(), test.err)
      }
    })
  }
}

```

```
```

```
```


