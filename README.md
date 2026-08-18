# Outils Locaux

Suite d'outils image et PDF **100 % locaux** — vos fichiers ne quittent jamais votre appareil.

Promesse : **« Vos fichiers ne quittent jamais votre appareil. »** Tous les traitements (fusion PDF, image → PDF, réduction d'image, extraction de texte) s'exécutent dans le navigateur du visiteur. Aucun fichier n'est envoyé sur un serveur.

## Fichiers du site

| Fichier | Rôle |
|---|---|
| `index.html` | Page de vente : hero, 3 douleurs, 3 étapes, 3 offres, formulaire EmailJS, section « 100 % local », FAQ, footer, JSON-LD, OG, reveal |
| `outil.html` | Outils réels 100 % local : fusion PDF, image → PDF, réduction d'image, extraction de texte + compteur localStorage (5 gratuites/mois) + modale upgrade |
| `chatbot.js` | Widget chatbot autonome (pattern ai-course-builder), accent `#8b5cf6` |
| `chatbot-config.js` | Config chatbot spécifique : nom, welcome, 8 FAQs métier, EmailJS |

## Design

- Violet orchidée `#8b5cf6` + noir `#0a0a12` + orange pêche `#ffb08a` — identité unique.
- Typographies : Space Grotesk (titres) + Inter (corps), Google Fonts.
- Reveal au scroll (IntersectionObserver), mockup fenêtre d'outil dans le hero, modale upgrade animée.
- `prefers-reduced-motion` respecté.

## Offres

- **Gratuit** — 0 € : 5 conversions/mois, 4 outils, fichiers jusqu'à 50 Mo.
- **Pro** — 9 €/mois (★ le plus choisi) : conversions illimitées, lot d'images (jusqu'à 50), fichiers jusqu'à 500 Mo, réduction par lots, support prioritaire.
- **Business** — 19 €/mois : tout Pro + jusqu'à 5 utilisateurs, gestion admin, facture mensuelle.

Paiement par virement ou message privé (Stripe en attente), déblocage sous 24 h ouvrées.

## EmailJS (réel)

- Service : `service_cy1ytdb` — Template : `template_xpo58cv` — Clé publique : `8Pui4ZEqxW2jRVF7h`
- Payload : `{ site: 'Outils Locaux', name, email, question }` — `question` contient l'offre concernée + le message.
- Le SDK est chargé à la demande au moment de la soumission (pattern `chargerEmailJS`), ce qui évite toute course asynchrone.
- Même configuration dans le chatbot (fallback FAQ → capture de lead).

## Compteur de conversions (outil.html)

- Clé localStorage : `ol_usage` = `{ mois: 'AAAA-MM', n: nombre }`.
- 5 conversions gratuites par mois, réinitialisées automatiquement le 1er du mois.
- Chaque opération réussie compte pour 1 conversion. À la 5ᵉ, la modale upgrade s'affiche.
- Lien « Passer au Pro » : `ol_offre_souhaitee` → pré-sélection de l'offre dans le formulaire de `index.html#commander`.

## Bibliothèques (CDN, chargées à la demande au premier usage)

- **pdf-lib** 1.17.1 (cdnjs) — fusion PDF et image → PDF.
- **pdf.js** 3.11.174 (cdnjs) — extraction de texte.

Aucun fichier utilisateur ne transite par ces CDN : seuls les codes des bibliothèques sont téléchargés, puis tout fonctionne hors-ligne.

## Limites honnêtes

1. **Extraction de texte** : ne fonctionne que sur les PDF « numériques » (texte sélectionnable). Un PDF scanné (image) renvoie une erreur claire — pas de reconnaissance optique (OCR) intégrée. C'est volontaire : un OCR embarqué nécessiterait un modèle de plusieurs dizaines de Mo, et une version cloud contredirait la promesse 100 % local.
2. **PDF protégés** : un PDF chiffré ou protégé ne peut pas être fusionné (pdf-lib ignore l'encryption à la lecture, mais la copie échoue) — message d'erreur explicite. Le plan Business n'ajoute aucune fonctionnalité de « déverrouillage » (éthique et légalité obligent).
3. **Fusion PDF** : l'ordre des pages = ordre de sélection des fichiers (pas de réordonnancement par glisser-déposer dans cette version). Les signets, annotations et formulaires ne sont pas préservés à la fusion.
4. **Réduction d'image** : PNG avec transparence sont recompressés en PNG (le poids baisse peu sur des captures d'écran à plat) ; les autres formats repartent en JPEG. Le nombre de couleurs n'est pas optimisé.
5. **Image → PDF** : la conversion utilise le canvas — les images EXIF (rotation) sont lues dans l'orientation native du fichier. Le texte des PDF générés n'est pas sélectionnable (ce sont des images), c'est le fonctionnement normal de ce type de conversion.
6. **Taille mémoire** : les gros fichiers (500 Mo en Pro) sont traités en mémoire vive. Sur un appareil modeste, un fichier proche de la limite peut ralentir le navigateur. C'est le prix de la confidentialité totale.
7. **Compteur mensuel** : stocké en `localStorage` par appareil et par navigateur. Vider les données du navigateur remet le compteur à zéro — c'est un garde-fou d'honnêteté, pas une faille : nous ne stockons rien côté serveur, donc nous ne pouvons pas « suivre » les conversions par ailleurs.
8. **Aucun OCR, aucune édition de PDF** (pas de signature, pas de remplissage de formulaire, pas de réordonnancement de pages dans cette version).
9. **Paiement** : virement ou message privé en attendant l'intégration Stripe. Le déblocage Pro/Business est manuel (24 h ouvrées).

## Vérifications effectuées

- Formulaire EmailJS branché sur le vrai service (vérifié par lecture de code — aucun envoi de test déclenché pendant la vérification).
- Chatbot : config chargée avant `chatbot.js`, accent `#8b5cf6`, FAQs spécifiques au business, EmailJS branché.
- Orthographe française vérifiée (script `verify_ortho_fr.py`).
- Rendu vérifié dans un navigateur headless (Chromium local) : pages chargées sans erreur JS, layout sans chevauchement, breakpoints mobile 390 px.

## Déploiement

Non publié sur GitHub (consigne). Pour mettre en ligne : héberger le dossier tel quel (les chemins sont relatifs) — par exemple GitHub Pages, Netlify ou un tunnel cloudflared. Aucune configuration serveur requise : le site est 100 % statique.
