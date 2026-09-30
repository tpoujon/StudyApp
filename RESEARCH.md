# RESEARCH — StudyApp

## 1. Quizlet — modes de base
- **Flashcards** : recto/verso feuilleté ; progression purement déclarative ("je sais / à revoir"), aucune preuve de rappel.
- **Learn** : moteur adaptatif ; enchaîne flashcard → QCM 4 choix → saisie libre. Monte en difficulté quand les réponses sont justes, redescend après une erreur ; suit les termes ratés et les redrille. Progression = anneau "en cours / maîtrisé".
- **Write** : définition montrée, l'utilisateur tape le terme ; correction tolérante, terme raté rejoué dans le round.
- **Match** : appariement termes ↔ définitions chronométré ; travaille la vitesse de récupération, effet de nouveauté et d'engagement plus que la rétention.
- **Test** : examen généré (QCM + vrai/faux + saisie), une passe notée sans reprise.
- Ce qui rend Learn "adaptatif" : le type de question dépend du niveau estimé carte par carte, et les cartes faibles sont priorisées.

## 2. Psychologie de l'apprentissage
- **Testing effect** (Roediger & Karpicke 2006) : le rappel actif bat la relecture pour la rétention durable, même quand la session paraît plus difficile sur le moment.
- **Feedback immédiat** : montrer la correction juste après la tentative est le facteur le mieux documenté de l'efficacité du test.
- **Spacing effect** : la pratique espacée bat la pratique massée.
- **Expanding retrieval** : intervalles croissants ; le bénéfice suppose que le premier rappel ne soit pas trop précoce (littérature récente : effet parfois nul si le 1er test suit l'étude de trop près).
- **Desirable difficulties** (Bjork) : rendre la récupération coûteuse (délai, variation des conditions) améliore l'apprentissage à long terme.
- **Interleaving** : alterner catégories et types de questions améliore l'apprentissage inductif, souvent plus que l'espacement seul.
- **Leitner / SM-2** : boîtes à intervalles croissants, échec = retour boîte 0 ; SM-2 ajoute un facteur de facilité (`ease`) propre à chaque carte.

## Décisions
- Testing effect → aucun mode de relecture pure ; le mode Cartes impose l'auto-évaluation 3 boutons, pas de bouton "suivant" neutre.
- Feedback immédiat → après chaque réponse : correct/incorrect + réponse attendue affichés avant la carte suivante.
- Rappel actif > reconnaissance → une carte n'est "acquise" qu'après 2 saisies libres justes consécutives ; le QCM ne fait jamais passer une carte.
- Expanding retrieval → intervalles box 0→5 = 0, 1, 2, 4, 8, 16 jours (le "0" reste une réinjection intra-session, pas un vrai intervalle espacé — voir plan).
- Desirable difficulty → carte ratée réinjectée après 3 à 5 cartes, jamais immédiatement.
- Interleaving → la file de session mélange les catégories par défaut ; "bloquer par catégorie" est opt-in.
- Spacing → file priorisée en retard > en cours > neuves ; `dueDate` par carte ; session plafonnée (défaut 20) pour tenir 5–10 min.
- SM-2 allégé → `ease` module la longueur d'intervalle ; échec → box 0 + `lapses++`.
- Learn adaptatif → QCM tant que box ≤ 1, bascule en saisie libre dès box ≥ 2 ; seule la saisie libre compte pour la maîtrise.
