# CS Analytics — Démo interactive

Démonstration **interactive et hors-ligne** d'une application desktop d'**analytics & d'automatisation** pour des équipes Customer Success.

> 🔒 **Données 100 % fictives.** Noms de clients, produits, infrastructures et chiffres sont inventés. Aucune donnée réelle, aucun secret. Cette démo reconstruit *l'interface et les concepts* d'une application interne, à des fins de présentation.

## ▶️ Voir la démo

- **En ligne** : *(active GitHub Pages, voir plus bas — l'URL apparaîtra ici)*
- **En local** : télécharge `index.html` et ouvre-le par double-clic. Aucun serveur, aucune dépendance, aucune connexion requise.

## ✨ Ce que montre la démo

| Section | Contenu |
|---|---|
| **Vue d'ensemble** | Panorama des fonctionnalités (rapports PPTX, calendrier, calculateur KPI, consolidation…). |
| **Analytics** | Tableau de bord interactif : sélection client / produit / période → KPIs, courbes d'évolution (SVG), jauges de consommation. Génération de rapport simulée. |
| **Clients & CRM** | Découplage **portefeuille privé (local)** vs **fiches partagées (équipe)**, fusionnées sans conflit. |
| **Console API** | Composition d'une requête (méthode, endpoint, corps JSON), **aperçu de bandeau FR/EN** en direct, envoi simulé à un **lot multi-infrastructures** avec résultats par cible. |
| **Architecture & sécurité** | Le pont natif front ⇄ back, les choix de sécurité (secrets éphémères, mTLS, sous-processus sûrs) et de fiabilité des données. |

## 🛠️ Stack de l'application d'origine

Python · PyWebView (WebView2) · pandas · python-pptx · HTML/CSS/JS vanilla · suite de tests pytest.
*(La démo elle-même est un unique fichier HTML statique : la dataviz est dessinée en SVG maison, sans aucune librairie externe, pour fonctionner partout hors-ligne.)*

<sub>Démo de présentation — données fictives — © sans données réelles.</sub>
