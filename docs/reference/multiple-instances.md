# Multiple App Instances

Apps that used to be supported by the "ArrX" system which allowed the user to define a set of instances of a given app [as opposed to installing multiple instances one at a time] are being transitioned to a new generalized, inventory-driven approach.

The general idea is to move all the configuration into the `/srv/git/saltbox/inventories/host_vars/localhost.yml` along with [other customizations](../saltbox/inventory/index.md).

You can find roles that support this new method with the following command:

```shell
grep -Ril "_instances:" /srv/git/saltbox/roles /opt/sandbox/roles | awk 'BEGIN{RS="roles/"; FS="/defaults"}NF>1{print $1}' | sort -u
```

At the time of writing the roles supported are:

```yaml
bazarr
deluge
emby
jellyfin
lidarr
nginx
overseerr
plex
qbittorrent
radarr
readarr
sonarr
tautulli
```

## Overview

Define a list of all the instances of the all you want to create; if you don't want to customize them beyond that, this is all that's required.

The standard app tag will now set up all those instances.

``` yaml
sonarr_instances: ["sonarr", "sonarrbing", "sonarrbang", "sonarrboing"]
```

Given the config above, `sb install sonarr` would install:

| entry         | Container    | Config dir         | Subdomain                    |
| ------------- | ------------ | ------------------ | ---------------------------- |
| sonarr        | sonarr       | `/opt/sonarr`      | sonarr.YOURDOMAIN.TLD        |
| sonarrbing    | sonarrbing   | `/opt/sonarrbing`  | sonarrbing.YOURDOMAIN.TLD    |
| sonarrbang    | sonarrbang   | `/opt/sonarrbang`  | sonarrbang.YOURDOMAIN.TLD    |
| sonarrboing   | sonarrboing  | `/opt/sonarrboing` | sonarrboing.YOURDOMAIN.TLD   |


Those names have to be unique across all of your containers, so it is suggested that you keep with the `rolename+suffix` pattern for these additional instances.

### Per-instance customization

Any instance defined can edit the following set of variables on a per instance basis in `localhost.yml`:

!!! note
    Replacing "instance" with the actual **instance name**, of course, i.e. sonarrbing_web_subdomain, etc.
```
instance_web_subdomain
instance_web_domain
instance_web_port
instance_traefik_sso_middleware
instance_docker_image_repo
instance_docker_image_tag
instance_docker_ports_defaults
instance_docker_ports_ui
instance_docker_ports_custom
instance_themepark_enabled
instance_themepark_domain
instance_themepark_theme
instance_docker_envs_default
instance_docker_envs_custom
instance_docker_commands_default
instance_docker_docker_commands_custom
instance_docker_volumes_default
instance_docker_volumes_custom
instance_docker_volumes_theme
instance_docker_devices_default
instance_docker_devices_custom
instance_docker_hosts_default
instance_docker_hosts_custom
instance_docker_labels_default
instance_docker_labels_custom
instance_docker_networks_default
instance_docker_networks_custom
instance_docker_capabilities_default
instance_docker_capabilities_custom
instance_docker_security_opts_default
instance_docker_security_opts_custom
```
