---
date: 2016-07-01T13:59:23+02:00
title: Software Projects
---

## Docker Images for Raspberry Pi

I created/ported some Docker Images that run on a Raspberry Pi and are useful in a home server scenario. All the images are publicly accessible from the default docker registry, the [Docker Hub](https://hub.docker.com/u/dastrasmue/).

### Samba
Samba allows you to share folders on your PC in your home network. The shares can have certain permissions, so that only the defined users can access the share after entering the correct password.
The image is based on [Alpine Linux](https://www.alpinelinux.org/), which is a Linux distribution with minimal size (only about 5MB). Busybox is another possible small base image, however Busybox doesn't have a packet manager. That makes it rather hard to install stuff.

[Source on GitHub](https://github.com/dastrasmue/rpi-samba)  
[Image on Docker Hub](https://hub.docker.com/r/dastrasmue/rpi-samba/)

#### run example:
```bash
docker run -d \
  -p 137:137/udp \
  -p 138:138/udp \
  -p 139:139 \
  -p 445:445 \
  -p 445:445/udp \
  --restart='always' \
  --hostname 'filer' \
  -v /media/stick:/share/stick \
  --name samba dastrasmue/rpi-samba:v3 \
  -u "alice:abc123" \
  -u "bob:secret" \
  -s "Backup directory:/share/stick/backups:rw:alice,bob" \
  -s "Alice (private):/share/stick/data/alice:rw:alice" \
  -s "Bob (private):/share/stick/data/bob:rw:bob" \
  -s "Documents (readonly):/share/stick/data/documents:ro:alice,bob"
  -s "Public (readonly):/share/stick/data/public:ro:"
```

