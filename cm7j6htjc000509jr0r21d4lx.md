---
title: "Certificate Transparency in Firefox: A Big Step for Web Security"
seoTitle: "Certificate Transparency in Firefox: A Big Step for Web Security"
datePublished: Mon Feb 24 2025 14:54:12 GMT+0000 (Coordinated Universal Time)
cuid: cm7j6htjc000509jr0r21d4lx
slug: ct-in-firefox
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/4xmVvHRioKg/upload/1845adcf3d0048cf789a75c0337d6b56.jpeg
tags: firefox, web-security, certificate-transparency, transparencydev

---

Certificate Transparency (CT) has been one of the biggest advancements in web security, keeping users safe from threats such as certificate fraud and man-in-the-middle attacks. While CT has been around for over 11 years, enforcement has [varied across browsers](https://certificate.transparency.dev/useragents/).

[**Firefox is now enforcing Certificate Transparency** on desktop platforms](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/OagRKpVirsA/m/Q4c89XG-EAAJ), taking a significant step towards a safer web. With this change, effective in version 135, Firefox will reject certificates that do not comply with CT requirements. This ensures that all certificates trusted by the browser meet high transparency standards.

## What does this mean for Website Owners?

It ensures that any TLS certificate trusted by Firefox is logged and publicly discoverable in [a Certificate Transparency log.](https://certificate.transparency.dev/logs/) If your website already follows best practices and uses CT-compliant certificates, you don’t need to take any additional action. However, if you’re unsure, here are some steps you can take:

1. **Ensure your Certificate Authority (CA) supports CT logging -** Most major CAs already comply, but if you are using an uncommon CA, verify their status.
    
2. **Monitor your certificates -** Use Certificate Transparency [monitoring services and tools](https://certificate.transparency.dev/monitors/)  to ensure no unauthorized certificates which would be trusted by Firefox and [other CT enforcing user agents](https://certificate.transparency.dev/useragents/) are issued for your domain.
    

## Firefox CT in Action

Certificate transparency information can be delivered either as:

* signed certificate timestamps (SCTs) embedded in the certificate itself, or
    
* SCTs stapled alongside the certificate (via the TLS handshake or in an OCSP response).
    

For a connection to succeed, sufficient certificate transparency information must be provided using either of these methods. See [Firefox CT Policy](https://wiki.mozilla.org/SecurityEngineering/Certificate_Transparency#CT_Policy) for more details.

You can see CT enforcement in action for yourself using the [‘https://no-sct.badssl.com](https://no-sct.badssl.com)‘ test site which if accessed from Firefox v135 shows the error "MOZILLA\_PKIX\_ERROR\_INSUFFICIENT\_CERTIFICATE\_TRANSPARENCY" to reflect the fact that the server does not send a Signed Certificate Timestamp (SCT) for the domain of the test site.

![Firefox v135 showing the error "MOZILLA_PKIX_ERROR_INSUFFICIENT_CERTIFICATE_TRANSPARENCY" to reflect the fact that the server does not send a Signed Certificate Timestamp (SCT) for the domain of the test site.](https://cdn.hashnode.com/res/hashnode/image/upload/v1740404495849/0e62b10f-fb01-492e-b261-03f64db998df.jpeg align="center")

Image by [Matthew McPherrin on Bluesky](https://bsky.app/profile/mattm.bsky.social/post/3lhexejiubc2t)

## Firefox Known CT Logs

Supporting CT in a browser requires verifying SCTs from an approved set of CT logs. This presents a non-trivial operational issue, especially as CT logs come and go over time. Each browser enforcing CT sets up their own user agent policy for log configuration. For CT in Firefox:

* The list of known trusted logs is derived from [Chrome’s list](https://www.gstatic.com/ct/log_list/v3/log_list.json) and is automatically updated each week in prerelease versions of Firefox. This means CT log operators do not need to submit new logs to Firefox.
    
* To see what logs are in a particular version of Firefox, you can examine the history of [Known CT Logs](https://searchfox.org/mozilla-central/source/security/ct/CTKnownLogs.h).
    

To learn more about Firefox CT policies, see [Certificate Transparency in Firefox](https://wiki.mozilla.org/SecurityEngineering/Certificate_Transparency)

## Firefox & Tile-Based Logs

As the Certificate Transparency community [moves toward tile-based logs](https://blog.transparency.dev/what-2025-holds-for-certificate-transparency-and-the-transparencydev-ecosystem) - supporting [static-ct-api](https://github.com/C2SP/C2SP/blob/main/static-ct-api.md) logs in addition to RFC6962 logs - questions arise about whether Firefox will follow suit. On the Mozilla dev-security-policy mailing list, Dana Keeler provided a positive indication that Mozilla is open to this approach, especially as industry adoption increases. [Keeler stated](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/lypRGp4JGGE/m/mareQmFjAAAJ): “*If it becomes clear that supporting static-ct-api logs is necessary to interoperate, we will probably allow them as well.*”

## Final Thoughts

The enforcement of Certificate Transparency in Firefox marks a major step forward in web security. With all major browsers now requiring certificates to be logged in CT logs, it makes it harder for attackers to abuse certificates without detection, and means a safer browsing experience for users. For website owners, it’s a reminder to stay vigilant and monitor CT logs for certificates covering their domains.

More broadly, Certificate Transparency continues to evolve and gain adoption across the industry. As browsers also start to look towards accepting tile-based logs, the CT ecosystem is becoming more robust, ensuring greater transparency and security for the web.