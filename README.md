# Node Passport OIDC Example for SSI-to-OIDC Bridge

> [!NOTE]
> This repository is based on a fork from [okta-node-passport-oidc-example](https://github.com/oktadev/okta-node-passport-oidc-example).

This code can be used as an example or template for an application using the [SSI-to-OIDC Bridge](https://github.com/GAIA-X4PLC-AAD/ssi-to-oidc-bridge) as an OIDC provider for logins. While the bridge should work with any OIDC client, setup and configuration can be troublesome. This repository is intended to provide a minimal working example.

**Prerequisites**

- Basic knowledge of JavaScript
- [Node.js](https://nodejs.org/en/) installed
- [SSI-to-OIDC Bridge](https://github.com/GAIA-X4PLC-AAD/ssi-to-oidc-bridge) deployed locally or remotely


## Getting Started with a Local Setup

To install this example application, run the following commands:

```shell
git clone https://github.com/jfelixh/node-passport-ssi-oidc-template.git
cd node-passport-ssi-oidc-template
```

Alternatively, create your own repository based on this one using the template functionality.

### Register an OIDC Client at the SSI-to-OIDC Bridge

Assuming you run the bridge in the provider `docker compose` configuration locally, you can run the following to register a new client. You need to replace the curly bracket placeholders with suitable values:

```shell
docker run --rm -it \
    --network ory-hydra-net \
    oryd/hydra:v2.2.0 \
    create client \
    --skip-tls-verify \
    --name {ANY NAME} \
    --secret {A SECURE CLIENT SECRET STRING} \
    --redirect-uri http://localhost:3000/authorization-code/callback \
    --token-endpoint-auth-method client_secret_post \
    -e http://hydra:4445 \
    --format json
```

The returned JSON contains a `client_id` that you will need in the next step.

Fill in the placeholder values in `app.js`:

```js
// set up passport
Issuer.discover("{SSI-to-OIDC BRIDGE BASE URL}").then((bridgeIssuer) => {
  const client = new bridgeIssuer.Client({
    client_id: "{client_id}",
    client_secret: "{CLIENT SECRET STRING}",
    redirect_uris: ["http://localhost:3000/authorization-code/callback"],
    token_endpoint_auth_method: "client_secret_post",
  });
```

### Install Dependencies and Run the App

To install the dependencies and start the app, run the following commands:

```shell
npm install
npm start
```

## License

Apache 2.0, see [LICENSE](LICENSE).
