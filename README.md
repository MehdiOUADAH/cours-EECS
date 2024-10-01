# cours-EECSDans un monde où la sécurité des données est primordiale, le protocole SSH (Secure Shell) se démarque comme un outil essentiel pour l'administration à distance et la protection des communications réseau. SSH permet d'accéder à des serveurs de manière sécurisée et offre de nombreuses fonctionnalités qui renforcent la sécurité des échanges. L'authentification par clés SSH élimine le besoin de mots de passe, offrant une méthode d'authentification plus robuste et moins sujette aux attaques par force brute.
De plus, les commandes SCP (Secure Copy Protocol) et SFTP (SSH File Transfer Protocol) facilitent le transfert de fichiers de manière sécurisée, garantissant la confidentialité et l'intégrité des données échangées. L'une des caractéristiques les plus puissantes de SSH est la possibilité de créer des tunnels SSH et de rediriger des ports, permettant de chiffrer des connexions à des services non sécurisés ou d'accéder à des réseaux privés de manière sécurisée.
Cependant, pour tirer pleinement parti de ces fonctionnalités, il est crucial de sécuriser et de configurer correctement un serveur SSH. Cela inclut des étapes telles que la désactivation des connexions par mot de passe, la configuration de l'accès basé sur des clés, et l'application de règles de pare-feu adéquates.
À la fin de cette exploration, nous observerons le trafic réseau à l'aide de Wireshark pour mieux comprendre comment SSH sécurise les données en transit et pour illustrer la différence entre les transmissions non sécurisées et celles chiffrées par SSH. Ce faisant, nous mettrons en lumière l'importance d'une bonne configuration SSH dans la protection des informations sensibles.

2. LISTE DES MANIPULATIONS REALISEES

Les commandes utilisées
### Synthèse des Exercices

Voici une synthèse des exercices réalisés, avec les commandes utilisées, le contexte de
chaque action, et les résultats attendus :

#### 1. Introduction à SSH et Connexion à Distance
LE CONTEXTE
1.1 **Qu'est-ce que SSH ?**
- **Contexte :** Présentation de SSH (Secure Shell) et de ses avantages par rapport aux
méthodes non sécurisées, comme Telnet.
- **Objectif :** Comprendre l'importance de SSH pour les connexions sécurisées à distance.

1.2 **Connexion à un serveur SSH distant**
- **Exercice 1 : Connexion à un serveur SSH public (freeshell.org)**

