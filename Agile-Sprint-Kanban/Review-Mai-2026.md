# Review du [sprint de Mai 2026](https://kanban.blavogiez.fr/public/board/7a86f59b699bf21d0b4738a037e861a22b11559d20620e17b1b2cbeae8cc)

Ce sprint est le tout premier de mon organisation de projets personnels, il démarre donc le 17 mai et se termine le 31 mai (je préfère commencer les prochains sur une date ronde 1 juin -> 30 juin par exemple).
Puisqu'étant le tout premier, il y a beaucoup de choses à en tirer.

## Thèmes

Ce sprint portait sur l'amélioration de mon Homelab Proxmox (Déploiement automatique de site, Backup PBS, Terraform, CI/CD Linters / Ansible), d'un changement de feature pour OpenLaTeX (Changement d'architecture de queue Redis dans le Cluster Kube + premier package Helm) et d'entraînement pour certifications Kubernetes CKA/CKAD.

## Gestion du temps

La principale difficulté que je rencontre dans les sprints vient du temps que j'ai à y consacrer.
Pendant cette période j'étais en stage, donc le temps que je pouvais dédier à ces projets personnels était limité, en y contribuant un peu le soir et le week-end.

j'ai trouvé un équilibre vers la fin, qui est de contribuer environ une heure par jour tous les jours, tout en variant les stacks (par exemple au lieu de faire 3 jours sur du Kubernetes puis 2 jours CI/CD d'affilée je fais 1 jour CI/CD et 1 jour Kubernetes alterné) pour maximiser l'apprentissage de chaque journée (suivant les études sur la [mémorisation espacée](https://thedecisionlab.com/fr/biases/spacing-effect)).

## Revue qualité

Pour mes sprints, un des principes est de passer les 20% de temps restant dans le mois à finaliser / améliorer la qualité des livrables du mois.

Ca m'a surtout permis de garder une bonne qualité des livrables, là où souvent dans les sprint il peut arriver de devoir faire du rush au détriment de la qualité. Même en rush, on sait que le livrable pourra finir qualitatif à la fin du mois avec cette marge de manoeuvre. je pense également qu'une double vérification est toujours intéressante sur un livrable. Il se peut également qu'un livrable réalisé en début de mois puisse être regardé sous un autre angle à la fin du mois, soit car j'aurais appris d'autres pratiques entre temps, soit car d'autres futurs livrables entrent en relation avec celui-ci.

Concrètement, il m'est arrivé de revoir l'architecture d'une feature à la fin du sprint car entre temps j'ai appris qu'il y avait une meilleure façon de faire (Notamment lorsque dans mon stage j'ai installé et configuré un serveur Proxmox, on m'a montré de meilleures pratiques que j'ai par la suite implémenté au niveau perso, avec par exemple un backup serveur PBS qui est meilleur pour mon cas au lieu d'un stockage S3). 

## Conclusion

### Points d'amélioration

En résumé, voici les points d'améliorations que j'ai remarqué :

- même si c'est utopique avec le contexte de projet perso, plus définir de tâches dès le début du sprint (= moins en rajouter en cours, mieux prédire les besoins) pour avoir une vue maximale des priorités dès le début 
- passer plus de temps à la planification (= se renseigner sur la meilleure implémentation possible) avant de démarrer un ticket (actuellement c'est environ 15% du temps total, je pourrais monter à 20/25%. Ca pourrait notamment réduire les revues d'architecture à faire à la fin en les repérant dès la planification, libérant du temps pour améliorer encore plus la qualité)  
- donc utiliser plus de sous-tâches pour avoir une meilleure granularité 

## Victoires

En résumé, voici les améliorations que j'ai remarqué :
- Le Kanban fait qu'il est impossible d'oublier quelconque tâche et permet de "saisir" toute idée dès qu'elle émerge en la notant
- Le Kanban motive plus à contribuer à des projets perso avec le ticket en tant que "carotte" / récompense
- La priorisation des tâches permet d'alléger les refactor à faire (Je pense que si une tâche prioritaire n'est pas réalisée au bon moment, elle impliquerait une bonne perte de temps de refactor)


## La suite

On embarque directement sur le [sprint de Juin](https://kanban.blavogiez.fr/public/board/2ac60474026e68fb16da9fc1b0a2ff9d0f54cd9e76224b62cbc9aab56708), avec cette fois comme thème [un projet réalisé en binôme, le Homelab Proxmox où nous sommes désormais deux avec Jonas Facon](https://github.com/jobacogiez-org/proxmox-gitops).

Review du sprint de Juin : (lien une fois réalisée)
