# Hébergement sur le VPS

Le site reste statique. Caddy sert ce répertoire et transmet uniquement la
liste publique des concerts à l’API Faille Ouverte.

Configuration de principe à adapter par root à l’installation Caddy existante :

```caddyfile
fjordinslovenia.eu, www.fjordinslovenia.eu {
    handle /api/concerts {
        rewrite * /api/public/concerts
        reverse_proxy 127.0.0.1:3000
    }

    root * /var/www/fjordinslovenia.eu
    file_server
}
```

Root doit publier uniquement les fichiers suivis par Git dans
`/var/www/fjordinslovenia.eu`; le CSV privé ne doit jamais y être copié.

Si Caddy tourne en conteneur, root doit monter ce répertoire en lecture
seule et remplacer `127.0.0.1:3000` par le nom réseau du service FISOS.

Le CSV de suivi est privé et ignoré par Git. Il ne doit jamais être servi par
Caddy.