- **Commandes :**
1. `ssh username@freeshell.org`
2. Vérifications :
- `ls` (Lister les fichiers)
- `pwd` (Afficher le répertoire courant)
- `whoami` (Afficher l'utilisateur connecté)
- **Résultat attendu :** Connexion réussie au serveur avec accès au terminal, permettant
l'exécution des commandes pour vérifier l'accès.

#### 2. Authentification par Clés SSH

2.1 **Génération et utilisation des clés SSH**
- **Objectif :** Mettre en place une authentification par clés SSH pour sécuriser les
connexions.
- **Exercice 2 : Générer des clés SSH et les installer sur le serveur distant**
- **Commandes :**
1. `ssh-keygen -t ed25519`
- La clé privée est sauvegardée dans `~/.ssh/id_rsa` et la clé publique dans
`~/.ssh/id_rsa.pub`.
2. `ssh-copy-id username@freeshell.org`

- **Résultat attendu :** Clé publique copiée avec succès sur le serveur, permettant des
connexions sans mot de passe.

2.2 **Désactivation de l'authentification par mot de passe**
- **Exercice 3 : Sécuriser la connexion**
- **Commandes :**
1. Modifier le fichier de configuration SSH :
- `sudo nano /etc/ssh/sshd_config`
- Changer la ligne : `PasswordAuthentication no`
2. Redémarrer le service SSH : `sudo systemctl restart ssh`
3. Tester la connexion : `ssh username@freeshell.org`
- **Résultat attendu :** Connexion réussie uniquement par clé SSH, sans possibilité
d'authentification par mot de passe.

#### 3. Transfert de Fichiers avec SCP et SFTP

3.1 **Utilisation de SCP pour le transfert de fichiers**
- **Objectif :** Transférer des fichiers entre la machine locale et un serveur distant.
- **Exercice 4 : Transférer un fichier avec SCP**
- **Commandes :**
1. Créer un fichier local : `echo "Ceci est un fichier test" > fichier.txt`
2. Transférer le fichier vers le serveur : `scp fichier.txt
username@freeshell.org:/home/username/`
3. Vérifier le fichier sur le serveur : `ssh username@freeshell.org "ls -l /home/username/"`
- **Résultat attendu :** Fichier transféré avec succès, visible sur le serveur distant.

3.2 **Utilisation de SFTP pour le transfert interactif**
- **Objectif :** Utiliser SFTP pour un transfert interactif de fichiers.
- **Exercice 5 : Connexion SFTP**
- **Commandes :**
1. Connexion au serveur : `sftp username@freeshell.org`
2. Naviguer dans les répertoires distants : `ls`, `cd /home/username/`
3. Transférer un fichier : `put fichier.txt`
- **Résultat attendu :** Accès à l'interface SFTP pour naviguer et transférer des fichiers, avec
confirmation de la réussite du transfert.

Cette synthèse résume les étapes clés et les résultats attendus, soulignant l'efficacité de SSH et
ses outils pour des connexions sécurisées et un transfert de fichiers fiable. Voici une liste des
difficultés potentielles que l'on peut rencontrer lors de la réalisation des exercices sur SSH, ainsi
que des méthodes pour les identifier et les résoudre :
PROBLEMES RENCONTRES ET SOLUTIONS
### 1. Problèmes de connexion SSH
Difficultés rencontrees
**Difficultés potentielles :**
- Mauvais nom d'utilisateur ou adresse IP.
- Port SSH différent de celui par défaut (22).
- Restrictions réseau ou pare-feu empêchant la connexion.
Identification et resolution des problèmes
**Identification :**
- Messages d'erreur lors de la tentative de connexion (par exemple, "Permission denied" ou
"Connection refused").

**Solutions :**

- Vérifier les informations d'identification et l'adresse IP.
- Utiliser la commande `ssh -p port username@adresse_ip` pour spécifier un port différent.
- Vérifier les règles de pare-feu sur la machine locale et sur le serveur.

---

### 2. Problèmes liés à l'authentification par clés SSH

**Difficultés potentielles :**
- Clé publique non copiée correctement sur le serveur.
- Permissions incorrectes sur le dossier `~/.ssh` ou le fichier `authorized_keys`.

**Identification :**
- Message d'erreur indiquant que l'authentification a échoué.

**Solutions :**
- Utiliser `ssh-copy-id` pour copier la clé publique.
- Vérifier les permissions avec `ls -ld ~/.ssh` (devrait être 700) et `ls -l ~/.ssh/authorized_keys`
(devrait être 600). Corriger avec `chmod`.

---

### 3. Échec de la désactivation de l'authentification par mot de passe

**Difficultés potentielles :**
- Modification incorrecte du fichier de configuration SSH.
- Service SSH non redémarré après modification.

**Identification :**
- Impossible de se connecter par clé SSH après désactivation de l'authentification par mot de
passe.

**Solutions :**
- Vérifier la syntaxe du fichier de configuration.
- S'assurer que le service SSH est redémarré avec `sudo systemctl restart ssh`.

---

### 4. Problèmes lors du transfert de fichiers avec SCP/SFTP

**Difficultés potentielles :**
- Chemin de destination incorrect.
- Permissions insuffisantes sur le serveur pour écrire dans le répertoire de destination.

**Identification :**
- Messages d'erreur tels que "No such file or directory" ou "Permission denied".

**Solutions :**
- Vérifier le chemin de destination et s'assurer qu'il existe.
- Vérifier les permissions du répertoire de destination sur le serveur.

---

### 5. Problèmes de configuration du client SFTP

**Difficultés potentielles :**
- Client SFTP non installé ou non configuré correctement.

**Identification :**
- Erreurs lors de la connexion, comme "command not found" ou "connection refused".

**Solutions :**
- Installer un client SFTP si nécessaire (par exemple, `sudo apt install openssh-client`).
- Vérifier les paramètres de configuration, le cas échéant.

