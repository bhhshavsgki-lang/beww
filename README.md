# Leboncoin Deal Finder — filtre local sans notifications

Cette version contient un moteur de filtrage et de sauvegarde entièrement local : **pas de Gmail, pas d'IA, pas de Telegram, pas de Discord et aucun secret GitHub**.

## Ce que le programme fait

Il lit de vraies annonces placées dans `data/input_ads.json`, puis applique les règles configurées dans `config/settings.json` : mots-clés, prix maximum, mots à rejeter, risque, doublons et distance lorsque les coordonnées sont disponibles. Les annonces retenues sont ajoutées dans `data/good_deals.md`, et l'historique est conservé dans `data/state.json`.

Le workflow GitHub Actions s'exécute toutes les six heures et commit automatiquement le rapport et l'état.

## Ce que le programme ne fait pas

Il ne recherche pas directement dans les pages Leboncoin. Sans une source autorisée d'annonces, GitHub ne peut pas découvrir seul les nouvelles publications. Le navigateur de test a montré que Leboncoin demande une vérification anti-robot, et son fichier `robots.txt` interdit l'accès automatique aux pages de recherche sans permission.

Il ne faut donc pas présenter cette archive comme un scraper automatique. Pour fournir des annonces réelles au filtre, il faut soit les ajouter à `data/input_ads.json`, soit connecter plus tard une source autorisée.

## Format de vraie annonce

`data/input_ads.json` doit contenir uniquement des annonces réelles :

```json
[
  {
    "title": "Titre exact vu sur Leboncoin",
    "url": "https://www.leboncoin.fr/ad/...",
    "price_eur": 280,
    "location": "Ville et code postal",
    "latitude": 48.8566,
    "longitude": 2.3522,
    "description": "Description réelle de l'annonce",
    "published_at": "2026-09-29T10:00:00Z",
    "image_urls": [],
    "source_search": "iPhone 14 128 Go"
  }
]
```

Pour le moment, le fichier est volontairement vide (`[]`) : aucune fausse annonce n'est incluse dans le ZIP.

## Installation GitHub

1. Copiez `config/settings.example.json` vers `config/settings.json`.
2. Remplacez la ville, les coordonnées, les produits et les prix.
3. Ajoutez uniquement de vraies annonces dans `data/input_ads.json`.
4. Envoyez tout le projet dans votre dépôt GitHub.
5. Ouvrez **Actions → Check local Leboncoin data → Run workflow**.

Aucun secret, compte Gmail, clé IA ou bot Telegram n'est requis.

## Test local

```bash
python -m src.main
python -m pytest -q
```

Avec un fichier d'entrée vide, l'exécution normale est :

```text
read: 0, processed: 0, saved: 0
```
