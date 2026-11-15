# Timer Pro

Application personnelle de chronométrage sportif : Tabata, HIIT, boxe, MMA, JJB, grappling, EMOM, AMRAP, minuteur, chronomètre, multitimer et séances sur mesure.

Aucun serveur, aucun compte, aucune dépendance externe. Tout le code (HTML, CSS, JavaScript) et les polices sont dans `index.html`.

## Déployer sur GitHub Pages

1. Créez un dépôt GitHub, par exemple `timer-pro`.
2. Déposez à la racine le contenu de ce dossier : `index.html`, `manifest.webmanifest`, `sw.js`, `README.md` et le dossier `assets`.
3. Dans **Settings > Pages**, choisissez **Deploy from a branch**, la branche `main`, le dossier `/ (root)`.
4. L'application est en ligne quelques minutes plus tard, à l'adresse `https://votre-compte.github.io/timer-pro/`.

Tous les chemins sont relatifs : l'application fonctionne dans un sous-dossier.

Lors d'une mise à jour, changez `VERSION` en haut de `sw.js` pour renouveler les sons et icônes en cache. La page, elle, se met à jour seule dès que le réseau est disponible.

## Installer sur iPhone

Ouvrez l'adresse dans **Safari**, touchez **Partager**, puis **Sur l'écran d'accueil**.

## Essayer en local

Le service worker et le chargement du gong exigent une adresse `http://` :

```
cd timer-pro
python3 -m http.server 8000
```

Puis ouvrez `http://localhost:8000`.

## Le gong

`assets/sounds/boxing-gong.wav` est un **enregistrement réel**, mais **pas une cloche de ring**. Aucun enregistrement de cloche de ring n'a pu être téléchargé depuis l'environnement de développement. Le fichier fourni est une frappe de cloche tubulaire (fa 5, frappe la plus forte), choisie pour son attaque métallique et sa résonance. Il a été coupé à 7 secondes, filtré sous 170 Hz, atténué en fin de résonance, converti en mono et normalisé.

Pour obtenir une vraie cloche de ring, deux enregistrements annoncés en licence CC0 sont identifiés :