---

### 6. Problèmes de compatibilité de clés SSH

**Difficultés potentielles :**
- Utilisation d'un algorithme de clé non supporté par le serveur.

**Identification :**
- Erreurs lors de la connexion indiquant une incompatibilité de clé.

**Solutions :**
- Générer une clé avec un algorithme supporté, comme RSA ou ED25519, et s'assurer que le
serveur prend en charge cet algorithme.

---

### 7. Surveillance et analyse du trafic

**Difficultés potentielles :**
- Wireshark non configuré ou manque de permissions.

**Identification :**
- Impossible de capturer le trafic, message d'erreur concernant les permissions.

**Solutions :**
- Exécuter Wireshark avec des privilèges administratifs.
- S'assurer que les interfaces réseau correctes sont sélectionnées pour la capture.

---

En étant conscient de ces difficultés potentielles et en suivant les étapes d'identification et de
résolution proposées, vous serez mieux préparé à gérer les problèmes qui peuvent survenir lors
de l'utilisation de SSH et de ses outils associés.

### Importance du Chiffrement SSH pour la Sécurité des Données

Le chiffrement SSH (Secure Shell) est un élément fondamental pour assurer la sécurité des
données échangées entre un client et un serveur. Voici les points clés concernant son
importance et ses avantages :

#### 1. **Chiffrement des Données**

Le chiffrement SSH protège les données en les rendant illisibles pour quiconque tenterait
d'intercepter le trafic. Les informations, y compris les mots de passe, les commandes et les
fichiers transférés, sont chiffrées avant d'être envoyées sur le réseau.

- **Avantage :** Cela garantit la confidentialité des données, rendant difficile l'accès aux
informations sensibles par des attaquants. Même si le trafic est intercepté, il ne peut pas être
exploité sans la clé de chiffrement.

#### 2. **Authentification Sécurisée**

SSH permet l'utilisation de clés cryptographiques pour l'authentification, offrant un moyen plus
robuste que les mots de passe traditionnels.

- **Avantage :** Les clés privées ne sont jamais envoyées sur le réseau, ce qui réduit le risque
d'attaques par phishing ou par force brute. Cela renforce la sécurité globale des connexions.

#### 3. **Intégrité des Données**

SSH utilise des algorithmes de hachage pour garantir l'intégrité des données pendant le
transfert. Cela signifie que toute tentative de modification des données en transit peut être
détectée.

- **Avantage :** Cela assure que les données reçues sont exactement celles qui ont été
envoyées, sans altération ou corruption.

### Mesures de Sécurité Complémentaires

#### 1. **Changement de Port SSH**

Par défaut, le service SSH utilise le port 22. Changer ce port à une valeur moins commune peut
réduire la probabilité d'attaques automatisées.

- **Importance :** Cela rend le serveur moins visible pour les scanners de ports malveillants, ce
qui diminue le risque d'attaques par brute force.

#### 2. **Utilisation de Fail2Ban**

Fail2Ban est un outil qui surveille les journaux du système et bloque les adresses IP qui
montrent des signes de comportement malveillant, comme plusieurs tentatives de connexion
échouées.

- **Importance :** Cela ajoute une couche supplémentaire de sécurité en protégeant le serveur
contre les attaques par force brute. En bloquant temporairement ou définitivement les adresses
IP suspectes, Fail2Ban réduit le nombre de tentatives d'intrusion réussies.

### Observations avec Wireshark

Lors de la réalisation des exercices impliquant SSH, plusieurs observations peuvent être faites
avec Wireshark, notamment :

1. **Trafic Chiffré :**
- Lorsque vous établissez une connexion SSH, vous verrez des paquets chiffrés (par exemple,
des protocoles comme SSHv2) dans l'analyse de Wireshark. Même si les paquets sont visibles,
leur contenu sera illisible, ce qui illustre l'efficacité du chiffrement.

2. **Échanges de Clés :**
- Pendant la phase d'établissement de la connexion, il peut y avoir un échange de clés. Vous
pourrez observer des messages d'initiation de session et de négociation de clés, mais sans
pouvoir déchiffrer le contenu.

