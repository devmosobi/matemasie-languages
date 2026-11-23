# Langues communautaires de Matemasie

Ce dépôt public héberge le catalogue des langues de l'application Matemasie (MOSOBI) proposées par la communauté. Le français et l'anglais sont intégrés à l'application et ne figurent pas ici.

L'application ne lit ce catalogue que si l'utilisateur active « Catalogue communautaire » dans Paramètres, Apparence et langue. Elle télécharge alors `catalog.json`, puis la langue choisie et sa signature, par de simples requêtes HTTPS, sans envoyer aucune donnée.

## Contenu

| Fichier | Rôle |
|---|---|
| `catalog.json` | Liste des langues proposées. |
| `<code>.json` | Fichier de langue (format décrit dans le guide de traduction de Matemasie). |
| `<code>.json.sig` | Signature Ed25519 de MOSOBI des octets exacts du fichier de langue, en Base64. |

Format de `catalog.json` (adresses relatives à celle du catalogue) :

```json
{"languages":[{"locale":"de","name":"Deutsch","version":"1.0.0","file":"de.json","signature":"de.json.sig"}]}
```

Limites appliquées par l'application : catalogue de 256 Ko, fichier de langue de 2 Mo, signature de 1 Ko.

## Français et anglais : `reference/`

`reference/en.json` (langue de référence) et `reference/fr.json` sont des copies des langues intégrées à l'application, mises à jour à chaque version de test. Elles ne figurent pas dans `catalog.json` : l'application refuse d'installer depuis le catalogue une langue qu'elle intègre déjà. Elles servent :

- de point de départ pour une nouvelle traduction (partir de `reference/en.json`) ;
- à proposer des corrections du français ou de l'anglais par demande de fusion sur ces fichiers ; MOSOBI les reporte ensuite dans l'application.

Version actuelle des copies : Matemasie 0.12.5.

## Vérifiée ou non vérifiée

Une langue dont la signature correspond à la clé publique de MOSOBI intégrée à l'application s'affiche « vérifiée ». Une langue sans signature, ou modifiée après sa signature, s'installe « non vérifiée ». Les fichiers sont toujours contrôlés avant usage : textes affichés comme du texte, sans balisage, variables identiques à l'anglais.

## Proposer une traduction

1. Traduire à partir de la référence anglaise de l'application, en suivant le guide de traduction (syntaxe ICU MessageFormat, clés inchangées).
2. Ouvrir une demande de fusion ajoutant `<code>.json`, sans signature.
3. Après relecture, MOSOBI signe le fichier, ajoute `<code>.json.sig` et l'entrée correspondante dans `catalog.json`.

## Publier une langue (MOSOBI)

La clé privée de signature reste hors de toute machine de développement et de tout dépôt.

```powershell
dotnet run --project tools/Localization -- sign --file de.json --key-file <chemin de la clé privée>
```

La commande signale si la clé ne correspond pas à la clé publique intégrée à l'application. Ne jamais modifier un fichier après sa signature : `.gitattributes` empêche toute conversion de fin de ligne.

---

# Matemasie community languages

Public catalog of community translations for the Matemasie app by MOSOBI. The app reads it only when the user turns on "Community catalog" in Settings, and sends no data. Files signed by MOSOBI (Ed25519) are shown as "verified"; unsigned or modified files install as "unverified". To propose a translation, start from `reference/en.json` and open a pull request adding `<code>.json`; MOSOBI signs it and adds it to `catalog.json` after review. Corrections to English or French go to `reference/`, and MOSOBI carries them over into the app.
