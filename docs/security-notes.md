# Security Notes Before Publishing to GitHub

Do not upload:

- Real `.env` files
- AWS `.pem` key files
- SSL private keys such as `privkey.pem` or `private.key`
- Real passwords or generated XMPP secrets
- AWS access keys or tokens
- Screenshots exposing AWS account ID, instance IDs, public IPs, private IPs, key names, or private paths

Use only:

- `.env.example`
- Sanitized screenshots
- Placeholder domains/IPs
- Sanitized documentation

Recommended placeholders:

```env
PUBLIC_URL=https://your-domain.example.com
JVB_ADVERTISE_IPS=YOUR_JITSI_PUBLIC_IP
JICOFO_AUTH_PASSWORD=CHANGE_ME_GENERATED_SECRET
JVB_AUTH_PASSWORD=CHANGE_ME_GENERATED_SECRET
JIBRI_RECORDER_PASSWORD=CHANGE_ME_GENERATED_SECRET
JIBRI_XMPP_PASSWORD=CHANGE_ME_GENERATED_SECRET
```
