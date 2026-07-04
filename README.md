# Dépôt F-Droid personnel — SafeTracks

Dépôt F-Droid statique pour l'app Android **SafeTracks** (gestion/visualisation de traces GPX avec
analyse des restrictions vélo), généré avec [fdroidserver](https://gitlab.com/fdroid/fdroidserver)
et publié via GitHub Pages.

## Ajouter ce dépôt dans le client F-Droid

Dans F-Droid → **Paramètres → Dépôts → Ajouter un dépôt** (ou via QR code / lien), renseigner :

- **URL** : `https://xavierlpro-coder.github.io/safetracks-fdroid/repo`
- **Empreinte (fingerprint)** : `F1374022A37ABBFBF64C7F00D89BAB1352A698B1BE62EC16E0D6BB1449DDD5FF`

(ou l'URL combinée `https://xavierlpro-coder.github.io/safetracks-fdroid/repo?fingerprint=F1374022A37ABBFBF64C7F00D89BAB1352A698B1BE62EC16E0D6BB1449DDD5FF`)

## Mettre à jour le dépôt (nouvelle release)

**Voie normale — automatisée en CI.** Pousser un tag `vX.Y.0` sur le dépôt principal SafeTracks déclenche
`.github/workflows/release-fdroid.yml` : build + signature de l'APK, release GitHub avec l'APK en pièce
jointe, puis mise à jour de **ce** dépôt (`repo/` + `metadata/`) via une clé de déploiement SSH dédiée
(accès write limité à ce seul dépôt). Rien ne se publie sans ce push de tag délibéré.

**Voie de secours — manuelle, en local.** Workspace local (non versionné ici) :
`C:\Users\xavierl\Workspace\safetracks-fdroid\`, contient `config.yml` + `keystore.p12` (clé de signature
du **dépôt**, distincte de la clé de signature de l'app — à sauvegarder précieusement, sa perte empêche de
publier une mise à jour crédible du dépôt).

1. Builder l'APK release signé côté SafeTracks (`assembleRelease`, cf. `docs/GUIDE-PUBLICATION.md`).
2. Copier l'APK dans `repo/` sous le nom `com.safetracks_<versionCode>.apk`.
3. Depuis le workspace local (Windows — `apksigner`/`jarsigner`/`keytool` n'y sont pas détectés
   nativement par fdroidserver, d'où le wrapper ; inutile côté CI, qui tourne sur Linux) :
   ```
   python _fdroid_win_wrapper.py update
   ```
4. Committer/pousser le contenu de `repo/` et `metadata/` (ce dépôt Git).

Voir `docs/BACKLOG.md` (idée **I55**) et `docs/NOTE-COMPAT-EMUI12.md` dans le dépôt principal
SafeTracks pour le contexte complet.
