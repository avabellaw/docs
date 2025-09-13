---
parent: My Laptop
---

# Suspend then hibernate after x minutes

```sudo nano /etc/systemd/sleep.conf```

Defaults are commented out. Uncomment and set HibernateDelaySec.

__default is 3 hours (180mins), I set to 90mins__

Override systemd suspend with suspend-then-hibernate soft symlink

```ln -s /usr/lib/systemd/system/systemd-suspend-then-hibernate.service /etc/systemd/system/systemd-suspend.service```