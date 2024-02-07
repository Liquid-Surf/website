# What Is FedCM?


FedCM, short for Federated Credential Management, is a new [draft specification](https://fedidcg.github.io/FedCM/) for web browsers, published by the [Federated Identity Community Group](https://www.w3.org/community/fed-id/) and strongly driven by teams from Google. It represents an advancement in how websites manage user logins, when logging in through different identity providers (such as "Sign in with GitHub/Google/etc.") while preserving user privacy. The initial motivation is to be able to use federated authentication without the use of 3rd party cookies. As described by Google:

>FedCM is a method that allows users to log into websites through federated identity services, such as "Sign in with...", without sharing personal information with either the identity service or the website.


Traditionally, when you log into a website using an external service provider (like using "Sign in with GitHub"), you are redirected to the provider's login page. This is the classic flow of federated authentication standards such as [OIDC](https://auth0.com/docs/authenticate/protocols/openid-connect-protocol) and [OAuth](https://oauth.net/2/). FedCM changes this flow by acting as an intermediary within your browser, between the website (called Relying Party or RP) and in this example GitHub (called Identity Provider or IDP). If the end-user/agent has once before logged successfully into their IDP, the browser can on subsequent visits to the RP retrieve the access token on behalf of the user without the need of a redirect to the IDP. We all know these *weird* hops from the RP to the IDP and back. No more.

In the following example Alice has logged into her IDP beforehand and is not visiting the RP again, where the session expired. Time for a new token.

| ![Federated authentication without FedCM](https://raw.githubusercontent.com/thhck/assets/main/without_fedcm.drawio.svg) | ![Federated authentication with FedCM](https://raw.githubusercontent.com/thhck/assets/main/with_fedcm.drawio.svg)
| -- | --
| Without FedCM | With FedCM |


In practice, FedCM works through a JavaScript API that websites can use. This means when you click a "Sign In with..." button on a website, instead of taking you to another page, the FedCM API in your browser springs into action. It helps you log in without leaving the website you're on. This new method is not only more convenient but also keeps your personal information more private, as it reduces the amount of data shared directly between you, the website, and the external service provider.


[FedCM demo](https://storage.googleapis.com/web-dev-uploads/video/vgdbNJBYHma2o62ZqYmcnkq3j0o1/TJLjWp1nVLlDMMCK2ugQ.mov)

## Federated Authentication Is Important for a Decentralized Web

Federated identity systems plus the introduced credential management in the browser offer a significant convenience. Improved user experience by getting rid of redirects to the IDP and remembering the previously used IDPs. Essentially, making it easier for the end-user to re-use their existing federated identity. All in all leading to a much smoother proof of identity on the web. However, when only a handful of major companies control the gateways to digital identity, it leads to a centralization of power and data, which poses a challenge to a truly decentralized web. 

<!-- [](https://upload.wikimedia.org/wikipedia/commons/8/8a/Screenshot_of_Ajapaik_rephoto_app_login_view.jpg) -->

This concentration of control not only stifles innovation and competition from smaller, independent identity providers but also raises concerns about privacy, data security, and user autonomy. In a truly decentralized web, users should have the freedom to choose from a diverse array of IDPs, promoting an environment where privacy, security, and user empowerment are paramount. Breaking this monopoly is not just about diversifying options; it's a crucial step towards realizing the full potential of a decentralized, open, and diverse internet ecosystem.


## Current Implementation of FedCM: a Challenge for Small IDPs

The present implementation of FedCM requires Relying Parties to offer a predefined list of IDPs through the browser's FedCM API, a practice mirroring the current trend in federated identification. Typically, this list is dominated by major tech companies like the usual Sign in with Facebook, Google, and GitHub. This preference arises as RPs often choose the most famous IDPs for their wider user base and perceived reliability. While this approach aligns with what users and developers are accustomed to, it perpetuates a significant drawback: the marginalization of smaller and independent IDPs. RPs tend to prioritize popular and widely used IDPs, inadvertently sustaining the tech giants' monopoly. Consequently, smaller and independent IDPs face challenges in gaining visibility and usage, overshadowed by the more established names often used by RPs.



## Promoting Equality: A Proposed FedCM Model Favoring Diverse IDPs

A more equitable implementation of FedCM could significantly benefit other IDPs. In this alternative implementation, the browser would maintain a list of IDPs that a user has previously registered with or signed into. This list gets populated as IDPs self-register with the browser during user registration or sign-in processes. In addition to relying on a hardcoded list of IDPs, Relying Parties (RPs) could also ask for registered IDPs when calling the FedCM API. This change allows the browser to present its own list of IDPs, which includes a broader range of providers. Such a model levels the playing field for other IDPs, enhancing user autonomy and fostering a more diverse identity ecosystem. It represents an important step away from the dominance of major tech companies in the realm of digital identities.

![image5](https://hackmd.io/_uploads/SJnEeXWjp.jpg)

## Benefits for the Decentralized Community

This FedCM implementation offers key advantages for the decentralized web.

 - Broad IDP Support with Great UX: It enables Relying Parties  to offer a wider range of Identity Providers while maintaining a smooth user experience. This approach allows for more IDP options without overwhelming the user.
 - Opening Doors for Smaller IDPs: The model gives non-big tech IDPs a fighting chance to be utilized by RPs, breaking the monopoly of major tech companies and fostering diversity and innovation in digital identity management.


## Call to Action

While FedCM is a step in the right direction, its current specs need to be push a bit further to stride towards a more balanced and diverse web. The specs are willing to go in this direction but don't want to develop and maintain a feature if it is not used [1](https://github.com/fedidcg/FedCM/issues/240#issuecomment-1783887988). If you believe this implementation of FedCM could be usefull for your community/tech/app please join our discussion [here](https://github.com/fedidcg/FedCM/issues/240)
