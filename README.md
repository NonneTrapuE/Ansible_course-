

# Ansible

## Fichiers hosts d'Ansible

Créer un fichier ansible.cfg dans /home/USERNAME/.local/ (si l'installation de Ansible est uniquement pour l'utilisateur)
Syntaxe du fichier

``` [defaults] ```
```inventory=/path/to/hosts``` 

## Fichier inventaire 

### permet d'avoir toutes les machines sous formes de groupes  

```nano hosts```

Syntaxe d'un fichier hosts

```[groupname]	``` 
```servername	```
```servername2	```
```ipserver	```

### Différents groupes par défaut

- all
- ungrouped

### Regroupement de groupes (groupes imbriqués)

Syntaxe : 

```[web]		```
```web1.local		```
```web2.local		```
```			```
```[linux:children]	```
```web			```

### Variables d'hôtes

Les variables d'hôtes sont utilisées pour ajouter de la dynamique dans les fichiers de configuration d'ansible

les variables peuvent être associées aux cibles : 

ex : 

	```[web]```
	```web1		ansible_connection=ssh	ansible_user=web1``` 

## Syntaxe d'une commande ansible 

### Commandes ad-hoc 

Les commandes ad-hoc permettent de faire des changements ponctuels ou des tests d'une seule commande. Pour tout autre cas, il faudra utiliser des playbooks.

Crée un fichier sur le localhost

```ansible localhost -m command -a "touch /home/bastien/ansible" ```

### Executer une commande ansible avec des droits sudo

```ansible localhost --become -m command -a "touch /home/bastien/ansible" ``` 

### Syntaxe du playbook

[voir fichier yaml](/home/bastien/test.yml)

 
