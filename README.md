# Hosting apple-app-site-association files

This is simply a repository to host AASA files. See [Apple's documentation](https://developer.apple.com/documentation/xcode/supporting-associated-domains
) for more information

## Set up

You will need to control a domain name, i.e. to add CNAME records to your domain's DNS zone.

Assuming

- your GitHug username is `<handle>`
- your Bundle ID is com.example.app
- your Team ID is UVWXYZ1234
- your FQDN is rp.example.com

perform the following steps:

- Clone this repository

    git clone git@github.com:joostd/apple-app-site-association.git

- Save your FQDN in the CNAME file

    echo rp.example.com > CNAME

- Enable Pages in your repository's Settings (select the root folder)

    https://github.com/<handle>/apple-app-site-association/settings/pages

- Add a CNAME record for your FQDN

    example.com CNAME <handle>.github.io

- Save your AppID in the AASA file

    echo '{"webcredentials":{"apps":["UVWXYZ1234.com.example.app"]}}' > .well-known/apple-app-site-association

## Test

Use a test site to check the result. For instance:

    [https://branch.io/resources/aasa-validator/](https://branch.io/resources/aasa-validator/)

Note that DNS records are cached (also negative results!), so you may need to wait for DNS changes to propagate.

    $ dig rp.example.com cname +short
    <handle>.github.io

