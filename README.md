clone_yumrepo
=============

Ansible role which clones YUM repos.

The configuration of the role is done in such way that it should not be necessary
to change the role for any kind of configuration. All can be done either by
changing role parameters or by declaring completely new configuration as a
variable. That makes this role absolutely universal. See the examples below for
more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name Example of how to use the role
  hosts: myhost1
  vars:
    # List of repos to clone
    clone_yumrepo_repos:
      - name: curator
        description: Elasticsearch Curator YUM repo
        baseurl: https://packages.elastic.co/curator/5/centos/7
        gpgcheck: 1
        gpgkey: https://packages.elastic.co/GPG-KEY-elasticsearch
        key_filename: GPG-KEY-elastic
        path: "el/7/elastic/curator/5"

      - name: elasticsearch
        description: Elasticsearch YUM repo
        baseurl: https://artifacts.elastic.co/packages/6.x/yum
        gpgcheck: 1
        gpgkey: https://artifacts.elastic.co/GPG-KEY-elasticsearch
        key_filename: GPG-KEY-elastic
        path: "el/7/elastic/elk/6.x"

      - name: grafana
        description: Grafana YUM repo
        baseurl: https://packages.grafana.com/oss/rpm
        gpgcheck: 1
        gpgkey: https://packages.grafana.com/gpg.key
        path: "el/7/grafana"

      - name: prometheus
        description: Promeheus YUM repo
        baseurl: https://packagecloud.io/prometheus-rpm/release/el/7/x86_64
        gpgcheck: 1
        gpgkey: https://raw.githubusercontent.com/lest/prometheus-rpm/master/RPM-GPG-KEY-prometheus-rpm
        path: "el/7/prometheus"

      - name: telegraf
        description: InfluxData Telegraf YUM repo
        baseurl: https://repos.influxdata.com/centos/7/x86_64/stable
        gpgcheck: 1
        gpgkey: https://repos.influxdata.com/influxdb.key
        key_filename: GPG-KEY-influxdata
        path: "el/7/influxdata"

    # Optionally use Nginx to expose the repos via HTTP
    nginx_vhost_config:
      default:
        - server:
          - listen 80
          - root {{ clone_yumrepo_webpath }}
          - location /:
              - autoindex on
              - index index.html
  roles:
    - alertmanager
    # Optional
    - nginx
```


Role variables
--------------

List of variables used by the role:

```yaml
# Packages needed fro this role to work
clone_yumrepo_pkgs:
  - yum-utils
  - createrepo

# Limit execution to specific repos only
clone_yumrepo_limit: []

# Options for the reposync command
clone_yumrepo_reposync_options: >
  --downloadcomps
  --download-metadata
  --newest-only
  --plugins

# Options for the createrepo command
clone_yumrepo_createrepo_options: >
  --verbose

# Root directory for all repos
clone_yumrepo_basepath: /srv/yumrepo

# Directory which should be used as root for the web server
clone_yumrepo_webpath: "{{ clone_yumrepo_basepath }}/html"

# Directory with cloned repos
clone_yumrepo_clonepath: "{{ clone_yumrepo_basepath }}/clones"

# Definitions of repos to be synced
clone_yumrepo_repos: []
```


Dependencies
------------

- [`nginx`](https://github.com/jtyr/ansible-nginx) (optional)


License
-------

MIT


Author
------

Jiri Tyr
