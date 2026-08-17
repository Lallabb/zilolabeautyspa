# Zilola Beauty Spa — Website Delivery Package

Production-ready static website for **Zilola Beauty Spa**.

- No framework
- No build step
- No database
- No server-side runtime required
- Responsive desktop/mobile experience
- Optional Docker/Nginx deployment included

## Package contents

```text
index.html          Main landing page
menu.html           Services/menu page
gallery.html        Gallery entry page
assets/              Images, Hero frames, logos and Lookbook media
netlify.toml         Netlify configuration
Dockerfile           Docker image based on Nginx
nginx.conf           Nginx site configuration
docker-compose.yml   One-command Docker deployment
.dockerignore        Excludes non-production files from Docker builds
README.md            This guide
```

> The folder containing `index.html` is the website root. Upload the **contents of this folder**, not an additional parent folder.

---

# English deployment guide

## Option 1 — Netlify, easiest method

### Browser upload

1. Extract the ZIP file.
2. Sign in to Netlify.
3. Create a new site using Netlify's manual deploy / drag-and-drop option.
4. Drag the extracted folder that directly contains `index.html` into the deployment area.
5. Wait for the deploy to finish and open the generated Netlify URL.
6. In **Domain management**, add the final custom domain.
7. Follow Netlify's DNS instructions for the domain. HTTPS is issued automatically after DNS validation.

No build command is required. The publish directory is the project root: `.`

### Netlify CLI alternative

From this folder:

```bash
npx netlify-cli deploy --prod --dir=.
```

The included `netlify.toml` configures the publish directory, cache headers and HTML fallback.

## Option 2 — Any traditional web hosting / cPanel

1. Extract the ZIP.
2. Open the host's File Manager or connect by FTP/SFTP.
3. Open the public web directory, usually one of:
   - `public_html/`
   - `www/`
   - `htdocs/`
4. Upload all package contents so that the final path is:

```text
public_html/index.html
public_html/assets/...
```

5. Point the domain to the hosting account and enable HTTPS.

Do not upload the files as `public_html/Zilola-Website/index.html` unless the website is intentionally meant to run under `/Zilola-Website/`.

## Option 3 — Docker on any VPS/server

Requirements: Docker with the Compose plugin.

From this folder:

```bash
docker compose up -d --build
```

The site will be available on:

```text
http://SERVER-IP:8080
```

Verify it:

```bash
curl -I http://127.0.0.1:8080/
docker compose ps
```

To use a real domain, place Caddy, Traefik, Nginx Proxy Manager or another HTTPS reverse proxy in front of port `8080`.

Example Caddy configuration:

```caddyfile
www.example.com {
  reverse_proxy 127.0.0.1:8080
}
```

Then point the domain's DNS `A` record to the server IP.

### Docker management

```bash
# View status
docker compose ps

# View logs
docker compose logs -f

# Rebuild after a website update
docker compose up -d --build

# Stop this website only
docker compose down
```

## Option 4 — Direct Nginx installation

Copy the website files to a directory such as:

```text
/var/www/zilola/
```

Minimal Nginx virtual host:

```nginx
server {
    listen 80;
    server_name www.example.com example.com;
    root /var/www/zilola;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Validate and reload:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

Add HTTPS with the server's preferred certificate system, for example Certbot or an external reverse proxy.

## External services used by the website

The website itself is static, but these public services require internet access:

| Service | Purpose |
|---|---|
| Google Fonts | Italiana and DM Sans typography |
| cdnjs | GSAP and ScrollTrigger libraries |
| Google Maps | Get Directions buttons |
| FormSubmit | Appointment request form delivery |
| Instagram/Facebook | Official social links |

## Appointment form — important activation step

Appointment requests are sent through FormSubmit to:

```text
zilolabeautyspa@gmail.com
```

FormSubmit normally sends a one-time activation email to this mailbox after the first real submission. The mailbox owner must open that email and confirm the form. Until this activation is completed, delivery may not work.

The form sends a **request**, not an automatic appointment confirmation. The salon must contact the customer by phone or email.

## Domain change

The website uses relative paths for all local assets, so no code change is required when moving to a new domain.

After domain setup, verify:

- `/` loads the complete Hero;
- `/menu.html` loads;
- `/gallery.html` loads;
- the Hero sequence scrolls smoothly;
- `Get Directions` opens Google Maps;
- Booking opens and submits after FormSubmit activation;
- Lookbook cards open on desktop and mobile;
- there are no browser console errors or missing images.

## Recommended acceptance checks

Test at minimum:

```text
Desktop: 1440 × 900
Mobile:  390 × 844
Mobile:  360 × 800
```

Also verify:

- HTTPS is active;
- no horizontal overflow appears;
- all 97 Hero frames return HTTP 200;
- the Lookbook media load;
- social links and telephone links work;
- the website root returns a non-empty HTML response.

---

# Guide de déploiement en français

## Netlify — méthode la plus simple

1. Décompresser le ZIP.
2. Se connecter à Netlify.
3. Créer un site avec l'option de déploiement manuel / glisser-déposer.
4. Déposer le dossier qui contient directement `index.html`.
5. Ouvrir l'URL Netlify générée et contrôler le site.
6. Ajouter le domaine final dans **Domain management**.
7. Appliquer les enregistrements DNS demandés par Netlify. Le HTTPS sera généré après validation du DNS.

Aucune commande de build n'est nécessaire. Le dossier de publication est la racine `.`

## Hébergement classique / cPanel

Déposer tout le contenu dans `public_html`, `www` ou `htdocs`. Le résultat doit être :

```text
public_html/index.html
public_html/assets/
```

Activer ensuite le domaine et le certificat HTTPS depuis l'hébergeur.

## Serveur Docker

```bash
docker compose up -d --build
```

Le site répondra sur le port `8080` :

```text
http://IP-DU-SERVEUR:8080
```

Brancher ensuite le domaine via un reverse proxy HTTPS.

## Points importants

- Le site est statique : aucun Node.js, PHP ou base de données n'est requis.
- Tous les médias nécessaires sont inclus dans `assets/`.
- Le dossier complet fait environ 122 MiB.
- Les polices, GSAP, Google Maps et FormSubmit nécessitent une connexion internet.
- La boîte `zilolabeautyspa@gmail.com` doit confirmer l'email d'activation FormSubmit lors de la première vraie demande.
- Aucun mot de passe, token ou credential n'est inclus dans ce pack.

## Support technique

Website created by **Clandestudio Agency**.

When requesting support, provide:

- deployment platform;
- final domain;
- browser/device affected;
- screenshot;
- browser console error if available.
