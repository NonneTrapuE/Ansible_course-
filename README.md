

# Ansible

## Fichiers hosts d'Ansible

Créer un fichier ansible.cfg dans /home/USERNAME/.local/ (si l'installation de Ansible est uniquement pour l'utilisateur).

Syntaxe du fichier:

	[defaults]
	inventory=/path/to/hosts 

## Fichier inventaire 

### permet d'avoir toutes les machines sous formes de groupes  

```nano hosts```

Syntaxe d'un fichier hosts

	[groupname] 
	servername
	servername2
	ipserver

### Différents groupes par défaut

- all
- ungrouped

### Regroupement de groupes (groupes imbriqués)

Syntaxe : 

	[web]
	web1.local
	web2.local
			
	[linux:children]
	web

### Variables d'hôtes

Les variables d'hôtes sont utilisées pour ajouter de la dynamique dans les fichiers de configuration d'ansible

les variables peuvent être associées aux cibles : 

ex : 

	[web]
	web1		ansible_connection=ssh	ansible_user=web1 

 ### Variables de groupes

 Ces variables sont définies au niveau du groupe

ex:

	[all:vars]
	ansible_connection=ssh
 

## Syntaxe d'une commande ansible 

### Commandes ad-hoc 

Les commandes ad-hoc permettent de faire des changements ponctuels ou des tests d'une seule commande. Pour tout autre cas, il faudra utiliser des playbooks.

Crée un fichier sur le localhost

```ansible localhost -m command -a "touch /home/bastien/ansible" ```

### Executer une commande ansible avec des droits sudo

```ansible localhost --become -m command -a "touch /home/bastien/ansible" ``` 

### Exemple de syntaxe d'un playbook

[voir fichier yaml]

### Variables dans le playbook

Syntaxe pour l'utilisation de variables dans un playbook

ex:

	---
 	  - hosts: web
        tasks:
	    - name : Create a web file
          file:
	         dest: '{{web_file}}'
	  	 state: '{{file_state}}'

 
## Inventaire dynamiques

Les inventaires dynamiques sont particulièrement utiles lors de développement de VM localement ou d'utilisation d'instances cloud. Elles premettent d'aller chercher les informations des machines voulues.
Cependant, certains plug-ins d'inventaire et pré-requis sont obligatoires pour leur utilisation. (aws_ec2 ou virtualbox par exemple)

ex:

Pour virtualbox, un fichier ansible.cfg devra contenir les lignes suivantes :
	
 	[inventory]
	enable_plugins = host_list, virtualbox, constructed, script, auto, yaml, ini, toml
	[defaults]
 	inventory=/path/to/vbox.yml

Le fichier d'inventaire, lui, devra s'appeler obligatoirement vbox.yml.
Il contiendra par exemple les lignes suivantes :

	plugin: virtualbox
	running_only: yes	#Récupère les vms en ligne uniquement
 	groups:
  	    web: "'web' in (inventory_hostname)"
 	    ha: "'ha' in (inventory_hostname)"


Plus de renseignements sur l'utilisation du plugin Virtualbox Inventory [ici](https://docs.ansible.com/ansible/latest/collections/community/general/virtualbox_inventory.html)

 Plus de renseignements sur l'utilisation des plugins d'inventaire [ici](https://docs.ansible.com/ansible/latest/plugins/inventory.html#using-inventory-plugins) 
