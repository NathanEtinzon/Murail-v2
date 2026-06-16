➡️ [Read this documentation in English](README-en.md)

# Mur@il
Plateforme de simulation de crise inspirée de l’exercice massifié REMPAR25 de l’ANSSI

Ce dépôt est un fork du projet original [JMousqueton/Murail](https://github.com/JMousqueton/Murail).

# Simulation de crise – Exercice inspiré de REMPAR25

## 📌 Contexte

Ce projet est né de l’exercice **REMPAR25**, un exercice de cybersécurité massifié organisé par l’**ANSSI** en 2025 en France.  
L’objectif est de mettre en situation des équipes afin de tester leur réactivité et leur coordination en cas de cyberattaque ou d’incident majeur.  

La plateforme permet de **simuler des canaux de communication réalistes** (réseaux sociaux, stimulus par messagerie interne) alimentés par un scénario défini dans un fichier Excel.  
Elle peut être utilisée lors de formations, de jeux de rôle ou d’exercices de gestion de crise.

---

## 🎯 Objectifs du projet

- Reproduire un environnement immersif simulant :
  - Un **réseau social** type Twitter.
  - Une **messagerie interne** type webmail, avec rôles (RH, Communication, Décision, etc.).
- Fournir aux participants un environnement simple d’accès, utilisable via un navigateur web.
- Permettre aux formateurs / encadrants de suivre la progression de l’exercice :
  - Une **console administrateur** pour charger et suivre le scénario.
  - Une **vue animateur** permettant d’analyser en temps réel l’évolution de l’exercice.

---

## ⚙️ Fonctionnalités principales

### 🔑 Authentification
- Accès **administrateur** protégé par mot de passe.
- Accès **animateur** protégé par un mot de passe distinct.
- Gestion des rôles via la messagerie (Communication, Décision, Informatique, RH, Juridique/Finance, etc.).

### 📊 Administration
- Téléversement du fichier Excel de scénario (`chronogramme.xlsx`).
- Téléversement du fichier Excel des réseaux sociaux (`PMS.xlsx`).
- Affichage des événements passés et des prochains messages/tweets planifiés.
- Suivi du nombre total de tweets et messages.

### 🐦 Réseaux sociaux
- Fil d’actualité imitant **Twitter**.
- Affichage des tweets programmés au fil du temps.
- Engagement dynamique (likes, retweets) qui évoluent automatiquement.
- Détection et affichage des **hashtags** tendances.
- Possibilité de filtrer la timeline par hashtag.

### ✉️ Messagerie interne
- Vue **webmail** avec sélection du profil utilisateur.
- Les messages s’affichent au fil de l’eau, en fonction du rôle choisi.
- Ajout d’un **mode “tous”** pour les messages destinés à l’ensemble des rôles.
- Chaque utilisateur peut marquer un message comme **“Traité”** (stocké en local sur son navigateur, sans impact sur les autres).

### 🪄 Animateur
- Accès réservé par mot de passe.
- Timeline affichant uniquement les **messages** (pas les tweets).
- Pour chaque message :
  - ID du stimulus en surbrillance (badge jaune).
  - Horaire de diffusion.
  - **Réaction attendue** (🔎) et **Commentaire** (📝) associés.
- Vue permettant de suivre en parallèle le déroulement et d’évaluer les réactions.

### 👁️ Observateur
- Accès réservé par mot de passe.
- Timeline affichant uniquement les **messages** (pas les tweets).
- Pour chaque message, l'observateur peut noter la réaction de la cellule de crise par un pouce vers le haut 👍 ou le bas 👎 et ajouter un commentaire 
- Les informations saisie sont stockées en local dans le navigateur dans l'observateur 
- Export en JSON ou CSV 

---

## 📂 Structure des fichiers de scénario (Excel)

La plateforme utilise **deux fichiers Excel distincts** :

### 1. **Chronogramme** (messages et événements)
Le fichier Excel `chronogramme.xlsx` doit contenir au minimum les colonnes suivantes :

- `id` : identifiant unique du stimulus (pour les messages).
- `horaire` : heure de diffusion (format `HH:MM` ou `HH:MM:SS`).
- `type` : `message` ou `decompte`.
- `emetteur` : auteur du message.
- `destinataire` : rôle(s) concerné(s) (ou `tous` pour diffusion générale). *Support multi-destinataires sur plusieurs lignes.*
- `stimuli` : contenu du message.
- `reaction attendue` *(optionnel)* : ce qui est attendu de l'équipe.
- `commentaire` *(optionnel)* : note pour l'animateur. Pour un `decompte`, ce texte est affiché sous le compte à rebours sur la page d'accueil et devient donc visible par les participants.
- `livrable` *(optionnel)* : sortie attendue (communiqué, rapport, etc.).

**Types supportés :**
- `message` : message interne diffusé aux rôles désignés.
- `decompte` : fenêtre de décompte (compteur à rebours pour l'exercice).

### 2. **PMS** (tweets) — *Optionnel, nécessite `ENABLE_PMS=true`*
Le fichier Excel `pms.xlsx` doit contenir au minimum les colonnes suivantes :

- `horaire` : heure de diffusion (format `HH:MM` ou `HH:MM:SS`).
- `emetteur` : auteur du tweet (compte Twitter simulé).
- `stimuli` : contenu du tweet.

---

## 🆕 Nouveautés (dernière mise à jour)

### Architecture améliorée
- **Séparation des sources** : tweets et messages chargeables depuis des fichiers Excel distincts
- **Extraction dynamique des rôles** : les rôles sont auto-extraits à partir des destinataires des messages
- **Support multi-destinataires** : un message peut être destiné à plusieurs rôles (avec sauts de ligne dans le CSV)

### Interface d'administration
- Interface simplifiée avec téléversement séparé pour :
  - **Chronogramme** (messages + décomptes)
  - **PMS** (tweets) — optionnel, nécessite activation
- Affichage du statut de chargement pour chaque module

### Gestion des timestamps
- Meilleure gestion des formats Excel et des fuseaux horaires
- Support automatique de formats d'heure variables (`HH:MM`, `HH:MM:SS`, etc.)

### Décomptes
- Affichage possible d'un message sous le compte à rebours depuis la colonne `Commentaire` du chronogramme.
- Rafraîchissement automatique de la page d'accueil toutes les 30 secondes pour basculer vers un décompte actif sans action manuelle.

### Améliorations techniques
- Pinning des versions des dépendances (`requirements.txt`)
- Gestion améliorée des verrous (threading) pour les structures partagées
- Support i18n complet avec traduction des nouvelles clés
- Headers no-cache pour éviter les problèmes de mise en cache des SSE
- Normalisation des langues système (`fr_FR.UTF-8`, `en_US.UTF-8`) et chargement explicite du `.env` avec Docker Compose.

---

Une documentation complète en français expliquant le fonctionnement et la préparation des fichiers Excel est disponible ici :  
➡️ [Documentation/Documentation-fr.md](Documentation/Documentation-fr.md)

---

## 🚀 Installation

### 1. Prérequis
- Python **3.9+**
- Pip et virtualenv

### 2. Installation locale
```bash
git clone https://github.com/jmousqueton/murail.git
cd murail
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

**Dépendances** (versions recommandées) :
- Flask==3.1.2
- pandas==2.3.3
- openpyxl==3.1.5
- python-dateutil==2.9.0.post0
- python-dotenv==1.2.1
- Unidecode==1.4.0

### 3. Configuration

Copier le fichier de configuration exemple et l'adapter :
```bash
cp env.example .env
```

Éditer le fichier `.env` et remplir les variables nécessaires. Voir [env.example](env.example) pour une description détaillée de chaque variable.

**Variables principales :**

```env
# Authentification (recommandé: mots de passe différents)
ADMIN_PASSWORD=MonMotDePasseAdmin
ANIMATOR_PASSWORD=MonMotDePasseAnimateur
OBSERVER_PASSWORD=MonMotDePasseObservateur

# Configuration
APP_ID=SIM-MURAIL
FLASK_SECRET=ma-cle-ultra-secrete-longue      # Générer: python3 -c "import secrets; print(secrets.token_hex(32))"
TZ=Europe/Paris                                # Fuseau horaire (ex: Europe/Paris, UTC)
LANG=fr                                        # Langue par défaut (fr ou en)

# Fichiers scénarios
CHRONOGRAMME_FILE=Sample/chronogramme.xlsx     # Messages et décomptes
ENABLE_PMS=true                                # Activer le module PMS (tweets)
PMS_FILE=Sample/pms.xlsx                       # Tweets (nécessite ENABLE_PMS=true)

# Optionnel
DEBUG=false                                    # Mode débogage Flask (ne pas activer en production)
DEMO=false                                     # Mode démo (bypass auth pour démonstration)
TRACKING=                                      # Code de suivi (ex. Google Analytics)
PORT=5000                                      # Port d'écoute (par défaut: 5000)
```

**Pour plus de détails**, consulter le fichier [env.example](env.example) qui contient les explications de chaque variable.

### 4. Lancer l’application
```bash
python app.py
```

L’application est alors disponible sur [http://localhost:5000](http://localhost:5000).

---
## 🐳 Déploiement avec Docker

### Option 1 : Docker Compose (recommandé)

La méthode la plus simple pour déployer Murail :

```bash
# 1. Créer un fichier .env avec vos configurations
cp env.example .env
# Éditer .env et configurer vos mots de passe et paramètres

# 2. Lancer l'application
docker-compose up -d

# Après modification du .env, recréer le conteneur pour appliquer les variables
docker-compose up -d --force-recreate

# 3. Voir les logs
docker-compose logs -f

# 4. Arrêter l'application
docker-compose down
```

L'application sera accessible sur [http://localhost:5000](http://localhost:5000).

### Option 2 : Docker seul

```bash
# 1. Construire l'image
docker build -t murail:latest .

# 2. Lancer le container
docker run -d \
  --name murail-app \
  -p 5000:5000 \
  -e ADMIN_PASSWORD=votremotdepasse \
  -e ANIMATOR_PASSWORD=votremotdepasse2 \
  -e OBSERVER_PASSWORD=votremotdepasse3 \
  -e FLASK_SECRET=$(python3 -c "import secrets; print(secrets.token_hex(32))") \
  -v $(pwd)/Sample:/app/Sample:ro \
  murail:latest
```

### Scénarios personnalisés avec Docker

Pour utiliser vos propres fichiers Excel :

```bash
# Placer vos fichiers dans ./custom-scenarios/
docker-compose up -d

# Ou avec docker run :
docker run -d \
  -p 5000:5000 \
  -v $(pwd)/custom-scenarios:/app/custom-scenarios:ro \
  -e CHRONOGRAMME_FILE=/app/custom-scenarios/mon-scenario.xlsx \
  murail:latest
```

### Health Check

Vérifier la santé du container :

```bash
# Via Docker
docker inspect --format='{{.State.Health.Status}}' murail-app

# Via HTTP
curl http://localhost:5000/health
```

### Production

Pour un déploiement en production, considérer :

- Utiliser un reverse proxy (nginx, Traefik)
- Activer HTTPS avec Let's Encrypt
- Configurer des mots de passe forts
- Conserver `DEBUG=false` et `DEMO=false`
- Limiter l'accès réseau avec des règles firewall

---
## 👥 Public cible

- **Organisateurs d’exercices de crise** (RSSI, DSI, formateurs).
- **Équipes de communication, juridique, RH, Finance, technique** lors d’un entraînement.
- **Animateurs** chargés d’évaluer la réaction et la coordination.

---

## Démo en ligne

Une instance de démonstration est disponible à l’adresse suivante :  
👉 [https://murail-demo.mousqueton.io](https://murail-demo.mousqueton.io)

En mode démo :

- L’accès à l’**Animateur** ne nécessite pas de mot de passe.
- L’accès à l’**Observateur** ne nécessite pas de mot de passe.
- L'accès à l'**administrateur** n'est pas accessible 
- Les autres fonctionnalités (Messagerie, Réseaux sociaux) restent accessibles pour tester le scénario.
- Ce mode est uniquement prévu pour découvrir l’outil.

---

## 🚀 ToDo

- [ ] Ajout d’un **mode clair/sombre** (préférence sauvegardée dans le navigateur)
- [ ] Possibilité de publier ses propres tweet (limité à la fonction comm)
- [ ] Générer un PDF à partir des remarques de l'**observateur**

---

## 📜 Licence

Projet distribué sous licence GNU.  
⚠️  Ce projet est destiné à la **formation et simulation uniquement**.

---

## 🙏 Remerciements

- **JMousqueton/Murail** pour le projet original dont ce dépôt est un fork.
- **ANSSI** pour l’organisation de **REMPAR25**, qui a inspiré cette plateforme.  
- Tous les contributeurs qui enrichissent les exercices de cybersécurité massifiés.
