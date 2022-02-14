# drone-diagnostics

A plugin with cli tools to diagnose issues in a build.

## Usage

This plugin has a number of diagnosic tools installed, use the command syntax to access them.

Installed tools:

- ping
- traceroute
- dig

### Example 1

The below example skips the implicit clone step. Then displays the environment variables, then checks access to github. With ping and traceroute.

```yaml
kind: pipeline
name: default

clone:
  disable: true

steps:
- name: run tphoney/drone-diagnostics plugin
  image: tphoney/drone-diagnostics
  pull: if-not-exists
  commands:
    - env
    - ping www.github.com -w 5
    - traceroute -T -p 443 www.github.com
    - dig www.apple.com
    - echo "end of test"
```

### Example 2

If the ftp upload fails, check connectivity to the ftp server. Useful for flakey networks.

```yaml
kind: pipeline
name: default

steps:
- name: upload to ftp server
  image: alpine
  commands:
    - echo 'ftp rules' # perform some ftp commands
- name: run tphoney/drone-diagnostics plugin
  image: tphoney/drone-diagnostics
  pull: if-not-exists
  commands:
    - ping ftp.server
    - echo "end of test"
  when:
    status:
    - failure
```

## Building

Build the plugin image:

```text
docker build -t tphoney/drone-diagnostics -f docker/Dockerfile .
```