- [G39-09-Boxing Fight Bell](https://freesound.org/s/438626), par craigsmith, sur Freesound (compte gratuit nécessaire). C'est un effet sonore Hollywood ancien numérisé : un souffle de fond est possible.
- `boxing_matchbell.wav`, dans [Boxing Ring](https://opengameart.org/content/boxing-ring-0), par Umplix, sur OpenGameArt.

Deux façons de l'utiliser :

- **Dans l'application** : Réglages > Sons > Importer un son, puis **Utiliser comme gong**.
- **Dans le projet** : remplacez `assets/sounds/boxing-gong.wav` par le fichier (au format WAV), puis changez `VERSION` dans `sw.js`.

Si le fichier du gong manque ou est illisible, l'application joue un gong de synthèse et l'indique clairement dans Réglages > Sons.

## Crédits et licences

| Élément | Source | Licence |
| --- | --- | --- |
| `boxing-gong.wav` | `Idiophones/Struck Idiophones/Tubular Bells 2/TB_hit_F5_v4_1.wav`, [Versilian Community Sample Library](https://github.com/sgossner/VCSL), Versilian Studios | CC0 1.0 |
| `handbell.wav` | `Idiophones/Struck Idiophones/Hand Bells, Nepalese/HB_1.wav`, même bibliothèque | CC0 1.0 |
| Bips, signal, décompte | Calculés par l'application (synthèse) | Sans objet |
| Polices Big Shoulders, Barlow | Embarquées dans `index.html` | SIL Open Font License 1.1 |
| Icônes | [Lucide](https://lucide.dev), intégrées à `index.html` | ISC |

Les couleurs sont une proposition personnelle : ce ne sont pas celles d'une application existante.

## Limites techniques

**iPhone et iPad**

- Sons et annonces **ne sont pas garantis** quand l'application est fermée, l'écran verrouillé ou l'application en arrière-plan : iOS suspend alors les applications web.
- Le temps reste juste : il est recalculé au retour à partir de l'horloge de l'appareil. Les signaux manqués ne sont pas rejoués en rafale ; l'application indique combien sont passés.
- Le premier son exige de toucher l'écran : c'est une règle des navigateurs.
- Le bouton Silencieux peut couper les sons. L'option *Sonner malgré le mode Silencieux* n'apparaît que si le navigateur propose l'API correspondante ; elle peut interrompre la musique en cours.
- La vibration n'existe pas dans Safari sur iPhone.
- Le mode plein écran n'existe pas dans Safari sur iPhone : installez l'application sur l'écran d'accueil pour masquer la barre du navigateur.
- Le maintien de l'écran allumé dépend de la version d'iOS. Réglages > Appareil et limites indique ce que votre appareil prend en charge.

**Tous les appareils**

- Le choix et la qualité des voix dépendent du système. Sans voix installée dans une langue, le système en choisit une autre.
- Après une actualisation, le son reste muet jusqu'au prochain contact avec l'écran.
- Les données sont stockées dans le navigateur (IndexedDB et localStorage). Effacer les données du site les supprime : exportez régulièrement une sauvegarde depuis Réglages > Données.
- Safari peut effacer le stockage d'un site non installé après plusieurs semaines sans visite. L'installation sur l'écran d'accueil protège mieux les données.
- En navigation privée, les données peuvent disparaître à la fermeture.

## Le son ne marche plus

Sur iPhone, un verrouillage, un appel ou une autre application qui prend le son peut bloquer le son d'une page web. L'application surveille son moteur audio : s'il se bloque, le bouton « Touchez pour réactiver le son » apparaît pendant la séance et un toucher le reconstruit. Réglages > Sons > Tester le son et la voix indique l'état réel et fait un essai.

Les sons font partie de l'application : ils sont enregistrés sur le téléphone à la première ouverture et fonctionnent sans réseau. Si l'option « Sonner malgré le mode Silencieux » est activée, iOS affiche son lecteur (écran verrouillé, centre de contrôle), nommé « Timer Pro ». Désactivée, il n'y a pas de lecteur et les sons se mêlent à votre musique, mais le mode Silencieux les coupe.

Si le test reste muet : vérifiez le bouton Silencieux de l'iPhone (ou activez « Sonner malgré le mode Silencieux »), le volume du téléphone et le volume général de l'application.

## Mettre à jour

Après chaque nouvelle version, changez `VERSION` dans `sw.js`. Sur iPhone, l'icône de l'écran d'accueil est figée à l'installation : pour voir une nouvelle icône, supprimez l'application de l'écran d'accueil puis ajoutez-la de nouveau depuis Safari. Attention : sur iPhone, supprimer l'application efface ses données. Exportez-les avant (Réglages > Données > Exporter tout), puis importez le fichier dans l'application réinstallée.

## État des tests

Effectués dans Chrome : 30 tests du moteur sur horloge simulée et 37 tests de bout en bout (audio Web Audio réel, séances complètes, reprise après gel et actualisation, import et export, hors connexion, données invalides, multitimer, largeur 320 px).

Non effectués : essai sur un vrai iPhone ou dans Safari, écoute humaine du gong. Le gong a été choisi d'après ses mesures (spectre, attaque, décroissance, absence d'écrêtage).

## Fonctionnement du moteur

Le temps n'est jamais compté : il est calculé. Chaque séance est compilée en phases, puis en repères datés sur une ligne de temps. La position résulte de la différence entre deux horodatages (`performance.now()`, corrigé de la veille grâce à `Date.now()`). Un Web Worker donne un battement indépendant de l'affichage. Chaque repère porte une clé unique, ce qui empêche tout double déclenchement.
