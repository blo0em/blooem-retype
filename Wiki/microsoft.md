---
icon: link
---
![](../static/e5custom.png)

# Microsoft E5 Custom Domain
Get your shiny custom domain for your E5 developer account.

## Steps:

!!!warning Make sure you have your E5 developer account set up.
If you haven't then follow this guide: [E5 Guide](https://rentry.co/E5Register)
!!!

### 1. Get your domain
- Go to this [website](https://nic.eu.org/arf/en/login/) and sign up.
- Click `New Domain`and fill up the domain name you want. Example: `something.eu.org`
- Check `server names` for the `Check for correctness of:` and set `NS1.HE.NET` for Name1, `NS2.HE.NET` for Name2, and accordingly for `NS3.HE.NET, NS4.HE.NET, NS5.HE.NET`. 
!!!We will be changing this to cloudflare nameservers so it won't actually matter what you put here as long as they are valid nameservers.
!!!
- Wait for 24hours (i think), or few days to get the domain registered. You will receive an email when that happens.


### 2. Cloudflare Setup
- Add your website to cloudflare and change your nameservers to the ones provided by cloudflare.
- It might take an hour or so for your domain to appear in cloudflare.

### 3. Mircosoft Admin Center
- Go to your admin center and to this page: [Manage Domains](https://admin.microsoft.com/Adminportal/Home#/Domains)
- Click `Add a new domain` and add your new `something.eu.org` domain there.
- It will now ask you to verify your domain. Choose the first option aka `Add a TXT record to the domain's DNS records`.

### 4. Back to cloudflare
- Go to DNS management for your website. Add a new new record with `TXT` as type, `@` as name and `MS=ms########` (unique ID from the admin center) as your content. Set TTL to `30min`.

### 5. Validate your TXT Record
- You can use this [website](https://www.kitterman.com/spf/validate.html?) to validate if TXT record has been updated.
- After adding those records, click verify and it should do so without errors.

### 6. More DNS setup

!!!danger The instruction on the microsoft page can be misleading/confusing. So follow these instructions.
Assuming my domain is `something.eu.org`, the page will ask you to add `something` as your host name, but literally adding `something`   will give you error like `We didn't detect that you added this record.` We are supposed to use `@` for this case in the host name.
!!!

=== MX Record
Go to the DNS management and add a new record. Use `MX` as type, `@` as name, and copy the "Points to address or value" value in your microsoft page and use it as content. Set `0` for priority and `1hr` for TTL.
===

=== CNAME Record
With `CNAME` as type for a new record, set the `autodiscover` as your name, `autodiscover.outlook.com` as your target and `1hr` for TTL. 
!!!This value is same for everyone.
!!!
!!!warning Make sure to set the `Proxy Status` as `DNS only`.
!!!

===

=== TXT Record
Add a new record. Use `TXT` as type, `@` as name, and copy the TXT value  (it should be `v=spf1 include:spf.protection.outlook.com -all`) in your microsoft page and use it as content. Set TTL to `1hr`.
===

### 7. Done
- Enjoy using your custom domain.
