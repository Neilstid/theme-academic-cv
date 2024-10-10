---
title: 'Edition Multi-Pivots: Vers une Edition Multi-Directionnelle et Démêlée'

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

date: '2024-07-01'
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

abstract: Les réseaux antagonistes génératifs (GAN) permettent d’éditer des images en manipulant leurs caractéristiques. Cependant, ces manipulations ne sont pas toujours démêlées. Par exemple, lorsqu’une ride spécifique est modifiée, d’autres caractéristiques liées à l’âge sont souvent modifiées également. Cet article propose une nouvelle méthode d’édition démêlée. L’approche présentée est basée sur des images pivots qui permettent d’apprendre des directions d’édition pour une image d’entrée. Ces pivots sont basés sur une image réelle (l’entrée) et des modifications synthétiques de l’image réelle. Bien que notre principal cas d’applications d’édition soit les rides, notre méthode peut être étendue à d’autres tâches d’édition, telles que l’édition de la couleur des cheveux ou du rouge à lèvres. Les résultats qualitatifs et quantitatifs montrent que notre Edition Multi-Pivots (EMP) fournit un niveau plus élevé de démêlage et une édition plus réaliste que les méthodes de l’état de l’art.

# Summary. An optional shortened abstract.
summary: Cet article propose une nouvelle méthode d’édition démêlée. L’approche présentée est basée sur des images pivots qui permettent d’apprendre des directions d’édition pour une image d’entrée. Ces pivots sont basés sur une image réelle (l’entrée) et des modifications synthétiques de l’image réelle.

tags:
  - GAN
  - Disentangled Image Editing
  - Age Editing

# Display this page in the Featured widget?
featured: true

# Custom links (uncomment lines below)
# links:
# - name: Custom Link
#   url: http://example.org

url_pdf: 'https://hal.science/hal-04616405/document'
url_code: 'https://github.com/Neilstid/Edition-Multi-Pivots'
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

#### Création  des  PGT

<p>Pour guider le réseau dans le processus d'apprentissage de l'édition, des fausses cibles sont créées : les pseudo-vérités terrain (PGT). Ces PGT sont créés via des techniques de traitement d'image traditionnelles à partir de l'image d'entrée \(x\). Pour $k$ directions d'édition, les PGT sont désignées par \(y_1\), ..., \(y_k\) (étape 2.a). Le nombre de directions \(k\) est compris entre 1 et la dimensionnalité de l'espace latent, soit 512 pour StyleGAN.</p>

#### Création  des directions d'édition
Pour réaliser  l'édition, une direction d'édition est associée à chacune  des  {{< math >}}$$k$${{< /math >}} pseudo-vérités terrain (PGT). Ces directions sont  notées  {{< math >}}$$\overrightarrow{d_1}$${{< /math >}}, ..., {{< math >}}$$\overrightarrow{d_k}$${{< /math >}}, respectivement pour les  PGT  {{< math >}}$$y_1$${{< /math >}}, ..., {{< math >}}$$y_k$${{< /math >}}. Ces directions sont  obtenues via l'utilisation  des dimensions de  l'espace latent peu  utilisées pour la génération  des images (étape 2.b).

#### Apprentissage  des directions d'édition
L'image  inversée  {{< math >}}$$\hat{x}_{inv}$${{< /math >}} est souvent  floue par rapport à l'image  originale  et  l'identité est perdue. Pour résoudre  ce  problème, le  PTI  \cite{Roich_Mokady_Bermano_Cohen-Or_2023} finetune  le  générateur  {{< math >}}$$G$${{< /math >}}  de  sorte  que  l'image  générée  {{< math >}}$$\hat{x}_{inv}$${{< /math >}}  soit  similaire à l'image  réelle  {{< math >}}$$x$${{< /math >}}  \cite{Roich_Mokady_Bermano_Cohen-Or_2023}. Cette  opération  de  finetuning  crée  une  copie  de  l'image  d'entrée  dans  l'espace latent du  générateur  avec  les  poids  {{< math >}}$$\theta_{pt}$${{< /math >}}. Nous avons  poussé plus loin cette  idée  de  finetuning  du  générateur pour éditer  les  attributs  d'une image réelle  {{< math >}}$$x$${{< /math >}}. Une  fois  que  le  générateur  {{< math >}}$$G$${{< /math >}} est finetuné, il  peut  éditer  les  caractéristiques  souhaitées  sur  l'image  {{< math >}}$$x$${{< /math >}} en suivant  les directions d'éditions  {{< math >}}$$\overrightarrow{d_1}$${{< /math >}}, ..., {{< math >}}$$\overrightarrow{d_k}$${{< /math >}}. Plus de  détails  dans  \ref{sec:Generator tuning for local and disentangle edition}.

