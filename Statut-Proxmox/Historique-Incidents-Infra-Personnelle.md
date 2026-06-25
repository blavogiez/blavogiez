# Historique des pannes de mes services auto-hébergés (*.blavogiez.fr)

Ce document retrace l'ensemble des pannes qui ont pu se produire sur mes sites `*.blavogiez.fr` et `blavogiez.fr`, auto-hébergés sur une [infrastructure Proxmox](https://github.com/blavogiez-org/proxmox-configuration) à mon domicile.

Ce type d'hébergement "civil" donne donc des défis d'installation physique.

Dans mon cas, la quasi-totalité des coupures sont physiques et non logicielles, c'est-à-dire des problèmes d'électricité, de fournisseur d'accès internet, ou d'aménagement du domicile.

Ce document s'inscrit également dans une logique d'amélioration de mes services pour arriver progressivement à de la `HA / High Availability` proche de 100%.
Pour chaque panne, j'inscris donc une réponse qui permettra d'évoluer.

## Jeudi 25 juin 2026 - 4h00 -> 6h15

**[Panne du réseau électrique](https://www.ici.fr/hauts-de-france/nord-59/lille/60-000-clients-touches-par-une-coupure-de-courant-cette-nuit-dans-la-metropole-lilloise-1074540) (incendie) dans mon quartier.**

La machine actuellement en production étant un ordinateur classique, il n'a pas de contrôle d'alimentation automatique disponible permettant de la rallumer automatiquement.

### Réponse

J'ai un onduleur en attente de livraison (déjà commandé avant la panne car c'est un problème assez récurrent et prévisible dans l'auto-hébergement) qui permettra de résister aux coupures électriques. Il protègera la box Internet et le serveur branché.
Avec le [NUT](https://doc.ubuntu-fr.org/nut), système qui permet la communication entre un onduleur avec un système Linux, dans un premier temps je l'éteindrai lorsque l'onduleur sera à 20% de batterie.

Une idée serait également de passer en mode économie d'énergie (éteindre des VMs, baisser la fréquence, délier le GPU...) pour qu'il s'éteigne le moins rapidement possible (par exemple à 80% de batterie, on passe en économie).
Une page (théorique) `status.blavogiez.fr` pourrait également être mise à jour très simplement (fichier html ciblé par reverse proxy) indiquant l'état électrique du serveur, et les éventuelles décisions automatiques.

L'onduleur a une batterie très faible, alors la prochaine solution serait une station de charge / grosse batterie (qui pourrait nourrir 500Wh, soit 2 heures minimum sur un gros serveur par exemple). Cet élément coûte beaucoup plus cher, alors j'attendrai de le trouver en occasion. La chaîne serait donc "prise -> station de charge -> onduleur -> box/serveur"

Avec tous ces éléments, les problèmes électriques seront résolus. Je m'étais également renseigné sur les statistiques de coupure d'électricité. Ces stats sont assez généralistes, je n'ai pas pu trouver le 5% des pires durées de coupure mais je l'estime comme étant la seule proportion possiblement supérieure à 2h (en métropole + grande ville). Le plan onduleur + station 500Wh pourrait alors couvrir au moins 95% des pannes, et dans 100% des cas avoir une mise hors alimentation sécurisée (avec station ou non, c'est le rôle du NUT / onduleur).

Quelques métriques intéressantes

- [Fréquence moyenne Enedis](https://opendata.enedis.fr/datasets/frequence-moyenne-de-coupure-par-client-bt)
- [Durée moyenne Enedis](https://opendata.enedis.fr/datasets/duree-moyenne-de-coupure-bt)
- [En graphiques](https://data.enedis.fr/pages/qualite-de-fourniture/)

## Mardi 26 mai 2026 - 10h30 -> 17h45

**Instabilité du réseau électrique ayant impacté la connexion Ethernet CPL**

(CPL permet d'avoir une liaison Ethernet à travers le réseau électrique d'un domicile, ce qui peut être plus rapide que le Wi-Fi selon la position de la box et de la machine.)

J'étais en stage / pas chez moi, alors je ne pouvais rien faire physiquement lorsque la panne est survenue. j'ai pu regarder pendant la pause de midi et le VPN revenait très brièvement environ 2 minutes par heure, j'avais accès au panel Proxmox pendant cette durée, donc j'ai pu déduire qu'il s'agissait d'un problème CPL propre à la machine (la box était joignable autrement).
C'était particulièrement frustrant de savoir le problème et de ne pas pouvoir le solutionner

### Réponse

C'était une erreur de ma part d'avoir relié le serveur en CPL. La connexion était stable pendant 3 semaines auparavant, et lorsque la première vague de chaleur est arrivée, le réseau électrique a fauté.

J'ai donc bougé le serveur à côté de la box pour avoir une connexion Ethernet directement (ce qui est beaucoup moins praticable puisque c'est un petit couloir, et c'est ce qui a fait qu'en premier j'utilisais un CPL) et depuis la connexion a toujours été stable.
J'ai également mis un fallback vers une carte Wi-Fi (car durant cette panne, le Wi-Fi était actif)
