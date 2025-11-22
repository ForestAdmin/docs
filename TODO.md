# Documentation TODO

## Issues à résoudre plus tard

### Design & UI
- [ ] **Toggle dark/light mode** - Le toggle est toujours visible en haut à droite malgré `isHidden: true`. Besoin de vérifier la version de Mintlify ou trouver une autre solution.
- [ ] **Améliorer les schémas d'architecture SVG** - Les illustrations dans le tableau des deployment options ne sont pas assez visibles/impactantes. À retravailler pour plus de clarté.
- [ ] **Revoir les liens de la homepage** - Vérifier que tous les liens pointent vers les bonnes pages et que la structure correspond à la navigation actuelle.
- [ ] **Mettre à jour le lien "Book a Demo"** - Le lien `https://www.forestadmin.com/demo` dans quickstart-on-premise.mdx pointe vers l'ancien site. À adapter avec le nouveau site quand il sera prêt.

### Architecture & Contenu

#### Self-Hosted : In-App vs Standalone
**Question :** Il existe deux façons de déployer en self-hosted :
1. **In-App** : L'agent Forest Admin fait partie intégrante de l'app du client (même codebase/process)
2. **Standalone/Microservice** : L'agent est une app séparée (microservice indépendant)

**Où en parler ?**
- Option 1 : Dans la page Quickstart (mais risque de surcharger)
- Option 2 : Dans Get Started > Connect > Setup (plus détaillé, séparé par langage)
- Option 3 : Dans Product > Integration > Architectures > Self-Hosted (page dédiée avec pros/cons)
- Option 4 : Dans les deux - mention rapide dans Quickstart + détails dans Architectures

**Recommandation initiale :**
- **Quickstart** : Garder simple, juste mentionner "Self-Hosted"
- **Architecture > Self-Hosted** : Créer deux sous-sections pour expliquer In-App vs Standalone
- **Setup pages (Node.js, Ruby)** : Montrer les deux approches avec des exemples de code

#### Forest Admin Models
- [ ] **Expliquer les models Forest Admin** - Créer une page ou section expliquant les modèles internes de Forest Admin (Collections, Fields, Relationships, Actions, Segments). Actuellement la page forest-admin-models.mdx a été supprimée car trop détaillée/hallucinations. À refaire de façon plus concise en se basant sur la vraie documentation.

