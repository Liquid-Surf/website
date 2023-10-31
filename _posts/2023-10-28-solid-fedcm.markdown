---
layout: post
title: "FedCM and BYOIDP"
# categories: news
author: jan
published: true
date: 2023-10-06
---

## Federated Credential Management

>The Federated Credential Management API (or FedCM API) provides a standard mechanism for identity providers (IdPs) to make identity federation services available on the web in a privacy-preserving way, without the need for third-party cookies and redirects. This includes a JavaScript API that enables the use of federated authentication for activities such as signing in or signing up on a website.

An end-user using a web app with FedCM will not have to be redirected to their IdP in order to receive its access token, but can use a new native browser UI to log in. The need for `<iframe>`, redirects, and third-party cookies to enable identity federation becomes obsolete, a win in user-privacy and user-experience.

## Goal

FedCM currently supports logging in a user-agent when the IdP is _known_. This means the RP needs to define a list of supported IdPs that it then passes to the FedCM API. The goal for this demo is to provide a _bring your own identity provider_ functionality, so that when querying FedCM with the `get` call, it will find all registered (previously logged in to) IdPs and return the list to the user. This way the RP does not need to provide a list of IdPs it wants to support but can simply allow this _wildcard_ approach.

## Demo

Clone the demo repositories.

```sh
git clone git@github.com:Liquid-Surf/fedcm-rp-node.git
git clone git@github.com:Liquid-Surf/fedcm-idp-typescript.git
```

Follow the instructions to run the RP and IdP. For this demo it is needed to run multiple IdPs.

Mapping the IdPs in your host file allows the FedCM demo to run multiple IdPs on localhost.

```sh
sudo tee -a /etc/hosts > /dev/null <<EOT
127.0.0.1       idp-1.localhost
127.0.0.1       idp-2.localhost
127.0.0.1       idp-3.localhost
EOT
```

Now we want to inject our own version of the `navigator.credentials.get` function that supports querying for registered IdPs. For this we use [Tampermonkey](https://github.com/Tampermonkey/tampermonkey) (a browser extension should also work) and `json-server` to keep track of registered IdPs.

```sh
npm install -g json-server
echo '{ "providers": { "urls": [] } }' >> db.json
json-server --watch db.json
```

The IdPs will post their hostname to this `db.json` file when a user logs in.

This script will expose our updated get method and expose it on `_navigator`. This will be called instead of the real `get` in the RP and injected using Tampermonkey.

```js
(async function () {
    'use strict';

    // Fetch the registered IdPs from the fake FedCM Store/JSON server
    const response = await fetch("http://localhost:3000/providers");
    const providerUrls = await res.json();

    // Extract the URL data and create registered providers
    const providersFromFakeFedCMStore = idpUrls.urls.map(url => ({
        nonce: "not-a-nonce",
        configURL: `http://${url}:8080/fedcm.json`,
        clientId: "yourClientID"
    }));

    function getCredentials(identity) {
        const providersFromRP = identity.providers;
        const providers = identity.registered ? providersFromFakeFedCMStore.concat(providersFromRP) : providersFromRP;

        return navigator.credentials.get({
            identity: { providers }
        });
    }

    window._navigator = {
        credentials: { get: getCredentials }
    };
})();
```

### Flow

1. Register and sign into the IdP on idp-1.localhost:8080
2. Register and sign into the IdP on idp-2.localhost:8080
3. Go to the RP at localhost:7080
4. Press Login
5. This should prompt you now with the option to continue with both IdPs, even though the RP only knew about idp-1 and queried the fake FedCM storage for idp-2.

## References

- <https://developer.mozilla.org/en-US/docs/Web/API/FedCM_API>
- <https://github.com/typicode/json-server>
- <https://github.com/Tampermonkey/tampermonkey>
