# PrestaShop - Overrides

> **ATTENTION**<br />
> PrestaShop recommande de n'avoir recours aux overrides qu'en cas d'extrême nécessité. Le plus souvent, il faudra passer par les alternatives : hooks, héritage des classes natives, décoration des services Symfony. Tout cela, en passant par la création d'un module.

> **INFORMATION**<br/>
Il est impossible d'overrider un thème depuis un module. Toutefois, les templates des modules ou leur CSS / JS peuvent être modifiés par les thèmes.

## Sommaire

- [Idées](#ideas)
- [Overrides](#overrides)
- [Alternatives aux overrides](#overrides-alternatives)
    - [Hooks](#hooks)
    - [Extension de classes](#classes)
- [Controllers](#controllers)
- [Formulaires (PS 1.7.4+)](#forms)

## <a id="ideas"></a>Idées

- Modifier les fichiers natifs de PrestaShop en précisant les lignes modifiées via un texte précis et facilement retrouvable via une recherche pour éviter les conflits d'override entre les différents modules.

## <a id="overrides"></a>Overrides

De manière générale, lorsque vous avez recours à un override pour ajouter / modifier / supprimer une fonctionnalité à votre PrestaShop, il vous faudra créer une classe, dans le dossier `override` de PrestaShop, qui héritera de la classe native du CMS :

```php
<?php

// /override/controllers/front/ProductController.php

if !(defined('_PS_VERSION_')) {
    exit;
}

class ProductController extends ProductControllerCore
{
    // ...

    /**
     * @author John Doe <john@doe.com>
     * @since 2023-09-18 12:16:00
     */
    public function initContent()
    {
        $this->myFeature();

        parent::initContent();
    }

    /**
     * @author John Doe <john@doe.com>
     * @since 2023-09-18 12:16:00
     */
    private function myFeature()
    {
        // ...
    }

    // ...
}
```

> **IMPORTANT**<br />
> Il est fortement recommandé de créer une méthode à part de celle dont vous modifiez le comportement afin de conserver votre script dans le cas où un module ajouterait un override sur la même méthode. Ainsi, vous n'avez qu'à rappeler votre méthode dans celle dont vous souhaitez modifier le comportement.

## <a id="overrides-alternatives"></a>Alternatives aux overrides

### <a id="hooks"></a>Hooks

Source : https://devdocs.prestashop-project.org/1.7/modules/concepts/hooks

Il est possible de greffer un module sur un ou plusieurs hooks disponibles dans le thème du site marchand.

Il est également possible de créer ses propres hooks et de les ajouter dans le thème afin que notre module personnalisé puisse se greffer dessus sans forcément qu'un autre module ne s'y greffe ou ne l'exploite (source : https://devdocs.prestashop-project.org/1.7/modules/concepts/hooks/#going-further-creating-your-own-hook).

### <a id="classes"></a>Extension de classes

Si une classe de PrestaShop ne correspond pas à vos besoins, il est possible d'en créer une qui héritera de celle-ci dans un module afin de rajouter des méthodes.

Exemple :

```php
<?php

class MyCustomTools extends Tools
{
    public static function getCurrentUrl()
    {
        // ...
    }
}
```

## <a id="controllers"></a>Controllers

Source : https://devdocs.prestashop-project.org/1.7/modules/concepts/controllers

Il ne semble pas possible de rajouter ou de modifier un controller sans passer par des overrides.

La documentation de PrestaShop évoque toutefois la décoration (override) de services Symfony à partir des versions 1.7.7 pour les controllers (source : https://devdocs.prestashop-project.org/8/modules/concepts/controllers/admin-controllers/override-decorate-controller).

Cependant, après avoir testé, il est nécessaire de réécrire l'intégralité des méthodes de la classe.

## <a id="forms"></a>Formulaires (PS 1.7.4+)

Source : https://devdocs.prestashop-project.org/1.7/modules/concepts/forms/admin-forms

> **ATTENTION**<br />
> Les exemples fournis dans la documentation de PrestaShop ne semblent pas fonctionner.

Si vous souhaitez modifier une `Grid` dans PrestaShop, vous pouvez le faire à l'aide des hooks de type :

- `action<Entity>GridDefinitionModifier` : Permet d'ajouter ou supprimer des colonnes ou des actions dans la `Grid`.
- `action<Entity>GridQueryBuilderModifier` : Permet de modifier la requête liée à l'entité et d'ajouter des requêtes SQL.
- `action<Entity>FormBuilderModifier` : Permet de modifier le formulaire de l'entité et d'ajouter des champs additionnels comme les modifier ou ajouter de nouvelles données au formulaire.
- `actionAfterUpdate<Entity>FormHandler` : Permet d'enregistrer le formulaire de l'entité et ses nouvelles données après sa mise à jour.
- `actionAfterCreate<Entity>FormHandler` : Permet d'enregistrer le formulaire de l'entité et ses nouvelles données après sa création.

> **INFORMATION**<br />
> Il est également possible de modifier les champs natifs de l'entité. Il n'est cependant pas recommandé de le faire.

## Menu

- Overrides

[Retour](index.md)
