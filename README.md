`<a id="top"></a>`

<p align="center">
  <img src="logo_clean.png" width="120" alt="CS Analytics" />
</p>

<h1 align="center">CS Analytics</h1>
<p align="center"><strong>Suite Reports</strong> — Boîte à outils desktop pour l'équipe Customer Success (Citadel & Cryptobox)</p>

<p align="center">
  <img alt="Platform" src="https://img.shields.io/badge/platform-Windows-0078D6?logo=windows11&logoColor=white">
  <img alt="Python" src="https://img.shields.io/badge/python-3.12%2B-3776AB?logo=python&logoColor=white">
  <img alt="GUI" src="https://img.shields.io/badge/GUI-pywebview-1f6feb">
  <img alt="Tests" src="https://img.shields.io/badge/tests-pytest-0A9EDC?logo=pytest&logoColor=white">
  <img alt="Usage" src="https://img.shields.io/badge/usage-interne%20CSM-orange">
  <img alt="Status" src="https://img.shields.io/badge/statut-actif-brightgreen">
</p>

<p align="center">
  Rapports PowerPoint automatisés, analytics interactifs, mini-CRM partagé, calendrier CSM, calculateur de KPI et automatisations — tout dans une seule app desktop, sans serveur, sans droits admin.
</p>

---

## 🔒 Sécurité & Conformité Cybersécurité (100% Hors-ligne)

Conçu pour des environnements d'entreprise hautement restreints :

- **Calculs locaux uniquement** : Tous les traitements de données, le parsing des logs d'activité et la génération de rapports PowerPoint sont exécutés **100 % en local sur le poste de l'utilisateur**.
- **Aucune dépendance cloud** : Aucune donnée client, identifiant, log parsé ou rapport n'est envoyé vers des serveurs cloud externes, éliminant tout vecteur de fuite de données et garantissant un alignement strict avec les exigences de cybersécurité les plus rigoureuses.

---

## 📑 Sommaire

