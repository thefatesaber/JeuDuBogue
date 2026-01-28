# Jeu du Bogue

Un jeu d'arcade Flash classique où vous devez écraser des insectes (bibittes) qui traversent l'écran. Gagnez des points en passant la souris sur les bibittes avant qu'elles n'atteignent l'autre côté de l'écran.

## Description

Jeu du Bogue est un jeu de réflexes développé en ActionScript 3 pour Adobe Flash. Des insectes apparaissent régulièrement et traversent l'écran. Le joueur doit rapidement passer la souris dessus pour les écraser et marquer des points. Chaque insecte manqué fait perdre une vie. Le jeu devient progressivement plus rapide pour augmenter le défi.

## Prérequis

### Pour jouer (exécuter le .swf)

**ATTENTION:** Adobe Flash Player a été officiellement abandonné le 31 décembre 2020. Ce jeu nécessite un environnement Flash fonctionnel.

Options pour jouer:
- **Flashpoint** (recommandé) - Projet de préservation des contenus Flash: https://flashpointarchive.org/
- **Ruffle** - Émulateur Flash open-source: https://ruffle.rs/
- Lecteur SWF standalone (Flash Player Projector) - Version archivée

### Pour modifier (éditer le .fla)

- **Adobe Flash Professional CC** (ou version ultérieure)
- **Adobe Animate** (successeur de Flash Pro)
- Ou tout logiciel compatible avec le format .fla

## Installation

### Jouer au jeu

1. Clonez ou téléchargez ce repository:
   ```bash
   git clone <url-du-repo>
   cd JeuDuBogue
   ```

2. **Option A - Avec Flashpoint:**
   - Installez Flashpoint
   - Ajoutez `JeuDuBogue.swf` à votre collection
   - Lancez le jeu depuis l'interface Flashpoint

3. **Option B - Avec Ruffle (navigateur):**
   - Installez l'extension Ruffle pour votre navigateur
   - Ouvrez `JeuDuBogue.html` dans votre navigateur
   - Le jeu devrait se charger automatiquement

4. **Option C - Flash Player Projector:**
   - Téléchargez Flash Player Projector (version archivée)
   - Ouvrez `JeuDuBogue.swf` directement avec le lecteur

### Modifier le jeu

1. Ouvrez `JeuDuBogue.fla` avec Adobe Flash Professional ou Adobe Animate

2. Les fichiers sources ActionScript sont dans le dossier racine:
   - `Main.as` - Logique principale du jeu
   - `Bibitte.as` - Comportement des insectes

3. Après modification, publiez le projet (File → Publish) pour générer un nouveau `JeuDuBogue.swf`

## Configuration

Aucune configuration nécessaire. Tous les paramètres sont définis dans le code source.

### Paramètres modifiables (dans le code source)

Dans `Main.as`:
- `Timer(750)` - Intervalle d'apparition des bibittes en millisecondes (ligne 8)
- `vies = 10` - Nombre de vies initiales (ligne 15)
- `stage.frameRate += 2` - Augmentation de vitesse par bibitte (ligne 25)
- `stage.frameRate<=60` - Framerate maximum (ligne 25)

Dans `Bibitte.as`:
- `Math.floor(Math.random()*5)+1` - Nombre de chemins possibles (ligne 15)

## Usage

### Contrôles

- **Souris:** Passez la souris sur les insectes pour les écraser
- Aucun clic n'est nécessaire, le simple survol suffit

### Règles du jeu

- **Objectif:** Écraser un maximum d'insectes avant de perdre toutes vos vies
- **Points:** +1 point par insecte écrasé
- **Vies:** Vous commencez avec 10 vies
- **Pénalité:** -1 vie si un insecte traverse l'écran sans être écrasé
- **Difficulté:** Le jeu accélère progressivement (framerate augmente de 18 à 60 FPS)
- **Game Over:** La partie se termine quand vos vies tombent à 0

### Interface

- **Coin supérieur gauche:**
  - `Points: X` - Votre score actuel
  - `Vies: X` - Nombre de vies restantes

## Structure du projet

```
JeuDuBogue/
├── Main.as                      # Classe principale du jeu
├── Bibitte.as                   # Classe des insectes
├── JeuDuBogue.fla               # Fichier source Flash (éditable)
├── JeuDuBogue.swf               # Jeu compilé (exécutable)
├── .gitattributes               # Configuration Git
│
└── JeuDuBogue/                  # Projet Flash décompressé (format XFL)
    ├── JeuDuBogue.xfl           # Fichier projet XFL
    ├── DOMDocument.xml          # Document principal Flash
    ├── PublishSettings.xml      # Paramètres de publication
    ├── MobileSettings.xml       # Paramètres mobiles
    │
    ├── LIBRARY/                 # Assets et symboles
    │   ├── mcBibitte.xml        # Symbole bibitte avec mouvement
    │   ├── mcCycleBibitte.xml   # Animation marche/mort
    │   ├── mcBibitteMorte.xml   # Bibitte écrasée
    │   ├── mcCorpsBibitte.xml   # Graphisme du corps
    │   ├── mcTableau.xml        # Interface score/vies
    │   ├── mcGameOver.xml       # Écran de fin
    │   ├── 1146519_64492589_600x450.jpg  # Image de fond
    │   └── Sploutch.mp3         # Son d'écrasement
    │
    ├── bin/                     # Fichiers compilés
    │   ├── M 3 1378776097.dat   # Données bitmap
    │   ├── M 4 1378832637.dat   # Données audio
    │   └── SymDepend.cache      # Cache de dépendances
    │
    └── META-INF/
        └── metadata.xml         # Métadonnées du projet
```

## Fonctionnement interne

### Architecture

Le jeu suit une architecture orientée objet simple avec trois classes principales:

