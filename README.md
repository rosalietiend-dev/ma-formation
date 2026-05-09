# 🎓 Inclusia — Plateforme de formation inclusive

Site web complet d'une école inclusive avec **assistant IA Inès** propulsé par Claude (Anthropic).

## 📁 Structure du projet

```
inclusia-projet/
├── backend-node/          # Backend Node.js (Express)
│   ├── server.js          # Serveur principal
│   ├── package.json       # Dépendances
│   └── .env.example       # Template de configuration
│
├── backend-python/        # Backend Python (FastAPI)
│   ├── main.py            # Serveur principal
│   ├── requirements.txt   # Dépendances
│   └── .env.example       # Template de configuration
│
├── frontend/              # Site web (HTML/CSS/JS pur)
│   ├── index.html         # Page d'accueil
│   ├── assets/
│   │   ├── styles.css     # CSS partagé
│   │   ├── data.js        # Données (formations, équipe, blog)
│   │   └── main.js        # Logique JS + connexion backend
│   └── pages/
│       ├── objectif.html         # Notre mission
│       ├── catalogue.html        # Catalogue formations
│       ├── blog.html             # Articles & actualités
│       ├── equipe.html           # L'équipe
│       └── espace-apprenant.html # Login + dashboard
│
└── docs/
    └── README.md          # Ce fichier
```

## 🚀 Démarrage rapide

### 1️⃣ Obtenir une clé API Anthropic

1. Créez un compte sur [console.anthropic.com](https://console.anthropic.com)
2. Générez une clé API (commence par `sk-ant-api03-...`)
3. ⚠️ **Ne committez JAMAIS cette clé dans Git !**

### 2️⃣ Choisir un backend (Node.js OU Python — au choix)

#### Option A — Backend Node.js

```bash
cd backend-node
npm install

# Créer le fichier .env
cp .env.example .env
# Éditer .env et coller votre clé Anthropic

# Démarrer
npm start
# Le serveur écoute sur http://localhost:3000
```

#### Option B — Backend Python

```bash
cd backend-python

# Créer un environnement virtuel
python -m venv venv
source venv/bin/activate    # Linux/Mac
# venv\Scripts\activate    # Windows

pip install -r requirements.txt

# Configurer
cp .env.example .env
# Éditer .env

# Démarrer
uvicorn main:app --reload --port 3000
# Doc auto : http://localhost:3000/docs
```

### 3️⃣ Lancer le frontend

Le frontend est en HTML/CSS/JS pur, **aucune compilation nécessaire**.

```bash
cd frontend
# Avec Python (déjà installé partout)
python -m http.server 8080

# OU avec Node
npx serve -p 8080

# OU simplement ouvrir frontend/index.html dans un navigateur
```

Ouvrez ensuite : **http://localhost:8080**

## 🤖 Fonctionnalités

### Assistant IA "Inès"
- Chatbot connecté à **Claude Opus 4.7** via votre backend
- Spécialiste : RQTH, AGEFIPH, FIPHFP, CPF majoré
- System prompt en français, ton bienveillant
- Suggestions de questions contextuelles
- **Fallback hors-ligne** si le backend est indisponible

### Sécurité
- ✅ Clé API Anthropic **uniquement côté serveur**
- ✅ CORS configuré strictement
- ✅ Rate limiting : 20 messages / 15 min / IP
- ✅ Validation stricte (max 500 caractères / message)
- ✅ Pas de logs de données sensibles

### Accessibilité (WCAG 2.1 AA)
- Mode dyslexie (Comic Sans + espacement)
- Contraste élevé
- Taille de texte ajustable (préférences sauvegardées)
- Sous-titres FR sur la vidéo YouTube
- Navigation clavier complète
- Lecteurs d'écran compatibles (NVDA, JAWS, VoiceOver)

### Pages
1. **Accueil** : Hero + vidéo YouTube + handicaps + services + simulateur financement
2. **Notre mission** : 3 piliers + chiffres clés + frise historique 2019-2026
3. **Catalogue** : 9 formations filtrables par catégorie
4. **Blog** : 6 articles filtrables par thème
5. **Équipe** : 8 membres avec bios
6. **Espace apprenant** : Login + dashboard (formations, ressources, certificats, référent)

## 🎬 Personnaliser la vidéo YouTube

Dans `frontend/index.html`, ligne ~67, remplacez `dQw4w9WgXcQ` par l'ID de votre vidéo :

```html
<iframe src="https://www.youtube.com/embed/VOTRE_ID?cc_load_policy=1&cc_lang_pref=fr&hl=fr&modestbranding=1&rel=0">
```

L'ID est la partie après `v=` dans l'URL YouTube : `youtube.com/watch?v=`**`VOTRE_ID`**

## 🔧 Endpoints API

Les deux backends exposent les mêmes routes :

| Route | Méthode | Description |
|-------|---------|-------------|
| `/api/health` | GET | Vérification du serveur |
| `/api/chat` | POST | Envoi message au chatbot Inès |
| `/api/evaluation` | POST | Réception formulaire d'évaluation |

Exemple de requête `/api/chat` :
```json
{
  "message": "Comment obtenir la RQTH ?",
  "history": [
    {"role": "user", "text": "Bonjour"},
    {"role": "bot", "text": "Bonjour ! Comment puis-je vous aider ?"}
  ]
}
```

## 📦 Déploiement en production

### Backend
- **Recommandé** : Render, Railway, Fly.io, DigitalOcean App Platform
- Configurer les variables d'environnement (`ANTHROPIC_API_KEY`, `FRONTEND_URL`)
- Activer HTTPS obligatoirement

### Frontend
- **Recommandé** : Vercel, Netlify, Cloudflare Pages (gratuits)
- Modifier `API_BASE` dans `assets/main.js` avec l'URL de votre backend en prod

### Domaine personnalisé
Modifier dans `main.js` :
```js
const API_BASE = window.location.hostname === 'localhost'
  ? 'http://localhost:3000'
  : 'https://api.inclusia.fr';  // ← Votre URL backend
```

## ⚠️ À faire avant la mise en production

- [ ] Remplacer toutes les références `0 800 XXX XXX` par votre vrai numéro
- [ ] Remplacer `contact@inclusia.fr` et autres emails par les vôtres
- [ ] Remplacer l'ID vidéo YouTube
- [ ] Configurer une vraie base de données pour `/api/evaluation` (PostgreSQL/MongoDB)
- [ ] Connecter un service email (SendGrid, Mailjet, Resend) pour les notifications
- [ ] Implémenter une vraie authentification pour l'espace apprenant (JWT, sessions)
- [ ] Audit accessibilité par un expert (RGAA 4.1)
- [ ] Mentions légales, CGU, politique de confidentialité (DPO)

## 💡 Évolutions possibles

- Intégration Stripe pour le paiement direct
- Système de visioconférence intégré (Whereby, Daily.co)
- Génération automatique de certificats PDF
- Notifications push (PWA)
- Application mobile (React Native, Flutter)
- Mode hors-ligne complet (Service Worker)

## 📞 Support

- **Email technique** : tech@inclusia.fr
- **Email commercial** : contact@inclusia.fr
- **Téléphone** : 0 800 XXX XXX (numéro vert)

## 📜 Licence

Projet propriétaire Inclusia. Tous droits réservés © 2026.
