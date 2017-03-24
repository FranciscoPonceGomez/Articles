

# Token authentication and token expiration time

## JSON Web Token (JWT) overview
JSON Web Token (JWT) is a compact token format intended for space constrained environments such as HTTP Authorization headers and URI query parameters. JWTs encode claims to be transmitted as a JSON object (as defined in RFC 4627 [RFC4627]) that is base64url encoded and digitally signed and/or encrypted. Signing is accomplished using JSON Web Signature (JWS) [JWS]. Encryption is accomplished using JSON Web Encryption (JWE) [JWE]. 
The suggested pronunciation of JWT is the same as the English word "jot".

The following example JWT Header declares that the encoded object is a JSON Web Token (JWT) and the JWT is signed using the HMAC SHA-256 algorithm:

```JSON
{"typ":"JWT",
 "alg":"HS256"}
 ```

Base64url encoding the bytes of the UTF-8 representation of the JWT Header yields this Encoded JWS Header value, which is used as the Encoded JWT Header:

eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9

The following is an example of a JWT Claims Set: 

```JSON
{"iss":"joe",
 "exp":1300819380,
 "http://example.com/is_root":true}
 ```

Base64url encoding the bytes of the UTF-8 representation of the JSON Claims Set yields this Encoded JWS Payload, which is used as the JWT Second Part (with line breaks for display purposes only):

```CSHARP
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly
9leGFtcGxlLmNvbS9pc19yb290Ijp0cnVlfQ
```

Signing the Encoded JWS Header and Encoded JWS Payload with the HMAC SHA-256 algorithm and base64url encoding the signature in the manner specified in [JWS], yields this Encoded JWS Signature, which is used as the JWT Third Part: 

```CSHARP
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

Concatenating these parts in the order Header.Second.Third with period characters between the parts yields this complete JWT (with line breaks for display purposes only):

```CSHARP
eyJ0eXAiOiJKV1QiLA0KICJhbGciOiJIUzI1NiJ9
.
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt
cGxlLmNvbS9pc19yb290Ijp0cnVlfQ
.
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```


## Authentication with the Bot Connector

The Bot Connector service uses OAuth 2.0 client credentials for bot authentication. To send messages to the Bot Connector, your bot must get an access token from the Microsoft Account (MSA) server. After getting the access token, you can include the token in the Authorization header of all requests that your bot sends to the Bot Connector.

The whole process is made in three steps:

1. Getting an access token and calling the Bot Connector Service.
2. Authenticating calls from the Bot Connector Service.
3. Enabling Authentication from the Bot Framework Emulator.

### Getting access to the token

The whole process can be done in three simple steps:

1. POST to the MSA login service.
2. Get the JWT token.
3. Use the JWT token in an authorization header.

An schema for all three steps are depicted below:

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/auth_bot_to_bot_connector.png" /> Communicatoin between the Bot and the Bot Connector</div> 

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/auth_bot_connector_to_bot.png" /> Communicatoin between Bot Connector and the Bot </div> 

<div style="text-align:center"><img src ="https://github.com/FranciscoPonceGomez/Articles/blob/master/Articles/images/auth_bot_framework_emulator_to_bot.png" /> Communicatoin between Bot Framework and Bot Coonector </div> 

For more information about the details for every step you can look here [here](https://docs.botframework.com/en-us/core-concepts/authentication/#getaccesstoken)
.


## Token expiration time and related issues in C#

The BotFramework deal with all theses problems in the backstage . Classes like [JwtTokenExtractor.cs](https://github.com/Microsoft/BotBuilder/blob/3a98a6b3d15962a57b5454bfb3f730d3588de3ef/CSharp/Library/Microsoft.Bot.Connector.Shared/JwtTokenExtractor.cs), [JwtTokenRefresher.cs](https://github.com/Microsoft/BotBuilder/blob/497252e8d9949be20baa2cebaa6ce56de04461cf/CSharp/Library/Microsoft.Bot.Connector.Shared/JwtTokenRefresher.cs) and
[BotAuthenticator.cs](https://github.com/Microsoft/BotBuilder/blob/a71e64c24bd40f8b99de0a3326ea1b79110c33e1/CSharp/Library/Microsoft.Bot.Connector.Shared/BotAuthenticator.cs)
take care of de adquisition, validation and lifecycle maintenance of the token. 

By defayult the expiration time of the token is one hour (3600 seconds). However, even though JwtTokenRefresher can find a new token before the expiration time we have detected that sometimes, after one hour of being iddle, a bot can lose its creadentials and return an unathorize error:

```MSDOS
Response status code does not indicate success: 401 (Unauthorized)
```

What the error is telling us is that our token has expired and the Bot Framework was unable to fetch a new one before that happened. Normally JwtTokenRefresher will retrieve a new one but as I said, we have experienced this error. However, we can work around this problem by simply creating a task that keeps the service alive and doesn't allow our bot to be idle while still in our current session. We can do that in many ways but we opted for a simple infinite loop with a 30 minute delay:


```CSHARP
[Serializable]
public class ActionDialog : IDialog<string>
{
   public async Task StartAsync(IDialogContext context)
   {
       Task.Factory.StartNew(() => GraphApiHelper.KeepServiceAlive());
       context.Wait(MessageReceivedAsync);
   }

    static bool isAlreadyInAliveMode = false;

    public static async Task KeepServiceAlive()
    {
        if (isAlreadyInAliveMode) return;
        isAlreadyInAliveMode = true;
        var authUrl = "https://login.microsoftonline.com/common/oauth2/v2.0/token";
        HttpResponseMessage response;
        while (true)
        {
            using(var httpClient = new HttpClient())
            {
                response = await httpClient.PostAsync(authUrl, new StringContent("credentials"));
            }
            await Task.Delay(1800); //sleep for 30  minutes
        }
    }
}
```

### References

* [JSON web token](http://openid.net/specs/draft-jones-json-web-token-07.html)
