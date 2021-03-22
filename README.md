# monit

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/monit/status.svg)](https://drone.osshelp.ru/ansible/monit)

Role for Ansible, which installs Monit and setups services monitoring.

## Usage (example)

Typical minimal usage:

```yaml
    - role: monit
```

Configure monit without installation and with custom service:

``` yaml
    - role: monit
      monit_setup: configure
      monit_custom_services:
        consul: {group: monitoring, pid: /var/run/consul/consul.pid, checks: [
          {type: host, host: "127.0.0.1", port: 8301, timeout: 30, action: alert}
        ]}
```

Make sure that you call this role **after** the [netdata](https://gitea.osshelp.ru/ansible/netdata) role, if used together. Otherwise netdata presence will not be automatically detected and cfg generation for it will be skipped.

## Available parameters

### Main

Main parameters:

| Param | Default | Description |
| -------- | -------- | -------- |
| `monit_setup` | `full` | Setup mode. See [OSSHelp KB article](https://oss.help/kb4895) |
| `monit_custom_services` | `{}` | Custom services |

Service parameters `monit_custom_services.service_name: {parameters}`:

| Param | Default | Description |
| -------- | -------- | -------- |
| `pid` | - | [Documentation](https://mmonit.com/monit/documentation/monit.html#Process) |
| `path` | - | [Documentation](https://mmonit.com/monit/documentation/monit.html#File) |
| `poll_time` | - | [Documentation](https://mmonit.com/monit/documentation/monit.html#SERVICE-POLL-TIME) |
| `group` | - | [Documentation](https://mmonit.com/monit/documentation/monit.html#SERVICE-GROUPS) |
| `checks` | - | Checks list |

Checks parameters `monit_custom_services.service_name: {checks:[{parameters}]}`:

| Param | Default | Description |
| -------- | -------- | -------- |
| `type` | - | Supported types: `size`, `host`, `socket`. [Documentation](https://mmonit.com/monit/documentation/monit.html#SERVICE-TESTS) |
| `timeout` | - | Check timeout |
| `times` | - | Check times |
| `cycles` | - | Check cycles |
| `action` | `alert` | Check action |
| `operator` | `EQUAL` | For `size` type only. [Documentation](https://mmonit.com/monit/documentation/monit.html#FILE-SIZE-TEST) |
| `value` | `0` | For `size` type only. [Documentation](https://mmonit.com/monit/documentation/monit.html#FILE-SIZE-TEST) |
| `host` | - | For `host` type only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |
| `port` | - | For `host` type only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |
| `port_type` | - | For `host` and `socket` types only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |
| `protocol` | - | For `host` and `socket` types only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |
| `request` | - | For `host` and `socket` types only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |
| `path` | - | For `socket` type only. [Documentation](https://mmonit.com/monit/documentation/monit.html#CONNECTION-TESTS) |

### Misc

Should not be changed usually.

| Param | Default | Description |
| -------- | -------- | -------- |
| `monit_hostname` | `{{ inventory_hostname }}` | Hostname for hlreport check |
| `monit_highload_la_1m` | `15` | 1m LA value for hlreport |
| `monit_highload_la_5m` | `10` | 5m LA value for hlreport |
| `monit_daemon_interval` | `120` | Check interval |
| `monit_start_delay` | `120` | Start delay |
| `monit_port` | `2812` | Listen port |
| `monit_address` | `127.0.0.1` | Listen address |
| `monit_allow_hosts` | `[]` | List of host allowed to direct connect to Monit |

## FAQ

### Trivial self-healing

All serviсes are restarted  by default on unsuccessful availability checks, except for:

- mysql
- mongod
- rabbitmq-server
- redis-server
- docker

## Useful links

- [Official documentation](https://mmonit.com/monit/documentation/monit.html)
- [OSSHelp KB Articles](https://rm.osshelp.ru/projects/support-servers/search?utf8=%E2%9C%93&q=monit&scope=&all_words=&titles_only=&titles_only=1&kb_articles=1&attachments=0&options=0&commit=%D0%9F%D1%80%D0%B8%D0%BD%D1%8F%D1%82%D1%8C)

## TODO

...

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
