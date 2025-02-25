# Docker Network Proxy

## Quick Start

    bin/up
    echo "127.0.0.1 api.aspiredev.org staging.aspiredev.org" | sudo tee -a /etc/hosts
    echo "::1       api.aspiredev.org staging.aspiredev.org" | sudo tee -a /etc/hosts

_(Windows users, **including WSL2**: edit `C:\Windows\System32\drivers\etc\hosts` instead of /etc/hosts)_

## HOWTO: self-signed certs that pass validation  

```shell
brew install mkcert     # also available on most other package managers
mkcert -install
mkcert -cert-file certs/aspiredev.crt -key-file certs/aspiredev.key "*.aspiredev.org"
```

## Enabling the proxy in docker-compose.yml

Add the following to the service you want proxied, substituting `myservice` and `myhostname.local` appropriately

`myservice` can be anything you want, but it must be unique across all your docker containers
    
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.myservice.rule=Host(`myhostname.local`)"
      - "traefik.http.routers.myservice-https.rule=Host(`myhostname.local`)"
      - "traefik.http.routers.myservice-https.tls=true"
    networks:
      - traefik
            
Add the following to the top level keys

    networks:
      traefik:
        external: true

Finally, add the following lines to your `/etc/hosts` file (`C:\Windows\System32\drivers\etc\hosts` on Windows)

```
127.0.0.1 myhostname.local 
::1       myhostname.local
```


