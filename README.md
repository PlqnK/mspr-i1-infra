# EPSI - Ingénieurie 1 - Mise en Situation Professionnelle Reconstituée

Dépôt contenant les sources Ansible d'un projet que nous avons dû réaliser en groupe à l'EPSI en 2020/2021.

Elles permettent de déployer les services suivants:

- Nextcloud
- Mattermost
- Rocket.Chat
- Moodle
- DokuWiki

Le tout sous docker avec Traefik comme reverse-proxy.

## Environnement de développement

### Prérequis

- [Vagrant](https://www.vagrantup.com/)
- Un hyperviseur d'installé et configuré, au choix:
  - KVM, avec libvirt et le plugin Vagrant `vagrant-libvirt`
  - Virtualbox
  - Hyper-V
  - VMware Workstation
- Un résolveur DNS local qui est capable de résolver `*.localhost.localdomain` vers `127.0.0.1`, au choix:
  - `systemd-resolved` sur Linux
  - `dnsmasq` sur Linux ou macOS
  - `Acrylic DNS` sur Windows

### Instructions de lancement

Clôner le dépôt et lancer Vagrant:

```bash
git clone https://github.com/PlqnK/mspr-i1-infra.git
cd mspr-i1-infra
vagrant up
```

Vagrant va créer une VM Ubuntu 18.04 dans l'hyperviseur actif sur votre PC et la provisionner avec Ansible.

### Usage

Quand le provisioning est terminé, vous pouvez ouvrir un navigateur web et naviguer vers l'interface des services web en tapant `https://[nom_du_service].localhost.localdomain`.

La documentation de Vagrant est disponible ici : <https://www.vagrantup.com/docs/>.

## Environnement de production

### Prérequis

Pour le contrôleur Ansible:

- Linux, macOS ou Windows avec WSL
- Ansible et le SDK Docker pour python d'installé (aussi connu sous le nom `docker-py`)
- Une clé SSH

Pour les hôtes de services:

- Ubuntu 18.04
- La clé SSH du contrôleur Ansible copiée sur le serveur
- Un nom de domaine payant sur lequel vous disposez de tous les droits d'administration

### Déploiement

```bash
ansible-playbook -i inventories/production.yml playbook.yml -e @vault/production.yml --ask-vault-pass
```
