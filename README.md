# quick-nginx-setup

A beginner-friendly Ansible role that installs and starts nginx on common Linux distributions.

## Supported Linux flavors

- Debian / Ubuntu (`apt`)
- Raspbian (`apt`)
- CentOS / RHEL / Rocky / Alma / Oracle Linux / Amazon Linux (`yum` / `dnf`)
- openSUSE / SUSE Linux Enterprise (`zypper`)
- Alpine Linux (`apk`)
- Arch Linux / Manjaro (`pacman`)

## Role Variables

The following variables are defined in `defaults/main.yml` and can be overridden by the playbook or inventory:

- `nginx_package_name`: name of the nginx package to install (default: `nginx`)
- `nginx_service_name`: name of the nginx service to manage (default: `nginx`)
- `nginx_state`: desired package state (default: `present`)
- `nginx_enabled`: whether to enable the nginx service at boot (default: `true`)
- `nginx_listen_port`: configured listen port for documentation only (default: `80`)
- `nginx_deploy_sample_index`: deploy a sample `index.html` page to the nginx document root (default: `true`)
- `nginx_document_root`: target document root for the sample page; if empty, the role chooses a common default based on the distribution.

## Example playbook

```yaml
- hosts: webservers
  become: true
  roles:
    - role: quick-nginx-setup
```

## How it works

- Detects the Linux package manager using `ansible_pkg_mgr` facts.
- Installs `nginx` using the matched package manager module.
- Enables and starts the nginx service.
- Fails if the package manager is not supported.

## Notes

- This role installs nginx from the operating system's package repositories.
- For custom nginx packages or third-party repos, override `nginx_package_name` or add repository setup tasks in your playbook.
- Ensure your playbook runs with privilege escalation (`become: true`) unless the target host already allows package installation as the current user.
