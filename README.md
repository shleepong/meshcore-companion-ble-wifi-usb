# MeshCore Companion : BLE + WiFi + USB simultanés

Firmware communautaire pour le rôle Companion de [MeshCore](https://github.com/meshcore-dev/MeshCore),
basé sur le tag officiel `companion-v1.17.1`. Trois transports actifs en même temps
(Bluetooth, WiFi, USB série), configurables une fois puis basculables à chaud depuis
l'écran de l'appareil, sans redémarrage.

**Solution de dépannage, pas un projet officiel.** Ce dépôt existe parce qu'aucun
firmware companion multi-transport n'est encore proposé en amont. Le support natif
déjà présent côté officiel (`MultiSerialInterface`) va clairement dans cette direction :
dès qu'une solution équivalente sera proposée par le projet MeshCore lui-même, ce dépôt
n'aura plus lieu d'être maintenu séparément.

## Ce que ça apporte

- **BLE actif dès le premier démarrage**, comme n'importe quel companion classique.
- **USB série** (115200 bauds) : le protocole companion complet, en plus du BLE.
- **WiFi configurable** (SSID/mot de passe), rejoint le réseau au démarrage si activé,
  tourne **en plus** du BLE et de l'USB, pas à leur place.
- **Page écran "WIFI"** dédiée : bascule le WiFi on/off à chaud, sans redémarrage.
- Aucun identifiant WiFi en dur dans le firmware.

## Boards supportées

| Board | Statut |
|---|---|
| Heltec V4 / V4.3 | Testé sur matériel réel (voir `firmware/heltec_v4/`) |
| Heltec Wireless Tracker V2 | Démarre sur matériel réel, configuration WiFi en cours de validation (voir `firmware/heltec_tracker_v2/`) |

## Utilisation rapide

1. **Flasher** : `firmware/<board>/firmware-merged.bin` (board neuve, à l'offset `0x0`,
   efface tout) ou `firmware.bin` (mise à jour d'un board déjà sous MeshCore, via
   [meshcore.io/flasher](https://meshcore.io/flasher) ou en série).
2. **Configurer le WiFi** : ouvrir la [page de configuration](https://shleepong.github.io/meshcore-companion-ble-wifi-usb/)
   hébergée par ce dépôt, en Bluetooth ou en USB, aucune installation requise
   (Chrome ou Edge, Windows ou Android ; Safari/iOS non supporté, restriction du
   navigateur, pas de ce firmware). **Code d'appairage Bluetooth : celui affiché
   sur la page d'accueil de l'écran** (`Pin:XXXXXX`), différent à chaque
   démarrage, pas une valeur fixe. Ce code ne sert qu'au tout premier
   appairage : une fois l'appareil apparié, Windows/Android le reconnaît
   automatiquement aux connexions suivantes (y compris après une mise à jour
   du firmware) sans redemander de code, tant que l'appairage n'a pas été
   supprimé manuellement côté PC/téléphone. Alternative en ligne de commande :
   [`meshcore-cli`](https://github.com/meshcore-dev/meshcore-cli), voir
   `set wifi_ssid` / `set wifi_pwd` / `set wifi_enabled`.
3. **Basculer le WiFi au quotidien** : page "WIFI" sur l'écran de l'appareil, appui
   long sur le bouton.

## Construire soi-même

Les fichiers `.patch` dans `patches/` s'appliquent dans l'ordre sur le tag officiel
`companion-v1.17.1` du dépôt [meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore).
Voir `patches/README.md` pour le détail de chaque patch et la procédure.

## Problème connu en cours d'investigation

Connexion WiFi (TCP, port 5000) parfois capricieuse depuis certains téléphones Android
(observé sur Samsung, pas reproduit sur Pixel) après plusieurs bascules on/off sans
redémarrage. Piste principale : gestion agressive du WiFi par certaines surcouches
Android (mise en veille réseau arrière-plan, détection "réseau sans accès Internet").
Bluetooth et USB ne sont pas concernés. En observation, pas encore de correctif.

## Licence

MIT, héritée de [meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore)
(voir `LICENSE`). Ce dépôt ne fait qu'ajouter des patchs et des binaires précompilés
par-dessus le code officiel.
