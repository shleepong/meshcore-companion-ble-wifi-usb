# MeshCore Companion : BLE ou WiFi (exclusif) + USB

Firmware communautaire pour le rôle Companion de [MeshCore](https://github.com/meshcore-dev/MeshCore),
basé sur le tag officiel `companion-v1.17.1`. Bluetooth **ou** WiFi, un seul actif à
la fois, la bascule se faisant par un redémarrage complet ; l'USB reste toujours
disponible dans les deux cas. Configurable via une page web, sans installation.

**Solution de dépannage, pas un projet officiel.** Ce dépôt existe parce qu'aucun
firmware companion multi-transport n'est encore proposé en amont. Le support natif
déjà présent côté officiel (`MultiSerialInterface`) va clairement dans cette direction :
dès qu'une solution équivalente sera proposée par le projet MeshCore lui-même, ce dépôt
n'aura plus lieu d'être maintenu séparément.

**Note historique** : une première version de ce firmware faisait tourner Bluetooth et
WiFi en même temps (`DUAL_BLE_WIFI`). Ça fonctionne bien sur une board avec PSRAM
(Heltec V4.3), mais bloque indéfiniment la connexion WiFi sur une board sans PSRAM
(Wireless Tracker V2) : les deux radios réservent chacune leurs propres tampons
mémoire au démarrage, ce qui ne laisse pas assez de marge pour que le WiFi termine sa
connexion. Plutôt que de maintenir deux mécanismes différents selon la board, seul le
mode exclusif (`EXCLUSIVE_BLE_WIFI`) est désormais proposé et maintenu, sur les deux
boards.

## Ce que ça apporte

- **BLE actif dès le premier démarrage**, comme n'importe quel companion classique.
- **USB série** (115200 bauds) : le protocole companion complet, en plus du BLE,
  toujours disponible quel que soit le mode actif.
- **WiFi configurable** (SSID/mot de passe), rejoint le réseau au démarrage si activé,
  à la place du BLE (jamais les deux en même temps).
- **Page écran dédiée** ("WIFI" et "BLUETOOTH") : un appui long redémarre la carte
  dans l'autre mode.
- **Pourcentage de batterie affiché** (au lieu d'une icône), plus une page dédiée avec
  tension mesurée et plage de référence.
- **Protocole actif signalé en cyan** sur les pages BLUETOOTH et WIFI (gris quand
  désactivé), sur les boards à écran couleur.
- Aucun identifiant WiFi en dur dans le firmware.

## Boards supportées

| Board | Statut |
|---|---|
| Heltec V4 / V4.3 | Testé sur matériel réel, cycle de bascule BLE/WiFi validé (voir `firmware/heltec_v4/`) |
| Heltec Wireless Tracker V2 | Testé sur matériel réel, cycle de bascule BLE/WiFi validé (voir `firmware/heltec_tracker_v2/`) |

## Utilisation rapide

1. **Flasher** : `firmware/<board>/firmware-merged.bin` (board neuve, à l'offset `0x0`,
   efface tout) ou `firmware.bin` (mise à jour d'un board déjà sous MeshCore, via
   [meshcore.io/flasher](https://meshcore.io/flasher) ou en série).
2. **Configurer le WiFi** : ouvrir la [page de configuration](https://shleepong.github.io/meshcore-companion-ble-wifi-usb/)
   hébergée par ce dépôt (disponible en français et en anglais), en Bluetooth ou en
   USB, aucune installation requise (Chrome ou Edge, Windows ou Android ; Safari/iOS
   non supporté, restriction du navigateur, pas de ce firmware). **Code d'appairage
   Bluetooth : celui affiché sur la page d'accueil de l'écran** (`Pin:XXXXXX`),
   différent à chaque démarrage, pas une valeur fixe. Ce code ne sert qu'au tout
   premier appairage : une fois l'appareil apparié, Windows/Android le reconnaît
   automatiquement aux connexions suivantes (y compris après une mise à jour du
   firmware) sans redemander de code, tant que l'appairage n'a pas été supprimé
   manuellement côté PC/téléphone. Alternative en ligne de commande :
   [`meshcore-cli`](https://github.com/meshcore-dev/meshcore-cli), voir
   `set wifi_ssid` / `set wifi_pwd` / `set wifi_enabled`.
3. **Enregistrer** sur la page web : sauvegarde seulement, sans redémarrer. Un
   message indique ensuite si un redémarrage est réellement nécessaire pour que le
   changement prenne effet, avec un bouton dédié pour le faire depuis la page
   elle-même.
4. **Basculer au quotidien** : page "WIFI" ou "BLUETOOTH" sur l'écran de l'appareil,
   appui long sur le bouton. La carte redémarre dans l'autre mode.

## Construire soi-même

Les fichiers `.patch` dans `patches/` s'appliquent dans l'ordre sur le tag officiel
`companion-v1.17.1` du dépôt [meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore).
Voir `patches/README.md` pour le détail de chaque patch et la procédure.

## Problème connu

Après une session USB ouverte puis fermée **depuis le navigateur** (bouton
"Déconnecter" de la page web), l'écran peut rester bloqué sur "Connected" au lieu
d'afficher le code d'appairage Bluetooth, tant que le câble USB n'est pas
physiquement débranché. Limite du cœur Arduino-ESP32 lui-même (la détection de
session USB ne se réinitialise que sur un vrai débranchement, pas sur une simple
fermeture logicielle du port), pas quelque chose de corrigible depuis ce firmware.
N'affecte que l'affichage, pas le fonctionnement réel.

## Licence

MIT, héritée de [meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore)
(voir `LICENSE`). Ce dépôt ne fait qu'ajouter des patchs et des binaires précompilés
par-dessus le code officiel.
