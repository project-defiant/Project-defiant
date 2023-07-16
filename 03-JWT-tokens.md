self contained tokens (stateless tokens)

Example of basic JSON token is
```{json}
{
	"sub": "alice",
	"exp": 123456,
	"attrs": {}
}
```

When sending data over the internet, we need to encode it into `base64` encoding. In the case of the JSON, it needs to be converted to bytes and then encoded to `base64`.

User sends encoded JSON token to the server, then JSON tokens are returned from the server to the client when they post the authentication. Then that can be resend from the user to the server with `Autorization: Bearer $TOKEN` header.

```{bash}
curl -H 'Autorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{"name":"some_val","owner":"test"}' http://localhost:7000/spaces
```

To ensure the security of the JSON tokens we use embed the message authentication code into the JWT.

## Message authentication code

HMAC - message authentication code 

![HMAC - how it works?](https://github.com/PROJECT-DEFIANT/Project-defiant/blob/main/images/HMAC.png?raw=true)

In this case when we first log in, we send our JWT token to the server, which uses the token to JWT token to generate a HMAC (message authentication code). HMAC is send back to the user to reuse in the authentication process.

Message authentication code generation need to use random 32 bytes key (back end stored) which is used to generate the HMAC.

To generate the key for HMAC algorithm you can use
```{bash}
keytool -keystore keystore.p12 -genseckey -alias hmac-key-2 -keyalg HmacSHA256
```

It is basically 32 bytes encoded in the `base64` so you can also use

```{bash}
head -c32 ?dev/urandom | base64
```

We need to make sure that the comparison between incoming MAC and original MAC needs to be of constant time


### JSON web token

![[Pasted image 20230711130341.png]]
![JWT](https://github.com/PROJECT-DEFIANT/Project-defiant/blob/main/images/HMAC2.png?raw=true)

Json web token consists of 
* header
* claims 
* HMAC tag

But more on that later.

The materials can be found in the https://learning.oreilly.com/library/view/api-security-in/9781617296024/
  
