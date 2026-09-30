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

## 3. Référence des recherches (2) : 
**Feedback**
Rowland, C. A. (2014). Psychological Bulletin, 140(6), 1432-1463.
Butler, A. C., & Roediger, H. L. (2008). Memory & Cognition, 36(3), 604-616. https://doi.org/10.3758/MC.36.3.604

**Spacing effect**
Cepeda, N. J., Pashler, H., Vul, E., Wixted, J. T., & Rohrer, D. (2006). Psychological Bulletin, 132, 354-380.
Latimier, A., Peyre, H., & Ramus, F. (2021). Educational Psychology Review, 33(3), 959-987. https://doi.org/10.1007/s10648-020-09572-8

**Expanding retrieval**
Karpicke, J. D., & Roediger, H. L. (2007). JEP: Learning, Memory, and Cognition, 33(4), 704-719. https://doi.org/10.1037/0278-7393.33.4.704
Latimier et al. (2021), ci-dessus.

**Desirable difficulties**
Bjork, R. A. (1994). In J. Metcalfe & A. Shimamura (Eds.), Metacognition: Knowing about knowing (pp. 185-205). MIT Press.
Bjork, E. L., & Bjork, R. A. (2011). In M. A. Gernsbacher et al. (Eds.), Psychology and the real world (pp. 56-64). Worth Publishers.

**Interleaving**
Kornell, N., & Bjork, R. A. (2008). Psychological Science, 19(6), 585-592 (volume et pages à revérifier). https://doi.org/10.1111/j.1467-9280.2008.02127.x
Kang, S. H. K., & Pashler, H. (2012). Applied Cognitive Psychology, 26, 97-103.
Brunmair, M., & Richter, T. (2019). Psychological Bulletin, 145(12), 1212-1244. https://doi.org/10.1037/bul0000209

**Leitner / SM-2**
Leitner, S. (1972). So lernt man lernen. Verlag Herder.
Woźniak, P. A. (1990). Algorithme SM-2, publié sur supermemo.com

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
