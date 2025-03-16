---
title: Bridging the User Adoption Gap through SSO authentification
---


As a developer, I've encountered a dilemma with Solid:

On one hand, I genuinely believe Solid represents the web we truly want—a web where users fully own and control their data.

On the other hand, as an app developer, I also want my application to succeed and reach a wide audience. Currently, achieving both objectives simultaneously feels difficult. If I build an app using Solid, attracting users becomes challenging because the number of users who have WebIDs and Pods is limited. Yet, if I build a non-Solid app, I miss out on the valuable data ownership and privacy features that Solid offers.

Today, to onboard a typical user to my Solid app, I have to:

 - Explain that they need a Pod, creating a pedagogical barrier.

 - Guide them in selecting a Pod provider—difficult when users don't fully understand Pods yet.

 - Have them create an account on the Pod ( which can be cumbersome at the moment,  especially with CSS)

 - Ask users to return to my app, remember their Solid instance URL, and manually paste it into the login field (an issue potentially solvable with FedCM in the future).

This complexity represents a significant barrier that we aim to overcome.

Solid currently appears mature enough, with sufficient tooling available to build good apps, even if some of them are still in early stages. However, it's challenging to feel motivated to build apps with limited potential users. This problem might be the root of Solid's chicken and egg  issue: the ecosystem isn't growing due to a lack of users, and there aren't many users because the ecosystem is underdeveloped.

To address this, we've developed the [css-direct-sso-auth](https://github.com/liquid-surf/css-direct-sso-auth) module for CSS. This module enables using a CSS instance as a backend for your app, allowing users to log in using their preferred SSO providers, such as GitHub or Google. Upon login, the module automatically creates a Pod and WebID behind the scenes, removing friction from account setup, Pod creation, and WebID generation. Existing Solid users can seamlessly continue to use their existing Pods.

The module today include Github and Google authentication. Google is based on OIDC and github on OAuth. By covering those two cases, it should be easy to add other SSO, either if there based on OAuth or OIDC, by copy-pasting the `config/github-setup.json` or `config/google-setup.json` respectively.

One might argue that using big tech SSOs contradicts Solid's philosophy. However, integrating popular SSO logins is, unfortunately, necessary to be close to what average users are familiar with.

From a developer's perspective, all users effectively have Pods (either externally hosted or internally generated). This doesn't introduce additional complexity to the developer experience: you can manage all users seamlessly using your preferred Solid libraries. The only change is that now their can add an additional "Login with ..." option alongside "Login with Solid."


By enabling hybrid applications compatible with both Solid and non-Solid users, we hope to significantly enrich and expand the Solid ecosystem.

I looking forward to ear your feedback on this, the source code and issue tracker is at:

 - [https://github.com/Liquid-Surf/css-direct-sso-auth](https://github.com/Liquid-Surf/css-direct-sso-auth)

Live demo is available at:

 - [https://sso-client.liquid.surf](https://sso-client.liquid.surf)
