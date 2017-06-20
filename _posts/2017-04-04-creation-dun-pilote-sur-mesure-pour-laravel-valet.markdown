---
layout: post
title:  "Création d'un pilote sur mesure pour Laravel Valet"
date:   2017-04-04
---

<p class="intro">
  <span class="dropcap">D</span>eux semaines passées, j'ai publié un article sur ma transition vers Laravel Valet.
  Aujourd'hui, je vais expliquer comment il est possible de créer un pilote sur mesure pour adapter le serveur aux besoins de votre projet.
</p>

## Qu'est-ce qu'un pilote?

Le pilote ou &laquo;driver&raquo; dicte comment doit être servi un logiciel PHP. De base, Laravel Valet supporte les plateformes suivantes:

- Laravel
- Lumen
- Bedrock
- CakePHP 3
- Craft
- Jigsaw
- Slim
- Statamic
- Static HTML
- Symfony
- WordPress
- Zend

Si vous avez l'intention d'utiliser Drupal, Joomla! ou toute autre plateforme non supportée avec Laravel Valet, vous aller avoir besoin de créer un pilote sur mesure ou d'en utiliser un créé par la communauté.

## Création d'un pilote

Tout d'abord, copiez le fichier `~/.valet/Drivers/SampleValetDriver.php` en prenant soin de renommer `SampleValetDriver.php` pour quelque chose d'autres comme `MonValetDriver.php`.

Pour écrire votre propre pilote, vous devez implémenter trois méthodes essentielles :

1. La méthode `servers` doit retourner `true` si votre pilote doit gérer la requête entrante.
2. La méthode `isStaticFile` doit déterminer si la demande entrante concerne un fichier statique telle qu'une image ou une feuille de style.
3. La méthode `frontControllerPath` doit renvoyer le chemin d'accès complet au <a href="https://en.wikipedia.org/wiki/Front_controller" title="Voir sur Wikipedia" target="_blank">contrôleur frontal</a> de votre application, qui est généralement votre fichier `index.php` ou l'équivalent.

## Exemple d'implémentation

Voici un pilote sur mesure que j'ai développé pour un projet multilingue et multidomaine sur la fantastique plateforme <a href="https://craftcms.com/" title="Site Web CraftCMS" target="_blank">CraftCMS</a>.

```php
<?php
class CraftLocalizedValetDriver extends ValetDriver {
  public function serves($sitePath, $siteName, $uri) {
    return is_dir($sitePath . '/craft');
  }
  public function isStaticFile($sitePath, $siteName, $uri) {
    if (file_exists($staticFilePath = $sitePath . '/public/' . $uri)) {
      return $staticFilePath;
    }
    return false;
  }
  public function frontControllerPath($sitePath, $siteName, $uri) {
    $frontControllerPath = "/public/index.php";
    $pathInfo            = "/index.php";
    if (strpos($siteName, '.') !== false) {
      $siteNameParts = explode('.', $siteName);
      $rootPath      = dirname($sitePath) . '/' . $siteNameParts[1];
      switch($siteNameParts[0]) {
        case 'en':
          $pathInfo            = "/public/index.php";
          $frontControllerPath = $rootPath . '/public/index.php';
          break;
        case 'fr':
          $pathInfo            = "/public_fr/index.php";
          $frontControllerPath = $rootPath . '/public_fr/index.php';
          break;
      }
      $_SERVER['SERVER_NAME']     = $_SERVER['HTTP_HOST'];
      $_SERVER['SCRIPT_FILENAME'] = $frontControllerPath;
      $_SERVER['PATH_INFO']       = $uri;
      $_SERVER['PHP_SELF']        = $pathInfo;
      $_SERVER['SCRIPT_NAME']     = '/index.php';
      return $frontControllerPath;
    }
    return $sitePath . '/public/index.php';
  }
}
```

Par défaut, Laravel Valet ne supporte pas les installations multidomaines. Pour arriver à mes fins, j'ai créé un dossier `monprojet`, `en.monprojet` et `fr.monprojet` avec des liens symboliques pour faire fonctionner le tout avec mon pilote sur mesure.

## Conclusion

Avec un peu d'imagination et de débrouillardise, il est possible de servir beaucoup plus d'application avec Laravel Valet que ce qui est suggéré &laquo;out-of-the-box&raquo; sur son site web.
De mon côté, je continue à explorer et tester ses limites.