3. **Tentatives de Connexion Échouées :**
- Si des attaques par force brute sont en cours (avant l'activation de Fail2Ban), vous pourrez
voir de nombreuses tentatives de connexion échouées provenant de la même adresse IP. Cela
peut indiquer un comportement malveillant.

4. **Modifications de Port :**
- Si le port SSH a été modifié (par exemple, de 22 à un autre port), vous ne verrez pas le trafic
sur le port 22, mais plutôt sur le nouveau port. Cela peut montrer l'efficacité de cette méthode
de sécurisation.

5. **Alertes de Fail2Ban :**
- Si Fail2Ban est activé, vous pouvez voir les tentatives de connexion bloquées dans les
journaux, mais Wireshark peut aussi montrer des paquets de tentative de connexion qui sont
ensuite rejetés par le serveur.

En résumé, le chiffrement SSH est crucial pour la sécurité des données, assurant leur
confidentialité, intégrité et une authentification sécurisée. Les mesures de sécurité
supplémentaires, comme le changement de port et l'utilisation de Fail2Ban, renforcent cette
protection. Les observations réalisées avec Wireshark pendant l'utilisation de SSH mettent en
évidence ces avantages, démontrant l'importance d'une approche de sécurité multifacette. ###
Résumé des Exercices Liés au Protocole SSH

Les exercices liés au protocole SSH (Secure Shell) ont été conçus pour démontrer l’importance
de la sécurité des connexions à distance. Voici les points clés résumés :

CONCLUSION

#### 1. **Introduction à SSH**
- **Qu'est-ce que SSH ?**
- Un protocole permettant des connexions sécurisées à distance, offrant des avantages
significatifs par rapport à des méthodes non sécurisées comme Telnet.

- **Connexion à un serveur SSH :**
- Utilisation de la commande `ssh username@freeshell.org` pour établir une connexion à un
serveur distant.
- Vérification de la connexion avec des commandes telles que `ls`, `pwd`, et `whoami`.

#### 2. **Authentification par Clés SSH**
- **Génération de clés SSH :**
- Création d'une paire de clés avec `ssh-keygen -t ed25519`, permettant une authentification
sans mot de passe.
- Utilisation de `ssh-copy-id` pour installer la clé publique sur le serveur.

- **Sécurisation supplémentaire :**
- Désactivation de l'authentification par mot de passe en modifiant le fichier de configuration
SSH et redémarrage du service.

#### 3. **Transfert de Fichiers**
- **Utilisation de SCP :**
- Transfert de fichiers avec `scp`, vérification de leur présence sur le serveur distant.

- **Utilisation de SFTP :**
- Connexion au serveur via SFTP pour un transfert interactif de fichiers.

#### 4. **Sécurité Renforcée**
- **Changement de Port :**
- Modification du port SSH par défaut pour réduire la visibilité aux attaques automatisées.

- **Utilisation de Fail2Ban :**

- Mise en place de Fail2Ban pour bloquer les adresses IP ayant un comportement malveillant,
renforçant ainsi la sécurité contre les attaques par force brute.

#### 5. **Importance du Chiffrement SSH**
- **Chiffrement des Données :**
- Protection des données échangées contre l'interception grâce au chiffrement.

- **Authentification Sécurisée et Intégrité :**
- Utilisation de clés cryptographiques pour l'authentification, garantissant que les données ne
sont pas altérées en transit.

#### 6. **Observations avec Wireshark**
- **Trafic Chiffré :**
- Observation des paquets chiffrés lors de la connexion SSH.

- **Tentatives de Connexion :**
- Identification de tentatives de connexion échouées, pouvant indiquer des attaques
potentielles.

- **Impact des Mesures de Sécurité :**
- Analyse de l'effet du changement de port et de l'activation de Fail2Ban sur le trafic réseau.

### Conclusion
Ces exercices démontrent comment SSH, avec ses fonctionnalités de chiffrement et
d'authentification par clés, assure la sécurité des connexions à distance. Les mesures de
sécurité supplémentaires, comme le changement de port et l'utilisation de Fail2Ban, complètent
cette protection. Les observations à l'aide de Wireshark illustrent concrètement l'importance
d'une approche de sécurité robuste dans la gestion des connexions sécurisées.
