# Config for caddyfile nat

Create an environment variable in the systemd file for caddy

```systemclt edit caddy```

``` systemd
[Service]
Enviro**n**ment=CF_API_TOKEN=xxxxxxxxxxxxxxxxxx
```

```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace ":80" below with your
# domain name.

#jellyfin.avabella.uk {
#       reverse_proxy 10.0.0.101:8096
#}

avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.1:8006 {
                transport http {
                        tls_insecure_skip_verify
                }
        }
}

adguard.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.100:80
}

jellyfin.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.101:8096
}

qbit.avabella.uk, qbittorrent.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.105:8090
}

sonarr.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.106:8989
}

radarr.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.107:7878
}

prowlarr.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.108:9696
}

jellyseer.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.104:5055
}

flaresolverr.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.109:8191
}

tdarr.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.103:8265
}

stork.avabella.uk {
        tls {
                dns cloudflare {env.CF_API_TOKEN}
        }
        reverse_proxy 10.0.0.115:8080
}
```