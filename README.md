![](https://img.shields.io/badge/Build%20with%20%E2%9D%A4%EF%B8%8F-at%20Technologiesitftung%20Berlin-blue)

<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-2-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->



# grist-docker-compose
Docker-compose for grist

Instructions for integration with keycloak

## Prerequisites

Docker, Docker-compose


## Keycloak integration

After creating a Realm

 1. Go to > Clients > Create
 2. The  **Client ID**  in Keycloak should be  `https://<grist-host>/saml/metadata.xml`. The type should be SAML
 3. Save the new client
 4. Set **Valid Redirect URIs**  to  `https://<grist-host>/*` and   `https://<grist-host>/`
 5. Go to **Fine Grain SAML Endpoint Configuration** and set **Logout Service Redirect Binding URL** to `https://<grist-host>/`
 6. Save the settings
 7. Switch to the tab "Mappers"
 8. Click on **Add Builtin**
 9. Add `X500 givenName`, `X500 email` and `X500 surname`

### How to copy keys and certificates

 This part will require you to put together the key and cert files. Basically a copy/paste into the docker volume.

 1. Go to the tab **Keys** and copy the contents into files called *key.pem* and *cert.pem*
 2. Go to **Realm Settings**  > **General**  tab and click on **SAML 2.0 Identity Provider Metadata**
 3. Copy the contents of content of **ds:X509Certificate** into a file called idp.pem
 4. Add to each file the PEM headers/footers:
    -   `-----BEGIN RSA PRIVATE KEY-----`/`-----END RSA PRIVATE KEY-----`
    -   `-----BEGIN CERTIFICATE-----`/`-----END CERTIFICATE-----`
  5. Copy the files into the docker volume "saml". How you do that depends on your set up.
  6. You should have key.pem, cert.pem and ipm.pem in the folder



<!--

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

-->


## Credits

<table>
  <tr>
    <td>
      Made by <a src="https://citylab-berlin.org/de/start/">
        <br />
        <br />
        <img width="200" src="https://citylab-berlin.org/wp-content/uploads/2021/05/citylab-logo.svg" />
      </a>
    </td>
    <td>
      A project by <a src="https://www.technologiestiftung-berlin.de/">
        <br />
        <br />
        <img width="150" src="https://citylab-berlin.org/wp-content/uploads/2021/05/tsb.svg" />
      </a>
    </td>
    <td>
      Supported by <a src="https://www.berlin.de/rbmskzl/">
        <br />
        <br />
        <img width="80" src="https://citylab-berlin.org/wp-content/uploads/2021/12/B_RBmin_Skzl_Logo_DE_V_PT_RGB-300x200.png" />
      </a>
    </td>
  </tr>
</table>
