# Laravel TALL StarterKit

## À propos de ce kit de démarrage

L'objectif est d'avoir une base de code pour démarrer un projet Laravel incluant :
- [x] La dernière version à jour (Laravel 9)
- [x] L'extension **[laravel-debugbar](https://github.com/barryvdh/laravel-debugbar)**
- [x] L'extension **[laravel-ide-helper](https://github.com/barryvdh/laravel-ide-helper)**
- [ ] Mise en place de la stack TALL (Tailwind CSS, AlpineJS, Livewire) avec gestion authentification
- [x] Favicon

Ainsi que la configuration de base de l'IDE PhpStorm.

## Mise en place du starter kit

### Création du projet

````shell
laravel new laravel-tall-starter_kit --git
````
### Installation debugbar

```shell
composer require --dev barryvdh/laravel-debugbar
```

* Puis chargement de la home pour ajout automatique du .gitignore
* Ajout de la variable `DEBUGBAR_ENABLED=true` au fichier `.env`.

### Installation ide-helper

```shell
composer require --dev barryvdh/laravel-ide-helper
```

* Puis publication du fichier de config `config/ide-helper.php` et configuration de diverses options.
```shell
php artisan vendor:publish --provider="Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider" --tag=config
```
* Ajout des fichiers au `.gitignore`

### Installation preset TALL

```shell
composer require livewire/livewire laravel-frontend-presets/tall
```
* diverses corrections, suppression sass et sass-loader

## Divers

### Correction config Vite avec environnement sécurisé TLS

À partir du moment où on sécurise le projet (valet secure) le serveur de dev généré par vite nécessite d'utiliser l'hôte (host).
En compilant un article sur freek.dev, la doc laravel-vite et ce gist (récupération dynamique du host), on obtient le fichier vite.config.js :
```js
import { defineConfig, loadEnv } from 'vite'
import laravel from 'laravel-vite-plugin'

export default defineConfig(({ mode}) => {
    // Load current .env-file
    const env = loadEnv(mode, process.cwd(), '')
    // Set the host based on APP_URL
    // noinspection JSUnresolvedVariable
    let host = new URL(env.APP_URL).host
    return {
        plugins: [
            laravel({
                input: [
                    'resources/css/app.css',
                    'resources/js/app.js'
                ],
                refresh: true,
                valetTls: host
            }),
        ]
    }
})
```

### Favicon

La config Nginx de valet pose toujours souci cf [ce ticket](https://github.com/laravel/valet/issues/375#issuecomment-1347146695).

Une fois le site lié et sécurisé (`valet link` et `valet scecure`) il faut modifié le fichier `~/.config/valet/Nginx/<PROJECT_NAME>>.test` :

```nginx
# remplacer
location = /favicon.ico { access_log off; log_not_found off; }
location = /robots.txt  { access_log off; log_not_found off; }

# par
location = /public/favicon.ico { access_log off; log_not_found off; }
location = /public/robots.txt  { access_log off; log_not_found off; }
```

## Post-installation

* supprimer le preset tall
```shell
composer remove laravel-frontend-presets/tall
```

## Configuration PhpStorm

* Exclusion des répertoires `vendor` et `node_modules`
