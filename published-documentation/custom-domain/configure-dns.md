# Configuring DNS

{% hint style="info" %}
Please follow the steps to set a custom domain in this order:

1. [Choosing a subdomain](choose.md)
2. [Deciding where to set the custom domain](location.md)
3. [Initiating the custom domain setup](initiate/) (at the [organization](initiate/organization-level-custom-domain.md), [collection](initiate/collection-level-custom-domain.md), or [space](initiate/space-level-custom-domain.md) level)
4. [**Configuring DNS**](configure-dns.md) **(you are here)**
5. [Confirming the custom domain setup](finalize.md)
{% endhint %}

Configuring DNS happens _outside_ of GitBook, at the DNS provider you are using for your domain.

There are three parts to this step:

1. [Configure a CNAME record](configure-dns.md#configure-a-cname-record)
2. [Check for a CAA record](configure-dns.md#check-for-a-caa-record)
3. [Wait for the changes to take effect](configure-dns.md#wait-for-the-changes-to-take-effect)

## Configure a CNAME record

The names of the fields and what to actually enter to configure the record may differ between DNS control panels, but we’ve covered the most common options here. If you’re in any doubt, check with your DNS provider.

* The **type** is the kind of DNS record that you want to create. Here, you need to choose **CNAME**.
* The **name** or **DNS entry** is where you enter your subdomain. You might need to enter it in full (e.g. **docs.example.com**) or you might just need to enter the part before your apex domain (e.g. **docs**). If you’re not sure which to use, check with your DNS provider.
* The **target** or **value** or **destination** is where the subdomain should be pointed.

You might also see a field named **TTL**, which stands for Time To Live. It’s the number of seconds that the DNS record can be cached for. If you’re not sure what to set, look at the TTL for your existing DNS records. You could set the same number. If you’re still not sure, we suggest setting 43200 seconds (12 hours) or 86400 seconds (24 hours).

Here’s an example of how a correct configuration looks in Cloudflare’s control panel:

![A properly configured custom domain in Cloudflare’s control panel](<../../.gitbook/assets/Screenshot 2022-04-11 at 16.53.56.png>)

{% hint style="warning" %}
**Note:** a CNAME record cannot co-exist with another record for the same name. If you already have an A record, AAAA record, TXT record, or any other type of record for your chosen subdomain, you would need to remove those first, _before_ adding the CNAME record.
{% endhint %}

### Are you using Cloudflare?

If you are configuring DNS in Cloudflare’s control panel, please ensure that Cloudflare’s proxying (the orange cloud, also called "Proxy status" in your domain settings) is **disabled**. This is for two reasons:

1. This option obfuscates the DNS target for your domain to the public, preventing GitBook from properly running routine checks on your custom domain.
2. Your custom domain will already benefit from Cloudflare’s CDN and a Google Trust Services SSL certificate on our end.

Again, please **turn off Cloudflare proxying** to ensure that your documentation is served without issues and can be monitored by GitBook.

{% hint style="warning" %}
**GitBook does not officially support proxy setups**&#x20;

If you decide to set it up anyway, please ensure your setup respects the following:

* You need to proxy all `GET`, `OPTIONS`, `HEAD`, `POST` methods.
* You should respect the `Cache-Control` header and use the entire request (url + headers, by respecting the `Vary` header) as a cache key.
* You should not modify the HTML to load external resources, or you should take the CSP into consideration and ensure a [`nonce`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global\_attributes/nonce) is set on every resources.
{% endhint %}

## Check for a CAA record

{% hint style="info" %}
The short answer: either don’t have a CAA record, or have one that explicitly allows Google Trust Services.
{% endhint %}

CAA records enable you to specify who can issue an SSL certificate for the domains that you own. We use Google Trust Services to issue an SSL certificate for your custom domain, so this needs to be allowed. There are two ways to do this.

1. Have no CAA record. Without a CAA record, there are no limitations on which SSL providers are allowed to issue an SSL certificate for your domain.
2. Have a CAA record that explicitly allows Google Trust Services. If a CAA record exists, any providers that are not explicitly allowed will be blocked. The following is the value that would need to be included in a CAA record to explicitly allow Google Trust Services:

```
0 issue "pki.goog"
```

## Wait for the changes to take effect

{% hint style="info" %}
The short answer: you might need to wait 1-48 hours for the DNS changes to take effect before moving onto the next step.
{% endhint %}

Remember the TTL (Time To Live) field we mentioned earlier? DNS records are cached for a period of time — which is usually a very good thing for performance reasons, because they typically don’t change very often. When they _do_ change, there is a period of time (the TTL value) where DNS cache servers need their cache to expire before they will check for any changes and behave accordingly.

In most cases, it’s best to allow at least an hour before moving onto the next and final step. Sometimes it could all update a bit more quickly, or it could take longer. It’s rare for this to take longer than 48 hours.

Want to check how this process, known as _propagation_, is progressing? You could use a DNS lookup tool, such as [WhatsMyDNS](https://www.whatsmydns.net/). Enter your full subdomain, select CNAME from the dropdown list, and press the Search button. DNS cache servers around the world will respond to let you know what their cached result is. You’ll want to periodically check these results until the vast majority respond with your assigned CNAME value.

Once DNS propagation has completed, you can move onto the last step: [confirming the custom domain setup](finalize.md).