- [✨ Fonctionnalités](#fonctionnalites)
- [🗂️ Architecture des données](#architecture-des-donnees)
- [🚀 Installation](#installation)
- [🖥️ Utilisation](#utilisation)
- [📂 Structure du projet](#structure-du-projet)
- [🛠️ Développement](#developpement)
- [🩹 Dépannage](#depannage)

---

`<a id="fonctionnalites"></a>`

## ✨ Fonctionnalités

|      | Page                                | Description                                                                                                               |
| ---- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 📦   | **Cryptobox Report**          | Génération de rapports PowerPoint d'usage Box, personnalisables par client, FR/EN.                                      |
| 🛡️ | **Citadel Report**            | Génération de rapports d'activité Citadel (comptes, messages, salons, licences).                                       |
| 📊   | **Analytics**                 | Tableau de bord interactif (graphiques ECharts) avec comparatif multi-périodes et jalons calendaires.                    |
| ⭐   | **Mes clients**               | Portefeuille personnel du CSM — KPI clés des clients favoris,*strictement local et privé*.                           |
| 🤝   | **Fiches Clients (Mini CRM)** | Contacts, licences, cas d'usage — base**partagée** entre tous les CSM, fusionnée automatiquement.                |
| 🗓️ | **Calendrier CSM**            | Suivi des déploiements, visites, incidents — export/import `.ics`.                                                    |
| 🧮   | **Calculateur de KPI**        | Indicateurs personnalisés à partir de formules sur vos colonnes Excel/CSV, réutilisables partout.                      |
| 🎥   | **Présentation**             | Mode diaporama plein écran, double-écran client, édition live des valeurs.                                             |
| ⚙️ | **Automatisation & Console**  | Envoi par Outlook, consolidation de fichiers, et Console API (style Postman) pour requêtes infra et gestion de domaines. |

> Un éditeur de données intégré (`data_grid_helper.py`) permet aussi de corriger des valeurs source avant génération, sans toucher au fichier d'origine.

### ⚙️ Console API (Style Postman)

La page **Automatisation** intègre une Console API complète permettant de piloter des opérations sur les infrastructures en direct :

- **Requêtes HTTP libres** : GET, PUT, POST, DELETE avec configuration d'URI, headers et corps JSON. Authentification par certificat `.p12` ou par Bearer Token.
- **Sélection interactive de lots d'infrastructures** : Sélection via un menu déroulant ou ajout par saisie manuelle (avec validation par la touche Entrée), affichées sous forme de tags (chips) interactifs.
- **Sélecteur de tenants Citadel** : Extraction automatique des tenants Citadel à partir du CRM et des exports de données locaux. Intègre des cases à cocher et une synchronisation bidirectionnelle en temps réel avec le corps JSON.
- **Affichage intelligent conditionnel** : Le sélecteur de tenants et les contrôles associés s'affichent uniquement pour les endpoints de type messages bandeaux (`/api/infra_messages`), épurant l'interface pour les autres endpoints.
- **Gestion des requêtes** : Enregistrement rapide des templates de requêtes et renommage direct depuis l'interface (bouton ✏️).

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="architecture-des-donnees"></a>`

## 🗂️ Architecture des données

Les fichiers de configuration et de données de l'application sont répartis selon leur utilité (locaux/privés ou partagés entre CSM sur le réseau) :

| Fichier                   | Contenu                                                                        | Portée                                                                                                       |
| ------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `favoris.json`          | `{client, produit}` — le portefeuille personnel coché dans *Mes clients* | 🔒**Local uniquement**, jamais synchronisé                                                             |
| `clients.json`          | Fiches CRM complètes (contacts, licences, cas d'usage...)                     | 🌐**Partagé** entre CSM via Cryptobox, fusion bidirectionnelle (la version la plus récente l'emporte) |
| `infra_lots.json`       | Définition des lots d'infrastructures pour les exécutions groupées          | 🌐**Partagé** entre CSM via Cryptobox, fusion bidirectionnelle à l'enregistrement                     |
| `console_requests.json` | Requêtes d'administration rapides enregistrées par l'utilisateur             | 🔒**Local uniquement** (sécurisé / propre à chaque CSM)                                              |
| `scripts_history.json`  | Historique des exécutions de requêtes et scripts via la console              | 🔒**Local uniquement** (sécurisé / propre à chaque CSM)                                              |

Tous ces fichiers disposent d'une copie miroir dans `%APPDATA%\SuiteReports` en filet de sécurité. Logique réseau gérée dans `config_helper.get_shared_sync_dir()`.

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="installation"></a>`

## 🚀 Installation

> Aucune compétence technique ni droit administrateur requis.

```text
1. Décompressez entièrement le dossier reçu (ex: Suite_Reports_Windows)
2. Double-cliquez sur INSTALLER.bat
3. Laissez le script s'exécuter — il installe Python, les dépendances et vérifie WebView2
```

✅ Une fois l'installation terminée, plus besoin de relancer l'installateur.

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="utilisation"></a>`

## 🖥️ Utilisation

1. Double-cliquez sur **`Lancer_Suite_Reports.bat`** — une vérification de mise à jour réseau s'exécute automatiquement avant le démarrage.
2. Naviguez entre les pages depuis le menu de gauche.
3. Configurez vos chemins (dossiers, templates) — ils sont **sauvegardés automatiquement**.
4. Choisissez le client et les dates → **Générer le rapport**.

Une notification apparaît en fin de génération pour ouvrir le rapport ou son dossier de sortie.

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="structure-du-projet"></a>`

## 📂 Structure du projet

```text
Suite_Reports_Dev/
├── main.py                  # Point d'entrée — classe Api exposée au frontend (pywebview)
├── config_helper.py         # Chemins, INI, sécurité, backup APPDATA, synchro réseau
├── calendrier_helper.py     # Calendrier CSM + favoris (local) + CRM (partagé)
├── infra_scripts.py         # Exécution et templating des scripts d'administration / Console API
├── ppt_helpers.py           # Moteur de génération PPTX ⚠️ fichier le plus sensible
├── data_grid_helper.py      # Éditeur de données source
├── consolidation.py         # Fusion multi-fichiers Excel/CSV
│
├── scripts_infra.json       # Modèles de requêtes pré-configurés (Domains Management, etc.)
│
├── analytics/                # Adaptateurs par produit (registry pattern)
│   ├── registry.py
│   ├── citadel.py
│   └── cryptobox.py
│
├── Citadel_Report_Tool/     # 🚫 Moteur autonome — boîte noire, ne jamais modifier
├── Script Cryptobox/        # 🚫 Moteur autonome — boîte noire, ne jamais modifier
│
├── *.html, js/, css/        # Frontend — une page par section
├── tests/                   # Suite pytest (dev uniquement)
├── handoffs/                # Fiches de transmission pour les agents IA
```

> ⚠️ **Règle absolue** : `Citadel_Report_Tool/` et `Script Cryptobox/` sont des moteurs externes historiques appelés en `subprocess`. Les modifier casserait la compatibilité avec les rapports existants et les installations client.

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="developpement"></a>`

## 🛠️ Développement

<details>
<summary><strong>Cliquer pour déplier — prérequis, tests, synchronisation, règles d'or</strong></summary>

> Tout le développement se fait **exclusivement dans `Suite_Reports_Dev/`**. `Suite_Reports_Windows/` et `Suite_Reports_Launcher/` sont des copies de distribution générées automatiquement — ne jamais les éditer directement.

### Prérequis

```bash
pip install -r requirements.txt
```

### Lancer en mode développement

```bash
python main.py
```

L'app démarre dans une fenêtre native **pywebview** (WebView2) — pas de serveur à ouvrir dans un navigateur.

### Lancer les tests

```bash
pytest tests/ -q
```

Tous les tests doivent passer avant de livrer.

### Synchroniser les distributions

```bash
python ../sync_reports.py
```

Propage vers `Suite_Reports_Windows/`, `Suite_Reports_Launcher/` et le partage réseau si détecté.

### Architecture simplifiée

```text
Frontend (HTML/JS) ←→ pont pywebview (js_api) ←→ Backend Python (classe Api, main.py)
                                                        │
                  ┌─────────────────────────────────────┘
                  │
      ┌───────────┴────────────┬──────────────────┬─────────────────────┐
      │                        │                   │                     │
 analytics/               ppt_helpers.py    calendrier_helper.py    config_helper.py
 (KPI par produit)         (génération PPTX) (calendrier/favoris/CRM) (chemins, INI, synchro)
```

### Règles d'or

1. 🔴 **`Citadel_Report_Tool/` et `Script Cryptobox/` sont intouchables** — boîtes noires exécutées en `subprocess`.
2. ✅ Toujours exécuter `pytest` (et `sync_reports.py` si propagation nécessaire) avant de livrer.
3. 🔌 Toute nouvelle méthode `Api` doit utiliser `@_api_endpoint(...)` pour la validation et la gestion d'erreur.
4. 📦 Les imports `pptx` restent dans `ppt_helpers.py`, jamais dans `main.py`.
5. 🛡️ Utiliser systématiquement `valider_nom_client()`, `valider_date_iso()`, `safe_subpath()` (`config_helper.py`) sur toute entrée utilisateur ou chemin.
6. 🔒 `favoris.json` ne doit **jamais** être écrit sur le réseau — toute nouvelle donnée locale-uniquement doit suivre ce même principe.

</details>

<p align="right">(<a href="#top">retour en haut</a>)</p>

`<a id="depannage"></a>`

## 🩹 Dépannage

- L'app ne se lance pas → vérifiez que le dossier est bien dézippé (pas lancé depuis l'intérieur du zip).
- Erreur au démarrage ou à la génération → consultez `launcher.log` (ou le bouton **Log** dans l'app).
- Dépendances manquantes ou environnement douteux → relancez `INSTALLER.bat`.

<p align="right">(<a href="#top">retour en haut</a>)</p>

---

<p align="center"><sub>Outil interne Customer Success — Citadel & Cryptobox</sub></p>
