# ARCHITECTURE — StudyApp

Application mono-fichier (`studyapp.html`), HTML + CSS + JS vanilla ES6+, ouvrable en `file://`,
sans serveur, sans CDN, sans build. Cible < 2500 lignes.

## Contrainte `file://`
`fetch()` est bloqué par CORS en `file://`. Les decks ne sont donc **jamais** chargés par chemin.
Deux voies, toutes deux implémentées :
1. **Decks embarqués** — sérialisés en template literals JS dans la section `DECKS`, parsés au boot.
   L'écran d'accueil n'est jamais vide. *(À ce stade : 3 decks de démonstration à 5 cartes chacun ;
   les 150 cartes définitives viendront remplacer ces sources, sans changement de code.)*
2. **Import utilisateur** — drag & drop + `<input type="file">` + zone « coller le texte »,
   lecture via `FileReader` (fonctionne en `file://`). Le deck importé est persisté et rejoint la liste.

## Découpage (sections commentées dans le `<script>`)
| Section | Rôle | Pur / testable Node |
|---|---|---|
| `PARSER` | `parseDeck(text) -> {cards, categories, warnings}`, `normalizeText`, `hashStr`, `cardId`, `hashDeckId` | oui |
| `STORAGE` | `localStorage` (1 clé/deck), `deckId` = hash du set de termes, `schemaVersion` + migration, export/import JSON, reset | partiel |
| `SCHEDULER` | `nextState(prev, outcome, now)`, `buildQueue(cards, options, now)`, `insertRequeue`, `cardStatus`, `interleave` | oui |
| `SESSION` | machine à états par mode, `compareAnswer`/`levenshtein`, `buildMCQ`, réinjection différée | partiel (correction = pur) |
| `STATS` | `computeStats` : réussite globale/par catégorie, rétention 30 j, top 10 ratées, série de jours, dus du jour | oui |
| `DECKS` | sources `.mkd` embarquées | — |
| `UI` | objet `state` unique + `render(state)` ; aucune écriture DOM hors de cette section ; raccourcis clavier ; thème | inspection manuelle |

## Modèle de données
- **Carte (contenu)** : `{ id, term, definition, sentence|null, category, lineNo }`.
  `id = hashStr(normalizeText(term))`. `sentence` contient `___` à l'emplacement du terme.
- **Progression (par carte, persistée)** : `{ box:0..5, ease:1.7..2.8, dueDate|null, streak, lapses, seen, correct, freeStreak, lastResult }`.
  Statut dérivé (jamais stocké) : `new` · `learning` · `mastered` (`freeStreak >= 2`) · `due` (`dueDate <= now`).
- **Bundle par deck** : `{ schemaVersion, deckId, name, source, progress:{}, sessionInProgress|null, stats:{history,daily,lastSessionAt} }`
  sous `localStorage["studyapp:deck:<deckId>"]`. Index : `localStorage["studyapp:index"]`. Réglages : `localStorage["studyapp:settings"]`.
- `deckId` = `hashStr(termes normalisés triés)` : **exclut** le nom de fichier et les `# Section`.
  Renommer le deck ou une section ne perd pas la progression ; au ré-import, la progression est fusionnée par `id` de carte commun.

## État runtime & rendu
Un objet `state` unique `{ route, decks[], settings, bundle, session, ui }`.
Une fonction `render(state)` reconstruit `#app` pour la route courante (remplacement de sous-arbres, pas de framework).
Les événements appellent des réducteurs qui mutent `state` puis rappellent `render()` + `saveBundle()`.
Le champ de saisie n'est **pas** re-rendu à chaque frappe (lecture DOM au submit) pour préserver le curseur.

## Scheduler (Leitner + SM-2 allégé)
- `BOX_INTERVALS = [0,1,2,4,8,16]` jours (rappel expansif ; le « 0 » = réinjection intra-session, pas un intervalle espacé).
- Réussite → `box+1` (max 5), `ease += 0.05` (ou `-0.15` si « Difficile »), `dueDate = now + BOX_INTERVALS[box] * ease/2.3`.
- Échec (`again`) → `box = 0`, `lapses++`, `freeStreak = 0`, `ease -= 0.2`, réinjection dans la session **après 3 à 5 cartes** (`insertRequeue`, jamais immédiate).
- « Presque » (Levenshtein ≤ 1, ≤ 2 si terme > 8 car.) → ni échec ni réussite ; carte rejouée en fin de session.
- `freeStreak` n'est incrémenté **qu'en saisie libre** (Écriture, Mise en application, phase libre d'Apprentissage) ; le QCM ne fait jamais passer une carte.
- `buildQueue` : `en retard > en cours > neuves`, plafonné à `size` (défaut 20), puis interleaving des catégories (round-robin) sauf option « bloquer par catégorie ».

## Modes (même dataset, zéro duplication)
Cartes (auto-éval 3 boutons) · Apprentissage (QCM si `box<=1`, saisie libre si `box>=2`) · Écriture (déf → terme) ·
Mise en application (phrase à trou ; désactivé proprement si le deck n'a aucune `sentence`) · Association (grille chronométrée, pilotable au clavier).
Sens de restitution `terme→définition` / `définition→terme` réglable par session.

## Persistance & reprise
Sauvegarde après **chaque** réponse (progress + `sessionInProgress` + stats). À la réouverture, l'accueil propose « Reprendre » si une session est en cours.
Export/Import de la progression en JSON, « Réinitialiser ce deck » avec confirmation. `schemaVersion` versionné, migration silencieuse.

## Accessibilité / UI
Feedback immédiat (correct/incorrect + réponse attendue + phrase d'exemple) après chaque réponse.
Progression `x / N` visible en continu, sans compte à rebours. Raccourcis : `Espace` retourner, `1/2/3` auto-évaluer, `Entrée` valider, `←/→` naviguer, `?` aide, `Échap` fermer.
Navigation clavier complète (éléments natifs focusables). `prefers-color-scheme` respecté + bascule manuelle persistée. Design sobre, carte centrée, pas de dégradé ni d'emoji décoratif.
