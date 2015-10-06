# Utilisation de DynamoDB

La documentation pour DynamoDB est visible ici : http://docs.aws. amazon.com/amazondynamodb/latest/developerguide/Introduction.html

_Attention à bien supprimer vos tables, arrêter vos instances EC2... à la fin du TP_

## Préparation du compte

1. Connectez-vous à votre console AWS: http://aws.amazon.com/fr/console/
2. Créez un nouvel utilisateur (rubrique Users) et créez lui une _AccessKey_ avec la politique _AmazonDynamoDBFullAccess_

## Préparation de l'environnement

### Par maven (méthode conseillée)

1. Créez un projet maven standard
2. Insérez les dépendances suivantes:

```xml
        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk-dynamodb</artifactId>
            <version>1.10.22</version>
        </dependency>

        <dependency>
            <groupId>com.amazonaws</groupId>
            <artifactId>aws-java-sdk</artifactId>
            <version>1.0.12</version>
        </dependency>
```

### Par eclipse (pas conseillé)

Installez le SDK eclipse


## Manipulation simple d’une table

AWS fournit une interface Web pour gérer à la fois son compte mais aussi les services. Chaque service est exécuté dans une région, indiquée 
en haut à droite de l'interface. Le choix de la région influe sur les performances mais aussi sur le coût. 

1. Quel est le coût d'instances DynamoDB en Europe et aux US ?

Pour la suite, vous pourrez exécuter une instance dans n'importe quelle région. Nous allons simuler un site de commerce électronique. 

1. Créez une table DynamoDB dans l'interface Web, avec comme clé un Hash de type Number et comme nom `productID`. 
2. Ecrivez un programme Java qui :
  
  1. Charge vos credentials `ProfileCredentialsProvider`
  2. se connecte à DynamoDB `AmazonDynamoDBClient`
  3. vérifie que votre table existe `Tables.doesTableExist`  

Nous allons maintenant remplir la table avec des éléments. Le principe consiste à créer un objet `Item`.
lui fixer une clé primaire et des attributs (technique du _call chaining_), et finalement le mettre dans la table. 

1. Ajoutez un produit représentant un livre avec au moins des attributs pour le titre, l'année de publication et le nombre de pages
2. Ajoutez un produit de type DVD avec un titre, une année et une durée. 
3. Vérifiez que les éléments ont bien été ajoutés grâce à la console AWS. 
3. Effectuez deux `Query` en utilisant les productID et vérifiez que vous retrouvez les éléments ajoutés
3. Faites un `Scan` pour récupérer l'ensemble des produits de votre table 


## Indexe secondaire

Un indexe secondaire permet de filtrer sur des attributs qui ne sont pas la clé primaire. Recréez  votre table pour y ajouter uné clé secondaire sur les titres, et faites des query sur le titre. 

## Accès concurrent
Par défaut les query sont éventuellement consistantes, ce qui veut dire qu'il est possible de ne pas obtenir la dernière valeur écrite. Nous allons essayer de mettre cela en valeur avec: 

1.une table qui contient des produits ayant un productID et au moins un attribut prix 
1.un programme qui modifie le prix (toujours incrémentalement) des produits
1.un programme qui vérifie et affiche le prix d'un produit

Essayez de mettre en valeur la consistance éventuelles en jouant sur les fréquences d'écriture et de lecture ainsi que le nombre de programes. Vous pourrez déployer votre code sur des instances EC2 pour observer l'influence de la latence.