<div align="center">

<img src="assets/banner.png" alt="Muesli pour Obsidian : le bloc-notes IA pour les réunions qui s'enchaînent, sans bot de réunion">

### Granola, dans Obsidian. Enregistre n'importe quel appel, récupère notes, actions et transcript dans ton coffre.

Open source. Aucun bot dans ton appel. Tes notes restent dans ton coffre.

[![Plugin de la communauté Obsidian](https://img.shields.io/badge/Obsidian-community%20plugin-7c3aed)](https://community.obsidian.md/plugins/muesli)
[![License: MIT](https://img.shields.io/badge/license-MIT-333)](LICENSE)
![macOS | Windows | Linux | iOS | Android](https://img.shields.io/badge/macOS%20%7C%20Windows%20%7C%20Linux%20%7C%20mobile-333)

<table>
  <tr>
    <td align="center" width="200"><h2>0</h2>bot dans ton appel</td>
    <td align="center" width="200"><h2>Toutes</h2>les visios</td>
    <td align="center" width="200"><h2>Secondes</h2>entre stop et tes notes</td>
    <td align="center" width="200"><h2>100 %</h2>Markdown, à toi</td>
  </tr>
</table>

[English](README.md) · **Français**

</div>

## Comment ça marche

1. Clique 🎙️ → **Réunion**.
2. Note quelques mots dans `Notes.md` pendant que les gens parlent.
3. Clique stop. Quelques secondes plus tard :

```
Réunions/2026-10-09 1430 Revue tarifs Acme/
  Résumé.md       ← TL;DR, décisions, actions, questions ouvertes
  Transcript.md   ← qui a dit quoi, horodaté
  Notes.md        ← tes notes brutes, intactes
  audio.mp3       ← optionnel
```

Les actions sortent en `- [ ] Paul — envoyer le récap (vendredi)`, prêtes pour Tasks, Dataview et Bases.

## Installation

Dans Obsidian : **Réglages → Plugins communautaires → Parcourir → « Muesli » → Installer → Activer**.
Ou [ouvre-le dans l'annuaire](https://community.obsidian.md/plugins/muesli) et clique *Add to Obsidian*.

Un écran de configuration demande ensuite une [clé OpenRouter](https://openrouter.ai/keys), ton nom, et lance un test du son. C'est tout.

## Pourquoi Muesli

|                                     | Granola | Otter / Fireflies | **Muesli** |
|-------------------------------------|:-------:|:-----------------:|:----------:|
| Aucun bot dans l'appel              |    ✓    |         –         |   **✓**    |
| Notes dans ton coffre Obsidian      |    –    |         –         |   **✓**    |
| Open source                         |    –    |         –         |   **✓**    |
| Zoom, Meet, Teams, n'importe quelle app | ✓   |         ~         |   **✓**    |

## Et aussi

- **Journal vocal** : dis tes tâches, plans et humeur, récupère des puces dans la note du jour.
- **Importe n'importe quel enregistrement** : dépose un `.m4a` ou `.mp3` dans `Réunions/À importer`, tu obtiens le même dossier.
- **Mémos Dictaphone de l'iPhone** : les nouveaux mémos sont transcrits et rangés sur ton Mac, automatiquement.
- **Sur le téléphone** : réunions en présentiel et journal, un gros bouton.
- **Ton modèle, ta langue** : les résumés suivent tes propres modèles (Général, 1:1, appel commercial…), en français ou en anglais.
- **Jamais d'oubli du stop** : une petite pastille au-dessus de toutes les apps, invisible en partage d'écran, termine seule les réunions oubliées.

## Confidentialité et transparence

- **Réseau** : l'audio et le texte partent **uniquement vers [OpenRouter](https://openrouter.ai)** (et les fournisseurs de modèles vers lesquels il route), avec ta propre clé. Aucune télémétrie, aucun serveur à nous.
- **Service payant** : OpenRouter est une API payante ; il faut un compte avec du crédit.
- **Helper natif (macOS)** : le son du système est capté par `meeting-tap`, un petit programme Swift open source ([source](native/macos/main.swift)) embarqué dans `main.js` avec son SHA-256 et écrit dans le dossier du plugin à la première utilisation. Rien n'est téléchargé. Les deux sont compilés par GitHub Actions avec des [attestations de provenance](https://github.com/Azilone/obsidian-muesli/attestations).
- **Fichiers hors du coffre** : seulement quand tu importes un fichier du disque, ou avec la boîte de réception Dictaphone optionnelle (accès complet au disque requis).
- **Préviens toujours les participants que tu enregistres**, et respecte les lois sur le consentement là où tu vis.

<details>
<summary><b>Prérequis</b></summary>

| | Appels (micro + son du système) | Présentiel, journal, import |
|---|---|---|
| macOS 14.4+ | ✅ | ✅ |
| Windows, Linux | — (micro seul pour l'instant) | ✅ |
| iOS, Android | — | ✅ |

Obsidian 1.13 ou plus récent, et une [clé API OpenRouter](https://openrouter.ai/keys).
Sur iOS, garde Obsidian au premier plan pendant l'enregistrement. La clé est stockée par appareil.

</details>

<details>
<summary><b>Autres façons d'installer</b></summary>

- **BRAT** (dernières versions) : installe [BRAT](https://github.com/TfTHacker/obsidian42-brat), puis *BRAT: Add a beta plugin* → `Azilone/obsidian-muesli`.
- **À la main** : télécharge `main.js`, `manifest.json` et `styles.css` depuis la [dernière release](https://github.com/Azilone/obsidian-muesli/releases/latest) dans `<ton coffre>/.obsidian/plugins/muesli/`, puis active **Muesli**.
- **Depuis les sources** : voir [CONTRIBUTING.md](CONTRIBUTING.md#development).

</details>

<details>
<summary><b>Sous le capot</b></summary>

| Brique | Rôle |
|---|---|
| **Micro** | Web Audio. En appel, c'est *toi* : tu es toujours bien attribué. |
| **Son du système** | `meeting-tap` (Swift, ~320 Ko) capte la sortie audio du Mac via les process taps de Core Audio. Ni bot, ni pilote audio virtuel. |
| **Suppression des silences** | Un détecteur de voix local retire les silences avant l'envoi, et coupe les segments dans les silences pour ne jamais couper un mot. |
| **Transcription** | [`microsoft/mai-transcribe-2`](https://openrouter.ai/microsoft/mai-transcribe-2) via OpenRouter, avec diarisation, en arrière-plan *pendant* la réunion. |
| **Nettoyage et résumé** | `anthropic/claude-haiku-5.5` par défaut (tout modèle OpenRouter marche) : relie les voix, corrige les erreurs, écrit le résumé autour de tes notes, choisit un titre. |

Tout est d'abord écrit dans ton coffre (`<réunion>/.raw/`) : un plantage ou un appel API raté ne fait jamais perdre une réunion. **Retraiter la réunion** reprend là où ça s'est arrêté.
Tu synchronises ton coffre avec git ? Pense à ajouter `**/.raw/` et `*.mp3` au `.gitignore`.

</details>

<details>
<summary><b>Réglages</b></summary>

| Réglage | Par défaut |
|---|---|
| Type de réunion par défaut | Appel (micro + son du système) / Présentiel (micro seul) |
| Modèle de résumé par défaut | `Général` |
| Langue / langue du résumé | Détection auto / français si Obsidian est en français |
| Garder l'audio | Oui |
| Pastille flottante | Oui, invisible en partage d'écran |
| Demander après N min sans parole / arrêter après | 10 min / 5 min |
| Durée maximale | 180 min |
| Modèles | `microsoft/mai-transcribe-2`, `anthropic/claude-haiku-5.5` |
| Dossiers | `Réunions`, `Réunions/Modèles`, `Réunions/À importer`, `Notes vocales` |

</details>

<details>
<summary><b>Développement</b></summary>

```bash
bun install
bun run check                           # types, règles de lint Obsidian, tests
./native/build.sh                       # recompile native/meeting-tap (universel, signé ad hoc)
VAULT="/chemin/du/coffre" bun run dev   # mode watch, build directement dans le coffre
```

Le helper écrit du PCM brut (s16le, mono, 16 kHz) sur stdout et des lignes de statut JSON sur stderr. Tout helper qui respecte ce contrat peut être ajouté pour Windows (WASAPI loopback) ou Linux (PipeWire monitor) : contributions bienvenues. Voir [CONTRIBUTING.md](CONTRIBUTING.md).

</details>

## Feuille de route

- Son du système sur Windows et Linux
- Transcript en direct pendant l'enregistrement
- « Demande à tes réunions » : un chat sur tout le coffre
- Titre et participants depuis le calendrier

<div align="center">

**Si Muesli t'a évité un compte rendu, une ⭐ aide d'autres à le trouver.**

[MIT](LICENSE)

</div>
