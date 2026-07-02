# Review du [sprint de Juin 2026](https://kanban.blavogiez.fr/public/board/2ac60474026e68fb16da9fc1b0a2ff9d0f54cd9e76224b62cbc9aab56708)

TD: en écriture au 2 juillet 2026, finalisation le 3 juillet max
TD: liens à remplir
TD: clean les espaces introduits par Vim

Ce sprint est le deuxième de mon organisation de projets personnels, couvrant tout le mois de juin. 
Il s'inscrit dans une stabilisation et application des [améliorations remarquées pour le sprint précédent](Review-Mai-2026.md) (Cette première review de Mai 2026 donne également le cadre de mon organisation et les principes que je suis).

## Résumé

Ce sprint portait sur la fiabilisation du [projet d'infrastructure Proxmox, désormais réalisé en collaboration avec Jonas Facon, camarade de BUT.]

Sous les principes d'automatisation / configuration descriptive par fichier régissant ce projet, j'ai traité 3 points principaux :
- le déploiement d'un coffre de secrets / Vault, ici OpenBao (fork de HashiCorp Vault), afin de centraliser la gestion de secrets qui jusqu'ici était principalement réalisée à la main ;
- la gestion de l'observabilité, avec une combinaison de Prometheus / Loki / Grafana sur une VM recevant les métriques / logs de toutes les instances, grâce à un agent Grafana Alloy déployé sur chaque instance, écrivant ainsi sur la VM monitoring (remote write) (cet agent peut également lire les logs docker grâce à la socket). Cette observabilité est complétée par une page de statut des domaines avec leur uptime (utilisation de Gatus, alternative à Uptime Kuma, s'étant trouvé plus simple d'utilisation pour des configurations par fichiers). Cela permet d'avoir une visibilité centralisée des erreurs pouvant se produire sur les VM.
- le déploiement de Komodo, plateforme de déploiement (PaaS auto-hébergé) pouvant héberger des conteneurs à la demande, sous le principe d'un contrôleur surveillant un dépôt Git et la [configuration associée]. Cela permet tout particulièrement de déployer une application conteneurisée sans avoir à créer d'instance dédiée (utile pour les petits services en compose, typiquement comme un site web avec du CRUD simple). L'accès au service ainsi déployé se fait par un domaine, avec les labels Traefik précisant l'hôte demandé (le trafic du serveur / PVE non décrit dans le Caddy central est ainsi redirigé vers cette VM Komodo, puis redistribué par Traefik selon les labels des conteneurs). Le plus grand avantage est que la configuration est dictée par le service cible à installer ([komodo.toml](https://) et [labels Traefik](https), et non par l'hôte installateur qu'est Komodo. Pour voir un exemple parlant, consultez l'exemple d'un simple site frontend Vite sur [blavogiez.github.io](https://github.com/blavogiez/blavogiez.github.io) et les [services dédiés du dépôt](https://github.com/jobacogiez-org/proxmox-gitops/main/services). 

Tous ces points sont automatiquement déployables par playbooks Ansible, avec une attention particulière à la personnalisation réalisée par Jinja2 (fichiers de template générés selon les variables). à la fin du sprint, le playbook permet, pour le déploiement d'un service quelconque, d'obtenir les secrets relatifs à celui ci en allant chercher ce chemin dans OpenBao, de copier le service vers la machine hôte, puis de générer les fichiers en injectant les variables, grâce aux templates Jinja2. Ces réalisations permettent donc d'être plus extensible pour l'implémentation d'un nouveau service, autant au niveau du monitoring avec un agent uniforme sur chaque instance, que de la gestion de secrets et de leur utilisation dans le déploiement. Cette polyvalence des déploiements est notamment permise grâce à l'arborescence du dépôt qui a été pensée par Jonas. 

Enfin, sans être un livrable à part entière mais plutôt un livrable support, les sous-réseaux ont été retravaillés, en utilisant les [SDN Proxmox](https://) décrits par Terraform (je considère les SDN comme un framework réseau pour Proxmox) et un DNS privé, permettant de restreindre des sites à un accès privé / VPN (par exemple, openbao est hébergé sous vault.priv.blavogiez.fr, sous domaine résolu par ce DNS privé (CoreDNS))

Il y a eu une bonne complémentarité avec Jonas, qui a réalisé des scripts d'initialisation du serveur / PVE avec un ordre défini (création d'openbao en premier, puis création assistée des secrets, permettant ensuite l'application de la configuration Terraform crééant tout le reste), en améliorant également la configuration Terraform et les modules. 

## Points marquants / Difficultés

L'utilisation de OpenBao post-déploiement a été la principale difficulté pour nous deux.
Il fallait dans un premier temps rentrer les secrets, ce qui a été facilité par le script de Jonas qui demande à l'utilisateur les valeurs et les injecte directement en lançant les commandes type (`bao kv put ...`) en arrière plan. Les lire a nécessité pour les playbook Ansible de se documenter sur le module et ses spécificités (par exemple, avoir la library python hvac sur la machine exécutant le playbook).
Cela dit, on s'attendait à cela en utilisant OpenBao / HashiCorp Vault, car il est dit qu'il est très puissant mais également complexe. C'est probablement surdimensionné pour nos besoins, mais nous faisons ce choix pour apprendre dessus car il s'agit souvent d'un standard dans la gestion d'infrastructure.

## Points d'amélioration

En résumé, voici les points d'améliorations que j'ai remarqué :


## Victoires

En résumé, voici les améliorations que j'ai remarqué :
- j'ai passé plus de temps à la planification / recherche de la meilleure solution, en passant d'environ 15% du temps d'un ticket à environ 25% aujourd'hui, comme le préconisait la précédente revue pour Mai 2026
- les tâches étaient bien définies dès le début et je n'ai pas eu à en rajouter entre temps, les besoins ont donc été assez bien prévus

## La suite

On embarque sur le [sprint de Juillet](https://kanban.blavogiez.fr/public/board/2ac60474026e68fb16da9fc1b0a2ff9d0f54cd9e76224b62cbc9aab56708),
Je prévois de continuer sur le sujet Proxmox. Pour les implémentations précises, je verrai avec Jonas dans les prochains jours pour voir les besoins que nous aurons. Un besoin direct que je trouve important est néanmoins l'utilisation de HTTPS pour le réseau privé (actuellement nous avons HTTPS pour les services publics et les requêtes sont redirigées en interne par HTTP, jusqu'ici pour simplifier), afin d'avoir une sécurisation maximale, même sur notre réseau domestique. Les certificats devront être signés et reconnus publiquement (pas de auto signés). Pour agir directement, j'ai mis un TLS auto signé pour OpenBao (le docker compose de OpenBao a un "sidecar" caddy qui fait le HTTPS) car il s'agit du service le plus important. Ce sera je pense un des premiers tickets du sprint 

Je prévois également de finaliser mon projet OpenLaTeX, notamment au niveau de l'infrastructure. En effet il y a 3 mois je pensais que l'infrastructure était bonne, mais avec mon regard d'aujourd'hui et ce que j'ai appris depuis, notamment sur Kubernetes (par les entraînements CKA/CKAD), je trouve qu'il y a des choses à revoir (par exemple faire plus de NetworkPolicy, de health / liveness probes, de limites de ressources clairement définies) et le niveau actuel de l'infrastructure ne me convient plus.

Ce sprint sera plus léger puisque pour ce mois-ci je serai plus occupé à d'autres sujets externes à l'informatique.

Review du sprint de Juillet : (lien une fois réalisée)

