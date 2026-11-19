# TP6 - Application répartie CORBA : Banque

## Présentation

Ce TP met en place une application répartie de gestion bancaire avec CORBA. Le projet est composé de deux programmes Java : un serveur qui publie le service bancaire et un client qui s'y connecte à distance.

## Architecture

- **BanqueServer** : démarre l'ORB, crée l'objet/service bancaire, l'enregistre auprès du service de nommage et attend les requêtes.
- **BanqueClient** : démarre l'ORB, recherche le service publié et invoque ses opérations à distance.
- **Interface IDL** (si présente dans le projet) : décrit le contrat partagé entre le client et le serveur. Les stubs/squelettes Java correspondants sont générés à partir de cette interface.

## Prérequis

- JDK compatible avec la version de CORBA utilisée dans le TP.
- Un service de nommage CORBA, généralement **orbd** dans les environnements Java qui le fournissent.
- Les sources du projet et les classes générées à partir du fichier IDL, le cas échéant.

> CORBA n'est plus inclus dans les versions récentes du JDK. Si le projet s'appuie sur `org.omg.*` et `orbd`, utilisez le JDK prévu par le cours (souvent JDK 8) ou configurez les dépendances CORBA demandées par l'enseignant.

## Démarrage

Lancer les commandes depuis le dossier du projet, en adaptant le nom du fichier IDL, les packages et le classpath à l'arborescence réelle.

1. **Compiler l'interface IDL**, si elle est fournie :

   ```bash
   idlj -fall Banque.idl
   ```

2. **Compiler les sources Java** :

   ```bash
   javac -d out $(find . -name "*.java")
   ```

   Sous Windows PowerShell, vous pouvez compiler les fichiers listés dans un fichier d'arguments ou utiliser la configuration de compilation de l'IDE.

3. **Démarrer le service de nommage** dans un premier terminal :

   ```bash
   orbd -ORBInitialPort 1050
   ```

4. **Démarrer le serveur** dans un deuxième terminal :

   ```bash
   java -cp out BanqueServer -ORBInitialPort 1050
   ```

5. **Démarrer le client** dans un troisième terminal :

   ```bash
   java -cp out BanqueClient -ORBInitialPort 1050
   ```

Les noms de classes, options ORB, ports et commandes exactes dépendent des fichiers fournis et peuvent nécessiter une adaptation.

## Vérification

Le client doit pouvoir retrouver le service enregistré par le serveur et effectuer les appels prévus par le TP. Démarrer le service de nommage avant le serveur, puis le serveur avant le client. Vérifier que les deux applications utilisent le même port et le même service de nommage.

## Dépannage

- **Service introuvable / `NotFound`** : vérifier que le serveur est lancé et que le nom utilisé lors de l'enregistrement correspond à celui recherché par le client.
- **Connexion refusée** : vérifier que `orbd` fonctionne et que le port ORB est identique côté serveur et client.
- **Classes générées ou packages introuvables** : régénérer les sources Java à partir de l'IDL et vérifier le classpath.
- **`package org.omg... does not exist`** : utiliser le JDK/les bibliothèques CORBA compatibles avec le projet.

## Organisation indicative

```text
.
├── Banque.idl                 # contrat CORBA (si fourni)
├── BanqueServer.java          # serveur
├── BanqueClient.java          # client
├── <sources générées IDL>/     # stubs, squelettes et interfaces
└── README.md
```