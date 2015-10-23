# Tweetalytics

Une application cloud native permettant d'exécuter régulièrement des analyses depuis le flux publique de twitter.
L'application est développée pour un hébergement sur le cloud de Amazon au travers de `Amazon Elastic Beanstalk`. 

- 4 personnes par groupe
- **deadline**: samedi 14 novembre, 20h30

## Fonctionnalités

- l'application doit pouvoir déployer ses différents composants automatiquement. Idéalement, quelques scripts à lancer manuellement pour initialier l'environnement puis un simple déploiement.
- l'administrateur de l'application peut définir et réviser la requête de base à utiliser sur le flux publique de Twitter (twitter refuse toute requête sans flux)
- l'application doit capturer et stocker le flux publique dans une base de données.
- un client peut demander d'exécuter des requêtes d'analyses sur les données stocker. Il spécifie alors le type de requête, les éventuels paramètres et la périodicité de la requête (temps mort entre 2 analyses).
- l'application doit pouvoir executer un nombre théoriquement infini de requêtes simultanément.
- le client doit pouvoir à tout moment consulter ses analyses ou les supprimer.
- l'application doit pouvoir notifier le client quand une analyse est terminée.

## Les analyses à proposer
 
 - **trend match**: retracer l'évolution dans le temps de la popularité d'un hashtag par rapport à un autre dans un contexte donné. Par exemple, on analyse l'évolution de `#sacc #top` face à `#sacc #tropTop`.

 - **locality match**: retracer l'évolution géographique d'un hashtag dans le temps

 - **trend evolution**: retracer l'évolution des _k_ tags les plus utilisés

 - **watchdog**: tracer la fréquence d'utilisation d'un hashtag, une fois qu'une condition est satisfaite (hashtag ou auteur particulier par exemple)

 - **tweet propagation**: tracer l'évolution du nombre de retweets pour un tweet donné, dans le temps ou géographiquement

 - **retweet bot**: Donner des statistiques sur le nombre de retweets par utilisateur.


## Environnement de travail

- java + maven.
- git pour la gestion des sources. Code de production à déposer dans la branche `master`
- Communication client/serveur par une interface REST et des messages au format JSON.
- base de données DynamoDB
- tous les services AWS qui vous semblent nécessaires

## Travail à rendre

### Par mail
 	
 - un lien vers le dépôt GIT permettant de récupérer la branche `master` à la deadline.
	- mon identifiant github: fhermeni
	- mon identifiant bitbucket: fhermeni
	
### Dans le repository
	
 - un fichier README.md contenant
   - le nom des membres de l'équipe
   - l'URL où l'application est déployée et testable
 	- un lien vers un document google doc résumant le travail réalisé
 	- un manuel décrivant comment déployer l'application _from scratch_
 	- un accès au dashboard de l'application sur AWS de manière à observer la configuration, les règles d'élasticité
   - un code source qui compile
   - les moyens pour déployer l'application et la tester (incluant le test de passage à l'échelle) 	 	
 		

### Dans le google doc
  
  - une description du travail réalisé/non réalisé avec l'affectation des tâches
  - une description justifiée de l'architecture de l'application (est-elle tolérante au pannes, auto-élastique, ...)
  - une description de l'API REST mise à disposition des clients
  - les types d'analyse supportées
  - un modèle de coût pour l'application: combien coûte l'application par mois en fonction de différents facteurs
  - des points notables concernant l'implantation, le protocole de test de charge		  		  
  - un retour sur votre vision du projet et du module en général

## Resources

- guide développeur pour utiliser l'API streaming de twitter: https://dev.twitter.com/streaming/overview
- un client Java pour l'API streaming de Twitter: https://github.com/twitter/hbc
