# Ansible Role Firewall

![Build Status](https://github.com/leadlineit/ansible-role-firewall/actions/workflows/ansible-galaxy-ci.yml/badge.svg)
[![Galaxy Role](https://img.shields.io/badge/Ansible--Galaxy-leadlineit.firewall-blue.svg?logo=ansible&logoColor=white)](https://galaxy.ansible.com/leadlineit/firewall/)

This role helps to system firewall on a Debian (stretch/buster/bullseye).

Requirements
------------

This role requires Ansible 1.4 or higher.

Role Variables
--------------

The variables that can be passed to this role and a brief description about them are as follows:

```yaml
---
firewall:
  ext_if: ens160
  int_if: ens192
  ipv4_forwarding: "true"
  drop_invalid: "true"
  drop_broadcast: "true"
  drop_portscan: "true"
  secure_redirects: "true"
  block_redirects: "true"
  ignore_broadcast_icmp: "true"
  ignore_bogus_errors: "true"
  block_source_route_packages: "true"
  block_proxy_arp: "true"
  enable_syn_cookies: "true"
  enable_reverse_path: "true"
  disable_bootp_relay: "true"
  disable_martian_loging: "true"
  disable_srr: "true"
  enable_sack: "true"
  disable_torrent: "true"
  ipv6_loopback_ok: "true"
  ipv6_drop_all: "true"
  ipv6_forwarding: "false"
  ipv6_disabled: "false"
  services:
    - name: SSH
      port:
        - 22/tcp
    - name: HTTPS
      port:
        - 443/tcp
        - 443/udp
  whitelist:
    - name: CloudFlare
      host:
        - 1.1.1.1
    - name: Google
      host:
        - 8.8.8.8
        - 8.8.4.4
  blacklist:
    - name: Banned
      host:
        - 9.9.9.9
  forwarding:
    - name: rdp
      rule:
        - 39929/tcp 51.75.43.24 10.0.0.5 3389
  custom_rules:
    - name: SSH-NEW-LIMIT
      rule:
        - "-A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set"
        - "-A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 5 -j DROP"
    - name: IPsec
      rule:
        - "-A INPUT -p udp --dport 500 -j ACCEPT"
        - "-A INPUT -p udp --dport 4500 -j ACCEPT"
        - "-A INPUT -p esp -j ACCEPT"
        - "-t nat -I POSTROUTING -m policy --pol ipsec --dir out -j ACCEPT"
    - name: PPTP
      rule:
      - "-A INPUT -p tcp --dport 1723 -j ACCEPT"
      - "-A INPUT -p gre -j ACCEPT"
```

Most of the above variables are optional. Some of them have a default value.
Default values for optional variables:

```yaml
---
  ext_if: eth0
  int_if: eth1
  ipv4_forwarding: "false"
  drop_invalid: "true"
  drop_broadcast: "true"
  drop_portscan: "true"
  secure_redirects: "true"
  block_redirects: "true"
  ignore_broadcast_icmp: "true"
  ignore_bogus_errors: "true"
  block_source_route_packages: "true"
  block_proxy_arp: "true"
  enable_syn_cookies: "true"
  enable_reverse_path: "true"
  disable_bootp_relay: "true"
  disable_martian_loging: "true"
  disable_srr: "true"
  enable_sack: "true"
  disable_torrent: "false"
  ipv6_loopback_ok: "true"
  ipv6_drop_all: "true"
  ipv6_forwarding: "false"
  ipv6_disabled: "false"
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
    - { role: leadlineit.firewall tags: firewall }
```

License
-------

BSD

Author Information
------------------

This role was created by Stas Stavnichuk.
