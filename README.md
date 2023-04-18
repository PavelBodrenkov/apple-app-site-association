# Hosting apple-app-site-association files

This is simply a repository to host Apple App Site Association (AASA) files during development of an iOS app.
See [Apple's documentation](https://developer.apple.com/documentation/xcode/supporting-associated-domains) for more information on AASA.

## Set up

You will need to control a domain name in order to add CNAME records to your domain's DNS zone.

Assuming

- your GitHub username is `<handle>`
- your Bundle ID is `com.example.app`
- your Team ID is `UVWXYZ1234`
- your FQDN is `rp.example.com`

perform the following steps:

1. Fork [this repo](https://github.com/joostd/apple-app-site-association) on GitHub.

2. Edit the [CNAME](CNAME) file to point to your FQDN (e.g. `rp.example.com`).

3. Visit your Domain Registrar and add a `CNAME` record with a short TTL for your FQDN, for instance `example.com 300 IN CNAME <handle>.github.io`

4. Enable _Pages_ in your repository's Settings (select the _main_ branch and the _root_ folder), and tick _Enforce HTTPS_. See `https://github.com/<handle>/apple-app-site-association/settings/pages`.

5. Wait for a while until GitHub generates a certificate for your Pages site.
    
6. Save your AppID in the AASA file, i.e. `apple-app-site-association` in the `.well-known` directory. For instance `{"webcredentials":{"apps":["UVWXYZ1234.com.example.app"]}}`
    
## Test

Your AASA file should be available on the URL
    
    https://rp.example.com/.well-known/apple-app-site-association
    
## Troubleshooting

When you receive a message similar to

```
[Authorization] ASAuthorizationController credential request failed with error: Error Domain=com.apple.AuthenticationServices.AuthorizationError Code=1004 "(null)"
Error: ["NSLocalizedFailureReason": Application with identifier UVWXYZ1234.com.example.app is not associated with domain rp.example.com]
```
there is a problem with your AASA.

Below are some tips for troubleshooting.

### Check your DNS

Note that DNS records are cached (also negative results!), so you may need to wait for DNS changes to propagate.

Use a DNS debug site such as [nslookup.io](https://nslookup.io/) or on MacOS/Linux, open a terminal and check with:

```bash
$ dig rp.example.com cname +short
<handle>.github.io
```

### Check your AASA file contents

You can use a test site to check the result. For instance [https://branch.io/resources/aasa-validator/](https://branch.io/resources/aasa-validator/)

Note that it may complain that "the JSON format seems Invalid", which probably means it is expecting an HTTP response with a `Content-Type` of `application/json`, where GitHub returns `application/octet-stream` in its response due to the lack of the `.json` file name extension. However,
[according to Apple's Garrett Davidson](https://developer.apple.com/forums/thread/726678?answerId=747975022#747975022), this is not a problem.

### Check Apple's CDN

Unless you are using developer mode, your AASA file will be cached in Apple's CDN. To check whether there is a difference between the contents of the AASA file you are hosting,
and the version  hosted in Apple's CDN, check the URL

    https://app-site-association.cdn-apple.com/a/v1/rp.example.com

If there is a difference, wait for the CDN to sync with your version, or use developer mode to bypass Apple's CDN.