1. **Main (Main.as):**
   - Hérite de `MovieClip`
   - Gère le timer de spawn (750ms)
   - Gère le score et les vies
   - Contrôle l'augmentation de difficulté
   - Affiche l'écran Game Over

2. **Bibitte (Bibitte.as):**
   - Hérite de `MovieClip`
   - Se positionne aléatoirement à l'initialisation
   - Détecte le survol de la souris
   - Communique avec Main pour le score et la suppression

3. **GameOver (lié à mcGameOver.xml):**
   - Simple MovieClip affiché à la fin
   - Animation de clignotement du texte

### Flow de jeu

```
[Initialisation]
    Main instancié → Timer démarre

[Boucle de jeu] (toutes les 750ms)
    Timer tick → Main.ajouterBibitte()
    → Nouvelle instance Bibitte créée
    → Bibitte.init() positionne aléatoirement
    → Animation de traversée démarre (51 frames)
    → FrameRate augmente (+2, max 60)

[Interaction joueur]
    MOUSE_OVER sur Bibitte
    → Bibitte.tuer()
    → Animation "meurt" + son "sploutch"
    → Main.ajouterPoint() (+1 score)
    → Frame 9: Bibitte.disparaitre()
    → Main.oublierBibitte() (suppression)

[Bibitte manquée]
    Frame 50 atteinte sans MOUSE_OVER
    → Main.enleverVie() (-1 vie)
    → Bibitte supprimée

[Condition de fin]
    Vies == 0
    → Main.terminerPartie()
    → Timer.stop()
    → GameOver affiché
```

### Système de positionnement

Les bibittes apparaissent à une position X calculée ainsi:
```actionscript
x = stage.stageWidth / (Math.floor(Math.random()*5)+1)
```

Cela crée 6 positions possibles:
- Diviseur 1: x = 600px (bord droit)
- Diviseur 2: x = 300px
- Diviseur 3: x = 200px
- Diviseur 4: x = 150px
- Diviseur 5: x = 120px
- Diviseur 6: x = 100px

Toutes partent du centre vertical (y = 225px) avec une rotation aléatoire.

## Dépannage

### Le jeu ne se charge pas dans le navigateur

**Cause:** Les navigateurs modernes bloquent Flash Player

**Solution:**
- Utilisez Flashpoint ou Ruffle (voir section Installation)
- Ou utilisez Flash Player Projector standalone

### "Adobe Flash Player is no longer supported"

**Cause:** Flash a été désactivé le 31/12/2020

**Solution:**
- Installez Flashpoint (émulateur Flash standalone)
- Ou utilisez Ruffle (émulateur Flash open-source)

### Le fichier .fla ne s'ouvre pas

**Cause:** Format de fichier trop récent ou trop ancien

**Solution:**
- Utilisez Adobe Animate CC ou version ultérieure
- Convertissez le .fla si nécessaire avec Adobe Animate

### Le jeu lag ou est trop rapide

**Cause:** Le framerate augmente avec le temps

**Solution:**
- Modifiez la limite de framerate dans `Main.as` ligne 25:
  ```actionscript
  if(stage.frameRate<=60){stage.frameRate += 2}; // Changez 60 ou 2
  ```

### Pas de son

**Cause:** Fichier Sploutch.mp3 manquant ou codec non supporté

**Solution:**
- Vérifiez que `JeuDuBogue/LIBRARY/Sploutch.mp3` existe
- Réimportez le son dans Flash si nécessaire

### Erreur "Cannot find class Bibitte"

**Cause:** Fichier Bibitte.as absent ou mal lié

**Solution:**
- Vérifiez que `Bibitte.as` est dans le même dossier que `Main.as`
- Dans Flash, vérifiez les paramètres de publication (ActionScript 3.0 Class Path)

### Game Over ne s'affiche pas

**Cause:** Symbole mcGameOver mal lié ou classe GameOver absente

**Solution:**
- Dans Flash, vérifiez les propriétés de mcGameOver
- Linkage doit être activé avec className = "GameOver"

## Contribution

Ce projet est un exemple éducatif d'un jeu Flash classique. Les contributions sont les bienvenues pour:

- Convertir le jeu vers des technologies modernes (HTML5/Canvas, Phaser.js, etc.)
- Améliorer les graphismes
- Ajouter de nouvelles fonctionnalités (niveaux, power-ups, etc.)
- Créer une version mobile (touch-friendly)

## Licence

Aucune licence spécifiée. Projet éducatif/historique.

## Notes de sécurité

### Sécurité Flash

**AVERTISSEMENT:** Adobe Flash Player contient de nombreuses vulnérabilités de sécurité connues. Adobe a officiellement mis fin au support le 31 décembre 2020.

**Recommandations:**
- **NE PAS** installer Flash Player sur votre système principal
- Utilisez Flashpoint (sandboxé) ou Ruffle (émulation sûre)
- Ne jouez PAS à des jeux Flash provenant de sources non fiables
- Ce jeu est un projet local sans connexion réseau (sûr)

### Données personnelles

- Ce jeu ne collecte aucune donnée
- Pas de connexion réseau
- Pas de cookies ou tracking
- Le score n'est pas sauvegardé (perdu à la fermeture)

## Contexte historique

Ce projet a été créé avec Adobe Flash Professional CC (probablement en 2013 d'après les timestamps). Flash était à l'époque la plateforme standard pour les jeux web interactifs. Ce projet représente un exemple typique de jeu Flash éducatif de cette période.

**Dates clés:**
- Création: ~2013 (d'après les dates dans DOMDocument.xml)
- Dernière modification: 2014 (d'après le commit Git)
- Fin de Flash Player: 31 décembre 2020

---

**Bon jeu!** 🐛💥
