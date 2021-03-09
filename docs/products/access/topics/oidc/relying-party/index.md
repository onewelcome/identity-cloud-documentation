# Relying party examples

An application that is depending on the OpenID Connect Provider (OP) is called a Relying Party (RP). Here are a few examples that have been tested against our
OP and some minor tweaks that need to be made to work properly.

## Node.js

Clone this repo: https://github.com/mcguinness/oidc-rp

Install npm dependencies if needed.

`npm install`

You can run the application in the console using the following command:

`node server.js --iss [issuerUrl] --cid [ClientId] --cs [ClientSecret] --scp "openid profile" --responseType code`

Full example:

```shell
git clone https://github.com/mcguinness/oidc-rp.git
cd oidc-rp
npm install
node server.js --iss http://localhost:7878/oauth --cid openid-client --cs secret --scp "openid profile" --responseType code
```

Once the console shows that the endpoints' discovery was successful go to
[http://localhost:7080](http://localhost:7080) and you should get redirected to the authentication endpoint.

## Java with Spring Framework

Please refer to our [Java Spring example](https://github.com/Onegini/java-spring-oidc-example) project for the code and configuration details.

## ASP.NET Core

Please refer to our [ASP.NET core example](https://github.com/Onegini/dotnet-mvc-oidc-example) project for the code and configuration details.