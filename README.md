# Vault Code Signing

_Keyless code signing using Hashicorp Vault as a code signing certificate authority_

Sigstore's keyless code signing is quickly gaining traction because of how easy
it is to use. The important aspects of keyless signing are

- OIDC ID token are used to request a certificate
- The certificate have a _very_ short TTL
- Signature transparency

Because OIDC identity providers are ubiquitous its easy to get an ID token. For
humans, many identities providers like Google, Microsoft or Github are either
already an OIDC compliant identity provider or can be made so with a federating
identity provider like [Dex](), [Keycloak]() or [Auth0](). Its also getting
easier for workloads to access ID tokens. Continuous integration providers like
CircleCI and Github Actions are now injecting job specific ID tokens into
builds. Google Cloud service accounts are already using ID tokens for identity.

The combination of short lived certificates and signature transparency allow us
to request a certificate on-demand when we want to sign something and throw the
private key away immediately. Signature transparency involves uploading the
signature, hash of the artifact signed and code signing certificate to a
transparency log like [Rekor](). This frees us from the burden of needing the
certificate to be valid at the time of validation. Instead, we need certificate
needs to be valid at the time of signature and this time is recorded by the
signature transparency log.

## Hashicorp Vault
