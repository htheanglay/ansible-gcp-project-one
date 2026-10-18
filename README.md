# Project One — Jenkins / SonarQube / Nexus on GCP with Ansible

| Playbook | What it does |
|---|---|
| `create.yml` | Creates 3 VMs on Google Cloud, installs Docker, tree, gh, Oh My Zsh and Portainer on each, runs Jenkins / SonarQube / Nexus in Docker with HTTPS (Caddy + Let's Encrypt), then prints the URLs and first-login passwords |
| `destroy.yml` | Deletes everything `create.yml` made |

Settings live in `group_vars/all/main.yml`. The SonarQube database password lives in
`group_vars/all/vault.yml`, encrypted with Ansible Vault. `ansible.cfg` points at the
vault password file (`~/.vault_pass`) and logs every run to `~/project-one-run.log`.

## Run

```bash
ansible-playbook create.yml    # ~10 min
ansible-playbook destroy.yml
```

| Service | URL | First login |
|---|---|---|
| Jenkins | https://jenkins.bluenodeops.online | password printed at the end of create.yml |
| SonarQube | https://sonarqube.bluenodeops.online | admin / admin |
| Nexus | https://nexus.bluenodeops.online | admin / password printed at the end of create.yml |
| Portainer | https://MACHINE-IP:9443 | create the admin user within 5 min of start (`docker restart portainer` if it timed out) |

## One-time setup (already done on gcp-main)

- `ansible-galaxy collection install -r requirements.yml`
- GCP service account key with Compute Admin at `~/.gcp/ansible-sa.json`
- `group_vars/all/vault.yml` copied from `vault.yml.example`, then `ansible-vault encrypt`
- DNS: `jenkins`, `sonarqube`, `nexus` A records at the registrar pointing to the static IPs (create.yml prints them)
