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
      <td>Login</td>
      <td>1</td>
      <td>0.1s</td>
      <td>1</td>
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

## Etapes des tests

## Execution des tests

## Resultats des tests

## Analyse et Optimisations proposées




