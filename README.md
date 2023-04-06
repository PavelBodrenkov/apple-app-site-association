# Hosting apple-app-site-association files

This is simply a repository to host AASA files. See [Apple's documentation](https://developer.apple.com/documentation/xcode/supporting-associated-domains
) for more information

## Set up

You will need to control a domain name in order to add CNAME records to your domain's DNS zone.

Assuming

- your GitHug username is `<handle>`
- your Bundle ID is `com.example.app`
- your Team ID is `UVWXYZ1234`
- your FQDN is `rp.example.com`

perform the following steps:

1. Make a clone of [this repo](https://github.com/joostd/apple-app-site-association) on GitHub.

2. Edit the [CNAME](CNAME) file to point to your FQDN (e.g. `rp.example.com`).

3. Visit your Domain Registrar and add a `CNAME` record for your FQDN, for instance `example.com CNAME <handle>.github.io`

4. Enable _Pages_ in your repository's Settings (select the _main_ branch and the _root_ folder), See `https://github.com/<handle>/apple-app-site-association/settings/pages`

5. Wait for a while until GitHub generates a certificate for your Pages site.
    
6. Save your AppID in the AASA file, i.e. `apple-app-site-association` in the `.well-known` directory. For instance `{"webcredentials":{"apps":["UVWXYZ1234.com.example.app"]}}`
    
## Test

Your AASA file should be available on the URL
    
    https://rp.example.com/.well-known/apple-app-site-association
    
Use a test site to check the result. For instance [https://branch.io/resources/aasa-validator/](https://branch.io/resources/aasa-validator/)

Note that DNS records are cached (also negative results!), so you may need to wait for DNS changes to propagate.

Use a DNS debug site or on MacOS/Linux, open a terminal and check with:
    
```bash
$ dig rp.example.com cname +short
<handle>.github.io
```
