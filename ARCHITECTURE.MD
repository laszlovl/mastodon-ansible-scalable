# Architecture

The environment created by this playbook is horizontally scalable, but _not_ yet redundant (postgres and redis are a SPOF).

The suggested architecture is to create a primary server for all the non-scalable roles, and any number of secondary servers.
For example, you could create a single secondary server to serve both web, streaming and sidekiq; or you could create one
secondary server dedicated to web, one to streaming, and two to sidekiq.

## Primary server

* Postgres
* Nginx
* Sidekiq: scheduler
* Redis (default, cache)
* Mailserver
* Crontabs

## Secondary server(s)

* Mastodon web (Puma)
* Mastodon streaming
* Sidekiq: default, push, pull, mailers, ingress

## Hosting

A few cheap hosting providers.

* Hetzner: 2 cores + 4GB = € 5,35 ex
* Netcup: 4 cores + 4GB = € 5 ex
* Pcextreme (starter): 4 cores + 4GB = € 5 ex
* Pcextreme (ipv6 only): 1 core + 1GB = € 1 ex

# Ansible woes

* Using the "classic" method to import roles generates multiple plays, so a lot of handlers/dependencies are duplicated ... hence this somewhat convoluted setup using include_role.
