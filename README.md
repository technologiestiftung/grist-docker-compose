# grist-docker-compose
Docker-compose for grist


Temporary instructions for integration with keycloak


I just got it working with Keycloak 17. It's a complicated beast that supports multiple multiple realms and protocols. Some things were not obvious to me (as a noob to Grist, Keycloak, and SAML)...

I already had a realm and user, so I only needed to create a Grist-specific SAML client:

-   The  **Client ID**  in Keycloak should be  `https://<grist-host>/saml/metadata.xml`
-   Keycloak needs to know where to redirect after login/logout
    -   Allow redirecting after login by setting the  **Valid Redirect URIs**  to  `https://<grist-host>/*`
    -   Enable redirecting after logout by setting the  **Logout Service Redirect Binding URL**  (under  **Fine Grain SAML Endpoint Configuration**)
-   Protocol mappers are needed; the builtin mappers work, but their SAML attribute names should be changed to work with  [SAML2-js](https://github.com/Clever/saml2):
    -   **givenName:**  `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`
    -   **surname:**  `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`
    -   **email:**  `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

Grist needs the following information from Keycloak:

-   The SAML login/logout URL in Keycloak 17 is  `https://<keycloak-host>/realms/<realm>/protocol/saml`  (in Keycloak 16, it would've been  `https://<keycloak-host>/auth/realms/<realm>/protocol/saml`; note the  `/auth`)
-   The client's private key and certificate could be obtained from the  **Installation**  tab (on the client page)
-   Keycloak's server (realm) certificate could be obtained from  **Realm Settings**  in the  **General**  tab behind  **SAML 2.0 Identity Provider Metadata**
-   These keys and certificates should be placed in files accessible to Grist (as indicated in  [SamlConfig.ts](https://github.com/gristlabs/grist-core/blob/main/app/server/lib/SamlConfig.ts)) with appropriate PEM headers/footers:
    -   `-----BEGIN RSA PRIVATE KEY-----`/`-----END RSA PRIVATE KEY-----`
    -   `-----BEGIN CERTIFICATE-----`/`-----END CERTIFICATE-----`

Here's how I start Grist (behind a reverse-proxy):
