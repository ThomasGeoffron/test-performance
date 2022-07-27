# test-performance
2nd semester project for 'Test de performance' course

Projet réalisé par :
- [Thomas GEOFFRON](https://github.com/ThomasGeoffron)
- [Milan CAMUS](https://github.com/MisterGoodDeal)
- [Arthur VADROT](https://github.com/Haborym)

Projet à tester : [Esca'lab (Projet Annuel)](https://github.com/MisterGoodDeal/Esca-lab)

## Préambule

### Esca'lab

Esca'lab est un site web qui permet aux utilisateurs de partager des informations sur leur activité dans le cadre de l'escalade. Ils peuvent accéder aux différentes salles inscrites sur le réseau et aux informations sur les activités organisées par les salles.

### Objectifs et type d'utilisateurs prévus

- En temps normal, une activitée quotidienne d'environ 10K utilisateurs répartis sur la journée de 9h00 à 23h00.
- Lors d'évènements, des utilisateurs peuvent se connecter à l'application pour participer à l'évènement en validant les voies complétées. L'infrastructure doit être capable de gérer environ 100 requêtes par minutes toutes les 5 minutes et cela pendant un période de 4h00 (généralement de 21h00 à 00h00) et pour chaque salle organisant un évènement.

## Architecture de l'application

### Technologies / Langages utilisés

- Serveur: 02Switch (Serveur mutualisé)
- BDD: PostgreSQL
- Framework: Symfony 5.3
- Langage: PHP 8.0
- Serveur WEB: NGINX
- Autres librairies : Bootstrap, jQuery

## Exigences du test

Ce test de performance a pour objectif de déterminer les limites de performances de l'application par l'envoi de multiples requêtes stressant le serveur.

### Capacité maximale (Connexions simultanées)

### Processus non-fonctionnels

<table>
  <thead>
    <tr>
      <th>Business Transaction</th>
      <th>User Load</th>
      <th>Response Time</th>
      <th>Transaction per hour</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Se connecter</td>
      <td>200</td>
      <td>1s</td>
      <td>500</td>
    </tr>
    <tr>
      <td>S'enregistrer</td>
      <td>200</td>
      <td>1s</td>
      <td>500</td>
    </tr>
    <tr>
      <td>Accéder aux franchises</td>
      <td>350</td>
      <td>0.3s</td>
      <td>600</td>
    </tr>
    <tr>
      <td>Accéder aux salles d'une franchises</td>
      <td>350</td>
      <td>0.3s</td>
      <td>600</td>
    </tr>
    <tr>
      <td>Accéder aux évènements</td>
      <td>350</td>
      <td>0.3s</td>
      <td>600</td>
    </tr>
    <tr>
      <td>Commenter une voie</td>
      <td>50</td>
      <td>0.5s</td>
      <td>150</td>
    </tr>
  </tbody>
</table>

## Environnement de test

Prod: 
- CPU: Intel Xeon E5-2680v4 @ 2.4 GHz
- OS: Linux server 20.04
- RAM: 48GB

Dev:
- CPU: AMD Ryzen 5 3500U @ 2.1 GHz
- OS: Ubuntu 22.04 LTS
- RAM: 8GB

L'environnement de test est nettement moins performant que la production.

Coefficient proportionnel : `0.28`

## Planification des tests

Le fait que le serveur soit un hébergement mutualisé indique que la bande passante est limitée car partagée. Ce qui en fait une limitation de l'infrastructure.

Premièrement, nous allons effectuer un test de type `Load Testing` dans lequel des utilisateurs non connectés vont visiter les différentes pages disponibles sur le site.

Deuxièmement, nous allons effectuer un test de type `Stress Testing` dans lequel un utilisateur va se connecter afin de se rendre sur la page d'une voie quelconque et y poste plusieurs commentaires.

Nous allons surveiller les métriques suivantes pour les deux types de tests :
- Utilisation du processeur
- Temps de réponse
- Utilisation de la bande passante

## Etapes des tests

<table>
  <thead>
    <tr>
      <th>Step #</th>
      <th>Business Process Name : Visiter le site</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Page d'accueil</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Accéder aux franchise</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Accéder aux salles de la franchise</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Revenir à la page d'accueil</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Accéder à la page des évènements</td>
    </tr>
</table>

<table>
  <thead>
    <tr>
      <th>Step #</th>
      <th>Business Process Name : Poster un commentaire sur une voie</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Page d'accueil</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Connexion</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Accéder aux franchise</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Accéder aux salles de la franchise</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Accéder aux informations d'une voie</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Poster un commentaire</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Se déconnecter</td>
    </tr>
</table>

Le jeu de données utilisé pour les deux process est créé via des fixtures de Symfony.

## Execution des tests

<table>
  <thead>
    <tr>
      <th>Test Run</th>
      <th>Test Scenario Summary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Cycle 1 - Run 1</td>
      <td>Load Test - 10 minutes test with peak load</td>
    </tr>
    <tr>
      <td>Cycle 1 - Run 2</td>
      <td>Stress Test - 5 minutes test with 125% of peak load</td>
</table>

<table>
  <thead>
    <tr>
      <th></th>
      <th>Test Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Purpose</td>
      <td>Tester le temps de réponse, l'utilisation du CPU et de la bande passante lors d'un fort traffic continu afin d'évaluer si le serveur peut supporter une telle charge</td>
    </tr>
    <tr>
      <td>No. of tests</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Duration</td>
      <td>Ramp-up: 2 min - Steady State : 6 min - Ramp-down : 2 min</td>
    </tr>
    <tr>
      <td>Script</td>
      <td>Visit.jmx</td>
    </tr>
    <tr>
      <td>Scenario Name</td>
      <td>Visiter le site</td>
    </tr>
    <tr>
      <td>User Load / Volume</td>
      <td>200 VUsers</td>
    </tr>
    <tr>
      <td>Entry Criteria</td>
      <td>
        <ul>
          <li>Le code est stable et les fonctionnalités sont vérifiées</li>
          <li>L'environnement de test est stable</li>
          <li>Les données de test sont disponibles</li>
          <li>Les scripts de test sont prêts à être exécutés</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Validation Criteria</td>
      <td>
        <ul>
          <li>Les tests sont terminés avec succès</li>
          <li>Le temps d'éxecution est inférieurs à 1s</li>
          <li>L'utilisation du processeur est inférieur à 80%</li>
          <li>L'utilisation de la bande passante est inférieur à 85%</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
      <th></th>
      <th>Test Details</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Purpose</td>
      <td>Tester le temps de réponse, l'utilisation du CPU et de la bande passante lors d'une surchage pour le serveur dans lequel plusieurs compte se connectent et commentent une voie</td>
    </tr>
    <tr>
      <td>No. of tests</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Duration</td>
      <td>Ramp-up: 1 min - Steady State : 3 min - Ramp-down : 1 min</td>
    </tr>
    <tr>
      <td>Script</td>
      <td>Comment.jmx</td>
    </tr>
    <tr>
      <td>Scenario Name</td>
      <td>Commenter une voie</td>
    </tr>
    <tr>
      <td>User Load / Volume</td>
      <td>300 VUsers</td>
    </tr>
    <tr>
      <td>Entry Criteria</td>
      <td>
        <ul>
          <li>Le code est stable et les fonctionnalités sont vérifiées</li>
          <li>L'environnement de test est stable</li>
          <li>Les données de test sont disponibles</li>
          <li>Les scripts de test sont prêts à être exécutés</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>Validation Criteria</td>
      <td>
        <ul>
          <li>Les tests sont terminés avec succès</li>
          <li>Le temps d'éxecution est inférieurs à 1s</li>
          <li>L'utilisation du processeur est inférieur à 80%</li>
          <li>L'utilisation de la bande passante est inférieur à 85%</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Resultats des tests

## Analyse et Optimisations proposées




