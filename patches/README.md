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

Chaque message de commit détaille la cause du bug et comment il a été confirmé
(souvent sur matériel réel). Voir directement le contenu des patchs pour le détail
technique complet.
