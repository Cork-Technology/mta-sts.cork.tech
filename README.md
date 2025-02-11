<h1 align="center">
  <br>
  📩 A Template to host an MTA-STS Policy file on GitHub
  <br>
</h1>

<h4 align="center">Use this template to host your <i>MTA Strict Transport Security (MTA-STS)</i> <a href="https://datatracker.ietf.org/doc/html/rfc8461">[RFC 8461]</a> policy file on GitHub Pages.</h4>

<p align="center">
  <a href="#how-to-use">How To Use</a> •
  <a href="#license">License</a>
</p>

MTA-STS is a security standard to secure e-mail delivery. E-mail servers that send inbound e-mail to your domain will be able to detect that your e-mail server supports SMTP-over-TLS via `STARTTLS` (also known as [Opportunistic TLS](https://en.wikipedia.org/wiki/Opportunistic_TLS)) before opening the actual connection.

In case the sending e-mail server is not able to initiate a secure connection, it will end the connection to enforce transport layer encryption. This mitigates [Man-in-the-middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) DNS and SMTP [downgrade attacks](https://en.wikipedia.org/wiki/Downgrade_attack) that would allow an attacker to read or manipulate e-mail in transit.

## How To Use

1. Make sure you are [signed in to GitHub](https://github.com/login). Then click on [**Use this template**](https://github.com/jpawlowski/mta-sts.template/generate) to create a copy to your own GitHub profile (see [GitHub Docs](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)). Don't _clone_ the repository.
   You may name your repository whatever you like. For simplicity, you can name it `mta-sts.<your_domain.tld>`.

2. Change the file `.well-known/mta-sts.txt` according to your needs.

3. Create a `CNAME` record for `mta-sts.<your_domain.tld>` in your domain's DNS that points to `<your_username>.github.io` or `<your_organization>.github.io` and [enable GitHub Pages](https://docs.github.com/articles/using-a-custom-domain-with-github-pages/).

4. Open a browser to `https://mta-sts.<your_domain.tld>` and make sure it does not show any certificate warnings.

5. Create a `TXT` record for `_mta-sts.<your_domain.tld>` in your domain's DNS to enable the MTA-STS policy for your domain.

   You may copy & paste this to your DNS provider:

   ```dns
   #HOST       #TTL    #TYPE    #VALUE
   _mta-sts    3600    TXT      "v=STSv1; id=20220317000000Z"
   ```

   **Note that you will need to change the `id=` here whenever you make changes to your `mta-sts.txt` policy file.**

6. Validate your setup, for example by using the [MTA-STS Lookup by MXToolBox](https://mxtoolbox.com/mta-sts.aspx), or looking into your [Hardenize Public Report](https://www.hardenize.com/).

_Optional (but **highly recommended**):_

7. Create another `TXT` record for `_smtp._tls.<your_domain.tld>` in your domain's DNS to enable reporting (see [RFC 8460](https://datatracker.ietf.org/doc/html/rfc8460)).
   You may copy & paste this to your DNS provider:

   ```dns
   #HOST         #TTL    #TYPE    #VALUE
   _smtp._tls    3600    TXT      "v=TLSRPTv1; rua=mailto:tls-rua@mailcheck.<your_domain.tld>"
   ```

   Note that the e-mail recipient mailbox shall be on a different domain _without_ MTA-STS being configured. This could be a subdomain like `mailcheck.<your_domain.tld>`.
   It is also quite painful to manually deal with the reports other e-mail providers will send to you. For that particular reason, you may want to consider sending these e-mails to a 3rd-party tool like [Report URI](https://report-uri.com/), [URIports](https://www.uriports.com/), or from other commercial providers.

   You probably want this to be the same tool you might use for DMARC reports, like [DMARC Analyzer](https://www.dmarcanalyzer.com/) or [Dmarcian](https://dmarcian.com/).

## License

[MIT License](https://github.com/jpawlowski/mta-sts.template/blob/gh-pages/LICENSE)


# mta-sts-template

This templated repository automatically deploys a GitHub Pages site for hosting a `mta-sts.txt` file.
You should be configuring a `mta-sts.txt` deployment for every domain you recieve emails with.

When [using this template](https://github.com/new?template_name=mta-sts-template&template_owner=co-cddo) you need to set the new name to the mta-sts fully qualified domain name, like `mta-sts.gc3.security.gov.uk`, this is to ensure the auto-discovery and deployment of Pages works appropriately. You can alternatively set the `MTASTS_DOMAIN` environment variable in the workflow.

## Steps
1. Publish a TLS-RPT record, like `_smtp._tls 300 TXT "v=TLSRPTv1;rua=mailto:tls-rua@mailcheck.service.ncsc.gov.uk"`
2. Use [this template](https://github.com/new?template_name=mta-sts-template&template_owner=co-cddo), making sure to set the new repository name to the full mta-sts domain, like `mta-sts.gc3.security.gov.uk`
3. Observe the [Actions](../../actions) to make sure [configure.yml](../../actions/workflows/configure.yml) and [gh-pages.yml](../../actions/workflows/gh-pages.yml) deploy correctly
    - Make sure the build and deployment source is set to GitHub Actions in [Settings → Pages](../../settings/pages)
    - You may need to select the `main` branch and `/ root` in [Settings → Pages](../../settings/pages)
5. Configure [your DNS to point to GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site)
    - If deploying in [co-cddo](https://github.com/co-cddo), use the CNAME `co-cddo.github.io` (`mta-sts 60 CNAME co-cddo.github.io.`)
6. Check the `Custom domain` in [Settings → Pages](../../settings/pages) and ensure `Enforce HTTPS` is checked (this can take a few hours)
7. Check your deployment by visiting the domain, where you should get automatically redirected to `/.well-known/mta-sts.txt` (e.g. <https://mta-sts.gc3.security.gov.uk>)
8. Set your `_mta-sts` TXT record, like `_mta-sts 60 TXT "v=STSv1; id=20240215"` (where the id value is set to the current date, you'll need to change this if `mta-sts.txt` is updated)

## More information
You can find more about MTA-STS here: 
- https://www.security.gov.uk/guidance/email-guidance/mta-sts/
- https://www.ncsc.gov.uk/collection/email-security-and-anti-spoofing/using-mta-sts-to-protect-the-privacy-of-your-emails
