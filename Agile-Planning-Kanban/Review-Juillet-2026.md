# Review du [planning de Juillet 2026](https://kanban.blavogiez.fr/public/board/df18641a843ec3cf86d8f96d5ce9ce4ba7839490f53476b761337c943ccb)

Ce planning est le troisième de mon organisation de projets personnels, couvrant tout le mois de juillet. 
Comme prévu, il était plus léger afin de concilier ces projets avec d'autres occupations personnelles.

[Informations sur ma méthode (25% planification / documentation, 50% implémentation, 25% review / post documentation)](Review-Mai-2026.md) 

## Résumé

Ce planning portait sur la fiabilisation du [projet d'infrastructure Proxmox, toujours en collaboration avec Jonas Facon](https://github.com/jobacogiez-org/proxmox-gitops), la remise à niveau de l'infrastructure de mon projet OpenLaTeX, et l'obtention de la certification Kubernetes CKAD.

Sur Proxmox et l'outillage global :
- la mise en place du HTTPS interne pour le réseau privé au PVE avec des certificats reconnus publiquement (challenge DNS Cloudflare), avec mtn un chiffrement de bout en bout en interne sans devoir ajouter de certificats auto-signés sur chaque machine ;
- la configuration d'un remote state pour Terraform (sur un backend distant), ce qui évite les conflits de state et simplifie grandement l'application de configurations quand je change de machine ;
- le déploiement d'une registry Docker interne, permettant d'accélérer les pulls d'images sur les VMs et de ne pas saturer la connexion internet lors des déploiements ;
- l'optimisation des pipelines CI/CD, avec l'exécution conditionnelle des jobs selon les dossiers modifiés (path filtering) et l'itération automatique sur les services pour appeler les playbooks Ansible associés ;
- la création d'un dépôt GitHub Actions réutilisables partagé entre le homelab et OpenLaTeX pour centraliser et factoriser nos workflows de CI/CD.

Côté OpenLaTeX, j'ai remis à niveau l'infrastructure du cluster Kubernetes avec ce que j'ai consolidé pendant mes entraînements (la qualité ne me convenait plus avec ce que j'avais maintenant appris) : ajout de NetworkPolicies (deny all et on autorise cas par cas) pour isoler notamment Redis, amélioration des health probes (liveness/readiness), et définition propre des limites de ressources. J'ai également standardisé le playbook Ansible de déploiement en utilisant des templates Jinja2 (sur le même principe que Proxmox) et ajouté une alternative de déploiement compose-only, plus simple et légère que Helm pour des environnements de test.

Enfin, j'ai passé et obtenu la certification CKAD (Certified Kubernetes Application Developer).

## Points marquants / Difficultés / Erreurs

J'ai je trouve perdu du temps sur les choix d'architecture par ticket.
Comme prévu je passe environ 25% du temps d'un ticket à planifier / chercher la meilleure solution, et un problème qui est revenu deux / trois fois est que je partais d'un biais.

Avant de commencer un ticket je cherche environ 3 à 5 solutions alternatives entre elles et choisit la meilleure (selon notamment la complexité, l'évolutivité future, la maintenabilité, la license...). Généralement je connais environ 1 à 3 solutions dès le départ car ayant déjà vu ça passer dans le paysage devops, et je cherche le reste

Le problème c'est que j'avais parfois tendance à favoriser (peut etre inconsciemment) une solution parce que je la connaissais déjà ou que j'en avais entendu parler, au lieu d'évaluer chaque alternative de manière totalement neutre et objective avec un regard comme si je ne connaissais pas

En concret ce biais ça a été pour le container registry de passer du temps à vouloir faire fonctionner Harbor, que je connaissais de nom, qui au final était pas adapté au dépôt puisque l'installation en docker compose a besoin de templater beaucoup de fichiers (De ce que j'ai testé Harbor s'installe par un script vu que des fichiers sont générés à l'installation et ont besoin de paramètres). Après avoir vu que ça aurait été trop long / pas dans le style du dépôt  qui veut être très concis et permettre des installs simples standardisées (pour les faire par les playbooks) pour les services d'administration (docker compose et qq fichiers templatés jinja2), j'ai au final choisi Zot, qui est un registry qui fait très bien le travail

## Points d'amélioration

En résumé, voici les points d'améliorations que j'ai remarqué :
- évaluer les choix d'architecture avec un œil plus neutre : ne pas privilégier d'office une solution par familiarité ou réputation, mais comparer objectivement les alternatives selon le besoin réel avant de trancher
- continuer à factoriser entre les projets dès qu'un besoin similaire apparaît dans plusieurs projets (comme pour les GitHub Actions partagées ou les templates Jinja2)

## Victoires

En résumé, voici les améliorations que j'ai appliqué :
- il y a eu plus de documentation car plus rapide à produire en utilisant bcp de screens / diction vocale
- toutes les contributions étaient directement utiles et amélioraient la vitesse de production des autres contributions (par exemple le remote state terraform qui aide bcp quand je change de machine, ci/cd conditionnelle selon les changements)
- la gestion du backlog a bien fonctionné : le ticket bonus (#90 controller GitHub Actions Runner) était bien étiqueté "si temps restant" et n'a pas créé de dette en fin de sprint, et j'ai vraiment pu focus sur les besoins plutôt que de prioriser du peaufinage (ce ticket bonus n'était pas nécessaire aux runners qui fonctionnent actuellement bien sur docker compose)
- obtention de la certification CKAD qui du coup m'a mieux mis au courant des best practices que je peux appliquer dans mes developpements kubernetes, et aussi aidé à travailler sur kubernetes très rapidement (génération rapide avec kubectl apply et edit vim rapide)

## La suite

Pour le mois d'août je ne ferai pas de planning car je ne serai pas assez disponible pour. Ce seront des contributions isolées.

Je recommencerai en septembre, avec des plannings persos de 3 mois (rythme ralenti puisque je rentre à l'UTC + alternance)

Pour les autres review je pense que j'aurai moins de points d'amélioration à noter car j'ai maintenant fait 3 reviews, et donc appliqué déjà les améliorations majeures que j'avais jugées pertinentes. Je suis partisan de l'amélioration continue et de la remise en question de ses méthodes mais je pense que dans un contexte de projets solo je peux pas trouver à l'infini des points d'amélioration qui seraient pertinents, il y en aura quand meme mais moins je pense.
