# StudyApp

Application mono-fichier d'apprentissage de vocabulaire par **rappel actif** (Leitner + SM-2 allégé).
Aucune installation, aucun serveur, aucune dépendance réseau.

## Ouvrir

Double-cliquez **`studyapp.html`**. C'est tout — l'app s'ouvre en `file://` avec 3 decks
d'exemple déjà chargés (Business English, Tech & AI, IELTS Speaking, 5 cartes chacun pour l'instant).

> `fetch()` étant bloqué en `file://`, les decks ne sont **jamais** lus par chemin :
> ceux d'origine sont embarqués dans le fichier, les vôtres passent par l'import ci-dessous.

## Ajouter un deck

Sur l'écran d'accueil, section **« Ajouter un deck »** — trois voies équivalentes :

- **glisser-déposer** un fichier `.md` sur la zone pointillée ;
- le **sélectionner** via le bouton de fichier ;
- **coller** son texte dans la zone prévue puis « Importer le texte collé ».

Le deck importé est enregistré localement et rejoint la liste. Un rapport d'import s'affiche
(`N cartes importées, M lignes ignorées` + détail des lignes ignorées).

### Format `.md`

```
# Nom de la section
- word : definition of the word | Sentence using the ___ in context.
- other word : another definition
```

- `# Titre` ouvre une **catégorie** ; chaque carte hérite de la dernière rencontrée.
- `- ` ouvre une carte. Séparateur terme / définition : ` : ` (**première occurrence seulement** —
  une définition peut contenir des `:`).
- ` | ` (optionnel) introduit une **phrase de mise en application**, où `___` marque l'emplacement du terme.
- Lignes vides, commentaires (`//`) et texte hors format : ignorés, mais **comptés** dans le rapport.
- Une ligne malformée ne bloque jamais l'import.

## Étudier

Choisissez un deck → réglez **mode**, **sens** (terme→définition ou l'inverse), **catégorie**,
**taille de session** → *Commencer*.

| Mode | Ce que vous faites |
|---|---|
| **Cartes** | Recto/verso, puis auto-évaluation : Encore / Difficile / Acquis. |
| **Apprentissage** | QCM pour les cartes neuves, saisie libre ensuite. Le cœur de l'app. |
| **Écriture** | La définition est affichée, vous tapez le terme. |
| **Mise en application** | Phrase à trou, vous tapez le terme manquant. (Grisé si le deck n'a pas de phrases.) |
| **Association** | Grille terme ↔ définition à apparier, chronométrée. |

Une carte n'est **« acquise »** qu'après **2 saisies libres correctes consécutives**
(le QCM ne compte jamais). Une faute la renvoie en boîte 0 et la réinjecte plus loin dans la session.
Fautes de frappe tolérées (« Presque ! » — pas compté comme échec) ; bouton **« Je l'avais »**
pour corriger un faux négatif.

### Raccourcis clavier

`Espace` retourner · `1` / `2` / `3` auto-évaluer · `Entrée` valider / continuer · `?` aide · `Échap` fermer.
Tout le parcours est faisable sans souris.

## Sauvegarder sa progression

Tout est persisté dans `localStorage` **à chaque réponse** (pas seulement en fin de session) ;
une session interrompue est proposée à la reprise au relancement.

Sur l'écran d'un deck, section **« Données & progression »** :

- **Exporter la progression (JSON)** — télécharge un fichier de sauvegarde.
- **Importer la progression** — réinjecte un tel fichier (fusion par carte).
- **Exporter le .md** — récupère la source du deck pour l'éditer puis la réimporter.
- **Réinitialiser ce deck** — efface la progression (confirmation requise).

L'identifiant d'un deck est un hash de l'ensemble de ses termes : renommer le deck ou une
section ne fait **pas** perdre la progression.

## Statistiques

Écran dédié (bouton *Statistiques* sur l'écran d'un deck) : taux de réussite global et par
catégorie, rétention sur 30 jours, top 10 des cartes les plus ratées (avec « réviser
uniquement celles-ci »), série de jours consécutifs, cartes dues aujourd'hui.

## Fichiers

| Fichier | Rôle |
|---|---|
| `studyapp.html` | l'application (HTML + CSS + JS, un seul fichier) |
| `ARCHITECTURE.md` | modèle de données, état, découpage des modules |
| `RESEARCH.md` | synthèse Quizlet + psychologie de l'apprentissage et décisions d'implémentation |
| `tests.mjs` | vérifications des fonctions pures (Node) |
| `TESTS.md` | plan de test et protocole des vérifications manuelles |

Auto-tests dans le navigateur : ouvrir `studyapp.html#selftest`.

## Encore à faire

Commencez par générer vos **10 premières cartes** en suivant correctement la forme ci-dessus dans le format `.md`
. Ils remplaceront les sources d'exemple embarquées sans modification du code. 
Enjoy la plateforme ! 
