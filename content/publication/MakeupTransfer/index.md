---
title: "Le transfert de fond de teint n\'est pas qu\'une copie de couleur"

# Authors
# If you created a profile for a user (e.g. the default `admin` user), write the username (folder name) here
# and it will be replaced with their full name and linked to their profile.
authors:
  - admin
  - Catherine Soladié
  - Gabriel Cazorla
  - Renaud Séguier

# Author notes (optional)
# author_notes:
#   - 'Equal contribution'
#   - 'Equal contribution'

date: '2024-07-02'
doi: ''

# Schedule page publish date (NOT publication's date).
publishDate: '2024-06-19'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *Reconnaissance des Formes, Image, Apprentissage et Perception*
publication_short: In *RFIAP*

abstract: Le transfert de maquillage vise à appliquer un maquillage donné (extrait d’un visage appelé référence) sur un visage non maquillé (source) tout en préservant les attributs d’identité de la source. Les méthodes récentes tendent à considérer le transfert de maquillage comme une copie de la couleur. Cependant, cette hypothèse conduit à appliquer un maquillage esthétique sur une image réaliste au lieu de générer un maquillage réaliste. Par conséquent, même si le maquillage généré peut sembler réaliste, il n’est pas conforme à la réalité. Notre approche vise à préserver les informations relatives au teint du visage source. De plus, pour éviter que notre modèle ne mélange les processus de maquillage et de démaquillage, ces processus sont séparés. Les résultats quantitatifs montrent que nous sommes plus performants que les architectures de transfert de maquillage de l’état de l’art, tant pour la précision du fond de teint que pour sa cohérence.

# Summary. An optional shortened abstract.
summary: Notre approche vise à préserver les informations relatives au teint du visage source. De plus, pour éviter que notre modèle ne mélange les processus de maquillage et de démaquillage, ces processus sont séparés.

tags:
  - GAN
  - Makeup Transfer

# Display this page in the Featured widget?
featured: false

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://hal.science/hal-04615787/document'
url_code: 'https://github.com/Neilstid/Transfert-Maquillage'
url_dataset: ''
url_poster: ''
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
image:
  caption: 'Image credit:'
  focal_point: ''
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
#   - example

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
---

### Méthode

<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/katex.min.js" integrity="sha384-y23I5Q6l+B6vatafAwxRu/0oK/79VlbSz7Q9aiSZUvyWYIYsd+qj+o24G5ZU2zJz" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.11.1/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous" onload="renderMathInElement(document.body);"></script>

![Architecture Multi-PTE](./architecture.png)

<p align=justify>L'architecture du modèle consiste en deux étapes d'apprentissage distinctes : l'apprentissage de l'encodeur/décodeur et l'apprentissage auto-supervisé du générateur \(\alpha\). 
L'apprentissage de l'encodeur/décodeur consiste à :</p>
- Extraire les caractéristiques avec un encodeur \(E\), prenant en entrée une source $x$ et une référence \(y\).
- Générer des images avec un décodeur pour le maquillage et le démaquillage (notés respectivement \(D_m\) et \(D_{mr}\)).
<p align=justify>
En plus des fonctions de perte utilisées par EleGANt, nous avons ajouté la fonction de perte Makeup Free Area Loss, qui assure la cohérence des zones sans maquillage (comme l'arrière-plan). </p>
