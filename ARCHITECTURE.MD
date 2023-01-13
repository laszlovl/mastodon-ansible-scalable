# Architecture

## Primary machine

* Postgres
* Nginx
* Sidekiq: scheduler
* Redis (default, cache)
* Exim

## Secondary machine(s)

* Puma
* Streaming API
* Sidekiq: default, push, pull, mailers, ingress

## Hosting

Hetzner: 2 cores + 4GB = € 5,35 ex
Netcup: 4 cores + 4GB = € 5 ex
Pcextreme (starter): 4 cores + 4GB = € 5 ex
Pcextreme (ipv6 only): 1 core + 1GB = € 1 ex

# Ansible woes

* Using the "classic" method to import roles generates multiple plays, so a lot of handlers/dependencies are duplicated ... hence this somewhat convoluted setup
