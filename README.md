# puppet
Création et configuration de machines virtuelles (maître et esclave) pour tester Puppet.

## Installation

### Installation des machines virtuelles (VM)

Vagrant est utilisé pour instancier la machine virtuelle.
- Installer [VirtualBox](https://www.virtualbox.org/wiki/Downloads),
- Installer [Vagrant](https://www.vagrantup.com/downloads.html),
- Installer [Git](https://git-scm.com/downloads),
- Cloner le dépôt sgalopin/anc:
    - Ouvrir un Git Bash:
        - Se placer dans le répertoire dans lequel vous souhaitez installer le projet,
        - Cliquer sur le bouton droit de la souris,
        - Cliquer sur 'Git Bash',
        - Taper la ligne de commande suivante: 'git clone https://github.com/sgalopin/puppet.git'.

### Lancer les VMs et valider le certificat client

Se placer dans le répertoire dans lequel vous avez installé le projet:
- Ouvrir une console ('clic droit' puis 'Git Bash Here'),
- Taper la commande 'cd VM_Puppet_Master && vagrant up' (Lance la VM maître),
- Taper la commande 'cd ../VM_Puppet_Agent && vagrant up' (Lance la VM esclave),
- Taper la commande 'cd ../VM_Puppet_Master && vagrant provision --provision-with sign-node-requests' (Signe les certificats).

### Accès à la console de Puppet Master
Lancer votre navigateur et entrer l'adresse : http://192.168.50.12/

### Ajout de la résolution de l'hôte en local (Optionnel)
Dans "C:\Windows\System32\drivers\etc\hosts" ajouter la ligne suivante:
```
192.168.50.12 master.example.com
```
Vous pouvez maintenant accéder à la console à l'adresse: https://master.example.com/.

## Désinstallation

- Dans Git Bash (A faire pour les deux VMs):
  - **$ vagrant halt**: Stope la VM.
  - **$ vagrant destroy**: Supprime la VM.
- Supprimer les sources,
- Supprimer VirtualBox, Vagrant, Git, Cygwin.

## Développement

### Commandes vagrant
- **$ vagrant ssh**: Ouvre une console ssh vers le serveur.
- **$ vagrant halt**: Stope la VM.
- **$ vagrant destroy**: Supprime la VM.
- **$ vagrant provision --provision-with sign-node-requests**: Signe les certificats des agents (VMs esclaves).
