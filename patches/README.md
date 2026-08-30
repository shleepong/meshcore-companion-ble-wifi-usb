# Patchs

Base : tag `companion-v1.17.1` du dépôt officiel
[meshcore-dev/MeshCore](https://github.com/meshcore-dev/MeshCore).

## Application

```bash
git clone https://github.com/meshcore-dev/MeshCore.git
cd MeshCore
git checkout companion-v1.17.1
git checkout -b companion-ble-wifi-usb
git am /chemin/vers/patches/*.patch
```

(`git am` applique les patchs dans l'ordre en conservant les messages de commit
d'origine. En cas de conflit avec une version plus récente de MeshCore, `git apply`
patch par patch, en résolvant à la main, fonctionne aussi.)

## Contenu, dans l'ordre d'application

| Fichier | Description |
|---|---|
| `0001-...cmd-line-limit.patch` | Corrige la limite de longueur de ligne de commande `cmd.exe` sur Windows lors de la compilation ESP32-S3 (SCons). |
| `0002-...dual-BLE-WiFi-transport...patch` | Ajoute le flag de build `DUAL_BLE_WIFI` : BLE et WiFi actifs simultanément (via `MultiSerialInterface`), réglages `wifi_ssid`/`wifi_pwd`/`wifi_enabled` persistés et configurables via `CMD_SET_CUSTOM_VAR`. |
| `0003-...WIFI-home-screen-page...patch` | Nouvelle page "WIFI" sur l'écran de l'appareil : bascule le WiFi on/off à chaud, sans redémarrage. |
| `0004-...boot-crash-and-WiFi-page...patch` | Corrige une boucle de redémarrage au boot (ordre d'initialisation `WiFi.mode()`) et l'affichage incorrect de l'état sur la page WIFI. |
| `0005-...enable-USB-companion-protocol...patch` | Active le protocole companion sur le port USB série (115200 bauds), en plus du BLE et du WiFi. |
| `0006-...CMD_GET_CUSTOM_VARS-leading-comma...patch` | Corrige une virgule de séparation mal placée dans la réponse à `CMD_GET_CUSTOM_VARS` qui faisait planter certains clients (dont `meshcore-cli`). |
| `0007-...TCP-companion-server...patch` | Corrige le serveur TCP companion qui restait mort après un cycle WiFi off/on depuis la page écran (sans redémarrage). |
| `0008-...dark-theme-for-ST7735-boards...patch` | Thème sombre (fond noir) pour les boards à écran couleur ST7735 (Wireless Tracker V2/V1, T096). |
| `0009-...ArduinoSerialInterface.isConnected...patch` | Corrige `isConnected()` qui renvoyait toujours vrai dès que l'USB companion était actif, masquant le PIN Bluetooth derrière `< Connected >` en permanence sur ESP32-S3/C3. |
| `0010-...EXCLUSIVE_BLE_WIFI-mode...patch` | Ajoute le mode `EXCLUSIVE_BLE_WIFI` : BLE et WiFi ne sont jamais actifs en même temps, un seul choisi au démarrage selon `wifi_enabled`, bascule par redémarrage complet (V4.3). |
| `0011-...shorten-reboot-switch-label...patch` | Réduit l'étiquette de bascule pour qu'elle tienne sur la largeur de l'écran OLED (SSD1306). |
| `0012-...fit-full-switch-label-on-WIFI-BLUETOOTH-pages...patch` | Réorganise les pages WIFI et BLUETOOTH du mode exclusif (texte au lieu d'icône, message sur deux lignes) pour que l'étiquette de bascule tienne entièrement à l'écran. |
| `0013-...add-EXCLUSIVE_BLE_WIFI-env-for-Tracker...patch` | Ajoute l'environnement de build `EXCLUSIVE_BLE_WIFI` pour le Wireless Tracker V2. |
| `0014-...shrink-Switching-to-X-rebooting-reboot-messag...patch` | Réduit la taille du message plein écran "Switching to X.../rebooting..." qui débordait de l'écran. |
| `0015-...battery-percentage-instead-of-icon...patch` | Remplace l'icône de batterie par un pourcentage en texte, ajoute une page BATTERY dédiée (tension, plage de référence). |
| `0016-...defer-WiFi.mode-to-the-WiFi-branch-only...patch` | Corrige un blocage total au démarrage sur le Wireless Tracker V2 : `WiFi.mode()` était appelé avant l'initialisation Bluetooth même en repli BLE, ce qui bloquait cette dernière sur cette board (jamais affecté le V4.3). |
| `0017-...add-accent_cyan-color...patch` | Ajoute une couleur cyan dédiée pour le texte d'état "on" des pages BLUETOOTH/WIFI sur les écrans couleur (l'ancienne couleur, un bleu marine très foncé, était peu lisible sur le Wireless Tracker V2). |

Chaque message de commit détaille la cause du bug et comment il a été confirmé
(souvent sur matériel réel). Voir directement le contenu des patchs pour le détail
technique complet.

Les patchs `0001` à `0007` ajoutent le mode `DUAL_BLE_WIFI` (BLE et WiFi actifs en
même temps), qui reste présent dans le code mais n'est plus proposé ni maintenu
(voir la note historique du README principal). Les patchs `0008` à `0017` ajoutent
le mode `EXCLUSIVE_BLE_WIFI`, seul mode désormais distribué par ce dépôt.

Vérifié le 30/08/2026 : les 17 patchs s'appliquent sans conflit sur une copie
fraîche du tag `companion-v1.17.1` (`git am`), le résultat reproduit exactement
(fichier pour fichier) l'état actuel du dépôt source pour les fichiers
concernés, et compile avec succès pour les deux boards
(`heltec_v4_companion_radio_ble_wifi_exclusive` et
`heltec_tracker_v2_companion_radio_ble_wifi_exclusive`), avec la même
utilisation RAM/Flash que les binaires précompilés fournis dans ce dépôt.
