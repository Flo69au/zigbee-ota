# Index OTA Zigbee commun

Ce dépôt regroupe les index OTA de tous mes projets Zigbee en un seul fichier,
[`ota_index.json`](ota_index.json), lu par Zigbee2MQTT (qui n'accepte qu'un seul index personnalisé).

| Projet | Fabricant | Type d'image |
|---|---|---|
| [capteur-temp-rature-scd40_zigbee](https://github.com/Flo69au/capteur-temp-rature-scd40_zigbee) | `0x1337` | `1` |
| [sonde-zigbee-radiateur](https://github.com/Flo69au/sonde-zigbee-radiateur) | `0x1337` | `2` |
| [capteur-co2-scd40-esp32h2](https://github.com/Flo69au/capteur-co2-scd40-esp32h2) | `0x1337` | `3` |
| [levoit-classic-300s-zigbee](https://github.com/Flo69au/levoit-classic-300s-zigbee) | `0x131B` | `0` |

Chaque projet garde ses releases et son propre `ota_index.json`. Le workflow
`.github/workflows/merge-index.yml` les fusionne **toutes les heures**, à chaque modification de
`sources.txt`, ou à la demande (*Actions → Fusion des index OTA → Run workflow*).

## Zigbee2MQTT

```yaml
ota:
  zigbee_ota_override_index_location: https://raw.githubusercontent.com/Flo69au/zigbee-ota/main/ota_index.json
  disable_automatic_update_check: false
  update_check_interval: 1440
  default_maximum_data_size: 50   # ne pas mettre 100 : l'OTA échoue dès le début
```

## Ajouter un projet

1. Le projet publie un `ota_index.json` (liste JSON) dont chaque entrée contient au minimum
   `url`, `fileVersion`, `imageType`, `manufacturerCode` (idéalement aussi `fileSize`, `sha512`, `modelId`).
2. Lui donner un couple **fabricant / type d'image unique** (voir tableau) : le workflow refuse
   de publier un index où deux projets partagent le même couple.
3. Ajouter l'URL brute de son index dans `sources.txt`.

Ne jamais mettre `"force": true` dans une entrée : Z2M proposerait la mise à jour en boucle.
