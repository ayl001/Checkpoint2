## Premier exercice

# Q.1.1 Pourquoi le ping avec les adresses IP des machines ne fonctionnent pas ?

Les deux machines ne sont pas sur le même LAN (les adresses IP sont différentes, elles ne sont pas dans le même /24).
Pour être méthodique, il faudrait vérifier la liaison physique (câbles) puis la liaison de donnée (ARP)
Ensuite on peut vérifier la nécessité et l'existence d'une passerelle. Les deux machines étant sur le même réseau 
physique, il convient de les configurer sur le même LAN, ainsi de changer l'adresse IP du client et peut-être le masque. 
Ce serait une mauvaise idée de changer les paramètres du serveur. On travaille donc sur la machine cliente. En pratique :
Démarrer > Paramètres > Réseau et Internet > Ethernet > Propriétés > Procole Intenet Version 4 (TCP/IP version 4) > Propriétés
et là on change l'adresse erronée par une acceptable. J'ai choisi 172.16.10.50 : je peux pinger le serveur, et le client depuis 
le serveur (voir captures d'écran).

# Q.1.2. 
Le ping fonctionne avec les noms dans les deux sens, je n'ai fait aucune manip particulière pour ça, à part changer 
l'adresse IP du client qui était erronée (Q1.1), et renseigner le serveur DNS. J'ai aussi examiné la configuration DHCP et DNS 
du serveur.

(voir captures d'écran)

# Q.1.3 
J'ai activé le DHCP sur le client. Pas de remarque particulière à faire Aucune manipulation sur le serveur, sur le client 
clic paramètres> réseau et internet > (ethernet) > propriétés > (paramètres IP)> modifier (c'est une méthode différente de 
la précédente. Je me renseignerai là-desssus). Le client ne reçoit pas la première adresse du serveur. C'est parce qu'il y 
a une plage d'exclusion (voir capture).

(voir captures d'écran) 

Q.1.4 Est-ce que ce client peut avoir l'adresse IP 172.16.10.15 en DHCP ?
Cette adresse est dans une zone d'exclusion, donc non a priori. Si on veut vraiment le faire, ce qui risque par exemple de 
priver un serveur de son adresse IP, si c'est la raison d'être de la zone d'exclusion, on peut procéder ainsi :
1. o^ter l'adresse 172.16.10.15 de la zone d'esxclusion ensuite il y a 2 méthodes :
- Première méthode : Dire au serveur DHCP d'attribuer l'adresse 172.16.10.15 à client1 (pour cela il faut renseigner 
l'adresse MAC de client1
- Deuxième méthode : faire de l'adresse 15 la première attribuable, client1 venant en premier, pas de problème, mais ça risque 
de ne plus marchera quand il y aura d'autres baux.
Dans un cas comme dans l'autre, il faut inclure l'adresse 15 au pool et exclure l'adresse 20 (afin que Clent1 ne la redemande)
Il y aurait une troisième méthode qui est celle utilisée en pratique d'attribuer au client une IP fixe. Elle ne répond pas à la 
question, donc je ne l'emploierai pas.

# Q.2.1 Lorsque l'on exécute le script il y a une erreur et le script AddLocalUsers.ps1 ne s’exécute pas.
Corrige ce script pour que le script AddLocalUsers.ps1 s'éxecute.
Il faut corriger le path de la commande appelée : il faut remplacer "Temp" par "Sript", ainsi main.ps1 lancera AddLocalUser.ps1

# Q.2.2 Le premier utilisateur du fichier Users.csv n'est jamais pris en compte. Modifie le script pour que cela soit le cas.

Quand on lit le CSV, il faut sauter une ligne (l'en-tête) au lieu de 2 : -skip 1 au lieu de -skip 2

# Q.2.3 Le champs Description est importé du fichier mais non-utilisé. 

Il faut ajouter à la déclaration de userInfo la ligne
User.Description = $Description ;

# Q.2.4 Dans l'importation des utilisateurs du fichier CSV, tous les champs sont pris.

(Non testé) dans l'initialisation de $users ne garder que les champs qui sont utilisé 

# Q.2.5 Le mot de passe crée n'est pas affiché, donc on ne le connait pas. Affiche le avec le message indiquant qu'un compte est crée.

Dans la boucle de création, quand celle-ci est effective, il faut rajouter ceci :
Write-Host "l'utilisateur $Name est créé , son mot de passe est $Pass" (en effet $password contient le mot de passe hashé)

# Q.2.6 Une fonction de création de log, nommée Log existe.

Le fichier de log est défini, mais pas utilisé,
Ajouter cette ligne dans la boucle principale :

Log -FilePath $LogFile -Content "Utilisateur $Name créé , Password: $Password"

# Q.2.7 Si l'utilisateur à créer existe déjà, il n'est pas crée.

Il convient de rajouter un else au if qui vérifie que l'utilisateur n'existe pas avant de le créer

quelque chose comme else 	{
        write-host "l'utilisateur $name existe déjà et ne sera pas scréé de nouveau"
				}

# Q.2.8 L'ajout des utilisateurs dans le groupe des utilisateurs locaux ne fonctionne pas.

L'emploi de la commande Add-LocalGroupMember ne convient pas, il faut utiliser Add-UserToLocalGroup à la place

# Q.2.9 Plusieurs fois dans le code du script, la chaine "$Prenom.$Nom" est utilisée.

La variable $name est déjà initiée dans le script à $prenom.$nom il suffit de remplacer les occurences suivante par $name et tout rentre dans l'ordre.

# Q.2.10 Les comptes utilisateurs créer ont un mot de passe qui expire.

Dans $UserInfo il faut changer la valeur de PasswordNeverExpires de false à true.

# Q.2.11 Modifie le code pour que le mot de passe soit constitué de 10 caractères au lieu de 6.

Il faut remplacer $length = 6 par $length = 10

## Troisième exercice