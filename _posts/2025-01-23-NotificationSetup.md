---
title: Notification Setup
categories: [Configuration]
tags: [notifications, email, pushover]
---

## Setting Up Notifications to Pushover Via Email

1. Run commands:
    ```bash
    sudo apt install -y libsasl2-modules
    sudo apt install mailutils
    echo "smtp.gmail.com michelfrancisb@gmail.com:PASSWORD" >> /etc/postfix/sasl_passwd
    sudo chmod 600 sasl_passwd
    postmap hash:sasl_passwd
    ```
2. Edit /etc/postfix/main.cf
   - Delete lines starting with "relayhost" and "mydestination"
   - Add lines:
      > relayhost = smtp.gmail.com:587
      smtp_use_tls = yes
      smtp_sasl_auth_enable = yes
      smtp_sasl_security_options =
      smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
      smtp_tls_CAfile = /etc/ssl/certs/Entrust_Root_Certification_Authority.pem
      smtp_tls_session_cache_database = btree:/var/lib/postfix/smtp_tls_session_cache
      smtp_tls_session_cache_timeout = 3600s

3. Run `postfix reload`

### Testing

```bash
echo "Test Email" | mail -s "Test Subject" DESTINATION_EMAIL
echo "Test email from Proxmox: $(hostname)" | /usr/bin/proxmox-mail-forward
```
