# Deployer automatiquement son site web statique sur un serveur sous Debian

Le playbook ansible fournit permet de configurer un serveur
Debian 8 afin d'y déployer automatiquement un site web statique.
Le serveur web utilisé est nginx.
Un serveur git est installé, et un script hook post-receive 
va permettre de déployer votre site dans le docroot.

Le playbook ansible a été testé sur un serveur VPS chez OVH.

## Prérequis

### Nom de domaine

Vous devez disposer de votre nom de domaine.

### Ansible

Installer et configuer ansible sur la machine de déploiement.
Dans /etc/ansible/hosts, ajouter la ligne 
concernant votre serveur web :

    monserveurweb.fr ansible_user=root

Le serveur à installer doit être accessible sous ce nom
(nom de domaine configuré).

### Accès au serveur

L'accès au serveur doit être possible par ssh 
pour l'uitlisateur root et l'**accès doit se faire par clé**.

### Paramétrage

Le fichier installation-web.yml doit être configuré (indiquer le 
nom du serveur) :

    hosts: monserveurweb.fr

Le fichier roles/nginx/vars/main.yml doit être configuré 
(indiquer les différents site web que vous souhaitez avoir) :

      vhosts:
          - www.monnomdomaine.fr 
          - blog.monnomdomaine.fr

Copier la clé publique ssh qui devra accèder au dépôt git dans :

     ansible/roles/git/files/cle-pub-ssh.pub

## Configuration du serveur

Le serveur devra être installé avec une Debian 8.
Se placer dans le répertoire ansible.

### Lancement de la configuration par ansible

**Attention, après le passage du playbook ansible, le serveur
sera accessible uniquement par clé ssh
(blocage de l'accès par mot de passe pour root).**

    ansible-playbook installation-web.yml


### Configuration du dépôt git local

Là ou vous avez développé votre site statique, configurer 
l'origin sur le serveur qui vient d'être installé.

    git init
    git remote add origin git@monserveurweb.fr:www.monnomdomaine.fr.git

Pour le premier push     
    git push --set-upstream origin master


Votre site web est désormais accessible à l'url http://www.monnomdomaine.fr.



