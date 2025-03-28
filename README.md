# xray-ansible: get XTLS/Xray server up & running using Ansible

Get your [XTLS/Xray](https://github.com/xtls) server up and running
with [Ansible](https://www.ansible.com/).

## Requirements

1. A Linux machine with `systemd` (tested with DigitalOcean's 1vCPU-512MB-10GB Ubuntu 23.10 droplet)
2. SSH config to access the Linux machine as a `root` (to make Ansible playbook work)
3. Python & `pip`
4. (Optional) [NekoRay/NekoBox](https://github.com/MatsuriDayo/nekoray) client (tested with v3.23)

## Provision the Linux machine

1. Clone the [repo](https://github.com/pilosus/xray-ansible)
2. Change directory

```
cd xray-ansible
```

3. Install requirements

```
pip install -r requirements.txt
```

4. Add your Linux machine IP address to the Ansible inventory file, e.g. `~/.ansible/hosts`:

```
[xray]
xray-az-label-1 ansible_host=your.machine.ip.address
```

5. Run the playbook with verbosity 1:

```
ansible-playbook -v --limit xray playbook-xray-setup.yaml \
    --inventory ~/.ansible/hosts \
    --user root
```

6. Note the `ansible_facts` from the step `xray: Decode and set Xray
   variables from file`, you will need them to set up your client
   
## Variables

The playbook is parametrized with the following variables:

- `xray_version` - version string of the [Xray-core](https://github.com/XTLS/Xray-core/tags) [default `v1.8.1`]
- `xray_platform` - platform the binary compiled for, see [Xray-core](https://github.com/XTLS/Xray-core/tags) releases for more platforms [default `linux-64`]
- `xray_path` - path for the Xray-core installation [default `/opt/xray`]

Use `-e variable=value` Ansible CLI argument to change default values, e.g.:

```
ansible-playbook -v --limit xray playbook-xray-setup.yaml \
    --inventory ~/.ansible/hosts \
    --user root \
    -e xray_version=v1.8.4
```

## Set up the client

1. Download the [NekoRay](https://github.com/MatsuriDayo/nekoray/releases) client
2. Unzip
3. Change directory to unzipped client
4. Run as `./launcher`
5. Preferences -> Basic Settings -> Core -> sign box
5. Server -> New profile -> Type VLESS
6. Edit settings:

```
Common:
Name: you profile name
Address: your Linux machine IP address/DNS
Port: 443

VLESS:
UUID: ansible_facts -> xray -> uuid
Flow: xtls-rprx-vision

Settings:
Settings: tcp
Security: tls
Packet encoding: xudp

TLS security settings:
SNI: www.microsoft.com
ALPN: h2

TLS camouflage Settings:
uTLS: chrome
Reality Pbk: ansible_facts -> xray -> x25519_public
Reaily Sid: ansible_facts -> xray -> short_id
```

7. Save settings
8. Select the new profile in the list -> right mouse click -> Start
9. Select the new profile in the list -> right mouse click -> Current Select -> URL test
10. If test from the previous step is ok, then select the checkbox System proxy
11. Enjoy

# Credits

The Ansible playbook and the `xray` role have been developed by
[dmgening](https://github.com/dmgening) and productionized by
[pilosus](https://github.com/pilosus).

The playbook relies heavily on the
[tutorial](https://habr.com/ru/articles/731608/) (in Russian; last
accessed on 25.10.23) by MiraclePtr.
