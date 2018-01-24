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

**Note:** Si la page affiche le message suivant "502 Bad Gateway", cela est dû aux services de la console de puppet qui sont très longs à démarrer (3-4 mins). Il suffit d'attendre un peu et de recharger la page régulièrement. Si rien ne se passe après 5 minutes, relancer les services (cf Commandes vagrant).

### Ajout de la résolution de l'hôte en local (Optionnel)
Dans "C:\Windows\System32\drivers\etc\hosts" ajouter la ligne suivante:
```
192.168.50.12 master.example.com
```
Vous pouvez maintenant accéder à la console à l'adresse: https://master.example.com/.

### Configuration de puppet et installation de l'application

- Ouvrir la console de puppet:
  - Ouvrir votre navigateur et saisir l'adresse: https://master.example.com ou http://192.168.50.12,
  - Entrer le login 'admin' et le mot de passe 'admin'.


- Préparer le déploiement des environnements:
  - Dans SETUP->Access control->users, ajouter un utilisateur 'Code Manager' (Full name) et 'code_manager' (Login),
  - Sélectionner l'utilisateur et cliquer sur 'Generate password reset',
  - Copier/Coller l'url dans votre navigateur,
  - Entrer un mot de passe pour l'utilisateur,
  - Dans SETUP->Acces control->User roles, cliquer sur le rôle 'Code Deployers',
  - Ajouter l'utilisateur 'Code Manager' au rôle,
  - Commiter les modifications.


- Déployer l'environnement de production (Défaut):
  - Taper les commandes suivantes dans la VM maître:
    - **$ puppet-access login --lifetime 180d**,
    - Entrer l'utilisateur 'code_manager' et son mot de passe,
    - **$ puppet-code deploy production --wait**.


- Configurer le catalogue de l'agent:
  - Dans CONFIGURE->Classification, ajouter un groupe 'OGAM',
  - Ouvrir le groupe,
  - Dans l'onglet 'Rules', ajouter le noeud 'agent1.example.com',
  - Dans l'onglet 'Configuration', ajouter la classe 'ogam_app',
  - Commiter les modifications.


- Lancer le catalogue de l'agent:
  - Dans INSPECT->Nodes, cliquer sur le noeud 'agent1.example.com',
  - Cliquer sur le lien 'Run Puppet' et sur le bouton 'Run'.

### Documentation Puppet utile

- [Puppet Enterprise 2017.3](https://puppet.com/docs/pe/2017.3/overview/pe_user_guide.html)
- [Puppet 5.3 (Open Source Puppet)](https://puppet.com/docs/puppet/5.3/index.html)


- Modules:
  - [Puppet Forge](https://forge.puppet.com)
  - [Module fundamentals](https://puppet.com/docs/puppet/5.3/modules_fundamentals.html)
  - [Beginner's guide to writing modules](https://puppet.com/docs/puppet/5.3/bgtm.html)
  - [The Puppet Language Style Guide](https://puppet.com/docs/puppet/5.3/style_guide.html)


- Certificats:
  - [Regenerate Puppet agent certificates](https://puppet.com/docs/pe/2017.3/ssl_and_certificates/regenerate_puppet_agent_certificates.html)
  - [Regenerate Puppet master certificates](https://puppet.com/docs/pe/2017.3/ssl_and_certificates/regenerating_certificates_monolithic_installs.html)


- Environnements de déploiement:
  - [Configuring Code Manager](https://puppet.com/docs/pe/2017.3/code_management/code_mgr_config.html#configuring-code-manager)

## Désinstallation

- Dans Git Bash (A faire pour les deux VMs):
  - **$ vagrant halt**: Stope la VM.
  - **$ vagrant destroy**: Supprime la VM.
- Supprimer les sources,
- Supprimer VirtualBox, Vagrant, Git, Cygwin.

## Développement

### Commandes Vagrant
- **$ vagrant ssh**: Ouvre une console ssh vers le serveur.
- **$ vagrant halt**: Stope la VM.
- **$ vagrant destroy**: Supprime la VM.


- Sur la VM maître:
  - **$ vagrant provision --provision-with sign-node-requests**: Signe les certificats des agents (VMs esclaves).
  - **$ vagrant provision --provision-with restart-services**: Restart the puppet console services.
  - **$ vagrant provision --provision-with clean-master-cert**: Supprime les certificats et les recrée.
  - **$ vagrant provision --provision-with manage_code**: Déploie les environnements de déploiement (TODO).


- Sur la VM esclave (agent):
  - **$ vagrant provision --provision-with clean-agent-cert**: Supprime les certificats et refait une demande au master.

### Commande Puppet
- **$ puppet config print certname**: Affiche le nom du certificat.
