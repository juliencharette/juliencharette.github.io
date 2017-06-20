---
layout: post
title:  "Environnement de développement avec Laravel Valet"
date:   2017-03-21
---

<p class="intro">
  <span class="dropcap">C</span>haque développeur doit polir son environnement de développement de temps à autre,
  afin de rester à jour avec les dernières technologies. Dans mon cas, ce fût une agréable transition de <a href="https://www.vagrantup.com/" title="Vagrant" target="_blank">Vagrant</a> vers <a href="https://laravel.com/docs/5.4/valet" title="Laravel Valet" target="_blank">Laravel Valet</a>.
</p>

<img src="{{ '/assets/img/environnement-de-developpement-vagrant.jpg' | prepend: site.baseurl }}" alt="Vagrant - Environnement de Développement Simplifié">

## Vagrant

Vagrant permet de faciliter la création de machines virtuelles sur VirtualBox et VMWare. C'est un outil relativement simple à apprendre et accessible à tous puisqu'il est gratuit. Cependant, Vagrant ne fait qu'assister un développeur dans la création d'une machine virtuelle.
Ce n'est pas une solution <a href="https://fr.wikipedia.org/wiki/LAMP" title="LAMP sur Wikipedia" target="_blank">LAMP</a> clé en main tel que <a href="https://www.mamp.info/en/" title="MAMP" target="_blank">MAMP</a> et <a href="http://www.ampps.com/" title="AMPPS" target="_blank">AMPPS</a>.
Vous devez être en mesure d'installer et configurer les composantes de votre serveur web.

**Les points forts :**

1. Contrôle total sur la technologie et configuration du serveur.
2. Environnement de développement contenu dans son propre système.
3. Facile de &laquo;packager&raquo; l'environnement et de la partager.
3. Multiplateforme (OSX, Linux, Windows).

**Les points faibles :**

1. Plus lent et plus lourd qu'une solution a même la machine locale.
2. Plus long à configurer même s'il est possible d'automatiser plusieurs opérations avec des outils comme Chef, Ansible et Puppet.

> La plupart de mes projets sont de taille modeste, je dois donc maximiser ma productivité...

Lorsque je travaille sur des projets ayant des besoins et configurations particulières, c'est mon outil de choix car j'ai un contrôle total sur la technologie. Par contre,
la plupart de mes projets sont de taille modeste, je dois donc maximiser ma productivité et dépenser moins de temps à configurer un environnement de développement local.

C'est ici qu'intervient **Laravel Valet**.

<img src="{{ '/assets/img/laravel-valet.jpg' | prepend: site.baseurl }}" alt="Laravel Valet - Environnement de Développement Minimaliste pour Mac">

## Laravel Valet

Laravel Valet est un environnement de développement minimaliste sous Mac OS. Il a été développé par <a href="https://twitter.com/taylorotwell" title="Profile Twitter de Taylor Otwell" target="_blank">Taylor Otwell</a>,
le créateur du framework PHP <a href="https://laravel.com/" title="Laravel" target="_blank">Laravel</a>. Contrairement à Vagrant, vous n'avez pas besoin de connaître ni de configurer le serveur web et c'est également gratuit.

Laravel Valet configure votre Mac afin que celui-ci exécute en permanence <a href="https://www.nginx.com/resources/wiki/" title="Nginx" target="_blank">Nginx</a> et PHP7 préalablement installés, puis utilise la technologie
<a href="https://fr.wikipedia.org/wiki/Dnsmasq" title="DnsMasq sur Wikipedia" target="_blank">DnsMasq</a> pour renvoyer toutes les requêtes depuis `*.dev` aux sites installés sur votre machine locale.

**Les points forts :**

1. Faible utilisation du RAM (plus ou moins 7MB).
2. Utilise le &laquo;filesystem&raquo; local.
3. Redirection automatique des domaines `*.dev`.
4. Supporte <a href="https://laravel.com/docs/5.4/valet#introduction" target="_blank">plusieurs frameworks et CMS</a>
5. Partage simplifié un site local en une ligne de commande.
6. Utiliser HTTP2 avec encryptage TLS en une ligne de commande.

**Les points faibles :**

1. Disponible seulement sous Mac OS.
2. Ne peut être partagé à ses collègues.

## Conclusion

Les deux technologies ont leur propre cas d'utilisation, mais ce que je préfère par-dessus tout avec Laravel Valet est
la vitesse à laquelle je peux démarrer un projet. Je n'ai besoin que de quelques minutes pour créer un dossier et installer
le CMS de mon choix avant de me lancer dans le code, chose qui pouvait prendre des heures auparavent avec Vagrant.

Je vous recommande fortement d'y jetter un oeil.

<a href="https://laravel.com/docs/5.4/valet" title="Laravel Valet - Documentation" target="_blank">Laravel Valet - Documentation officielle</a>