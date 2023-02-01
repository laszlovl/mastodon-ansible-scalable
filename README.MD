# Create your environment configuration

This repository currently contains both the generic ansible configuration as the specific configuration for my PoC environment https://tootnet.nl.
To get started with your own environment, simply fork this repository and add your own environment-specific configuration.

* Generate a random password and store it in a file named `.vault_password`
* Think of a name for your environment, e.g. "mastodon_eu"
* `mkdir group_vars/mastodon_eu`
* Create the vault with secrets: `ansible-vault create group_vars/mastodon_eu/vault.yml`
  * See `group_vars/vault.yml.example` for instructions
* Create the regular configuration in: `group_vars/mastodon_eu/vars.yml`
  * See `group_vars/tootnet_live/vars.yml` for an example, and browse the various `defaults/` directories to see what else you can override
* Create the inventory in: `inventory/mastodon_eu.yaml`
  * See `inventory/tootnet_live.yml` for an example

# Deployment

Servers must run Ubuntu 22.04 LTS. An IPV6 address is mandatory, IPV4 is optional (but then a provider with 6to4 support is recommended to ensure federation with ipv4-only instances)

* Run `ansible-playbook -i inventory/mastodon_eu.yaml server_setup.yaml` for the initial deployment, then repeat every time you add a new cloud server to your inventory.
  * Because this playbook changes the SSH port, it will throw errors for servers where it's already been ran. You can simply ignore those.

* Run `ansible-playbook -i inventory/mastodon_eu.yaml mastodon.yaml`  for the initial deployment, then repeat every time you make a change to the configuration.

# DNS

Given a `mastodon_app_domain` of "mastodon.eu", point A/AAAA records to the nginx server(s) for these hostnames:

* `mastodon.eu`
* `files.mastodon.eu`
* `assets.mastodon.eu`

For mail delivery, given the `mail` role running on a server with hostname "server1" and DNS record "server1.mastodon.eu",
set the reverse DNS for server1 to `mastodon.eu` and create an SPF record  like `v=spf1 a:server1.mastodon.eu ~all`.

# Initialize

To create an admin user: `RAILS_ENV=production bin/tootctl accounts create mastodon_eu --email admin@mastodon.eu --confirmed --role Owner`

# TODO

* build mastodon as deb
* monitoring
* dkim