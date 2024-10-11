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

abstract: <p align=justify>Les réseaux antagonistes génératifs (GAN) permettent d’éditer des images en manipulant leurs caractéristiques. Cependant, ces manipulations ne sont pas toujours démêlées. Par exemple, lorsqu’une ride spécifique est modifiée, d’autres caractéristiques liées à l’âge sont souvent modifiées également. Cet article propose une nouvelle méthode d’édition démêlée. L’approche présentée est basée sur des images pivots qui permettent d’apprendre des directions d’édition pour une image d’entrée. Ces pivots sont basés sur une image réelle (l’entrée) et des modifications synthétiques de l’image réelle. Bien que notre principal cas d’applications d’édition soit les rides, notre méthode peut être étendue à d’autres tâches d’édition, telles que l’édition de la couleur des cheveux ou du rouge à lèvres. Les résultats qualitatifs et quantitatifs montrent que notre Edition Multi-Pivots (EMP) fournit un niveau plus élevé de démêlage et une édition plus réaliste que les méthodes de l’état de l’art.</p>

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

![Architecture Multi-PTE](./architecture.png)

<p align=justify>Pour guider le réseau dans le processus d'apprentissage de l'édition, des fausses cibles sont créées : les pseudo-vérités terrain (PGT). Ces PGT sont créés via des techniques de traitement d'image traditionnelles à partir de l'image d'entrée \(x\). Pour $k$ directions d'édition, les PGT sont désignées par \(y_1\), ..., \(y_k\) (étape 2.a). Le nombre de directions \(k\) est compris entre 1 et la dimensionnalité de l'espace latent, soit 512 pour StyleGAN.</p>

#### Création  des directions d'édition

<p align=justify>Pour réaliser l'édition, une  direction d'édition est associée à chacune des \(k\) pseudo-vérités terrain (PGT). Ces directions sont notées \(\overrightarrow{d_1}\), ..., \(\overrightarrow{d_k}\), respectivement pour les PGT \(y_1\), ..., \(y_k\). Ces directions sont obtenues via l'utilisation des dimensions de l'espace latent peu utilisées pour la génération des images (étape 2.b).</p>

#### Apprentissage  des directions d'édition

<p align=justify>L'image inversée \(\hat{x}_{inv}\) est souvent floue par rapport à l'image originale et l'identité est perdue. Pour résoudre ce problème, le PTI finetune le générateur \(G\) de sorte que l'image générée \(\hat{x}_{inv}\) soit similaire à l'image réelle \(x\). Cette opération de finetuning crée une copie de l'image d'entrée dans l'espace latent du générateur avec les poids \(\theta_{pt}\). Nous avons poussé plus loin cette idée de finetuning du générateur pour éditer les attributs d'une image réelle \(x\). Une fois que le générateur \(G\) est finetuné, il peut éditer les caractéristiques souhaitées sur l'image \(x\) en suivant les directions d'éditions \(\overrightarrow{d_1}\), ..., \(\overrightarrow{d_k}\).</p>

### Resultat

#### Resultat Quantitatif

| Metrics                                     |    IC    |  LD_o  |   MSE_o  |     SSIM    |  FID |   KID  |
|---------------------------------------------|:------------------:|:----------------:|:------------------:|:-------------------:|:----------------:|:------------------:|
| Resefa [^2]    |        0.32        |       $154$      |       $0.89$       |       $0.859$       |       $89$       |       $1.70$       |
| StyleMapGAN [^3] |       0.27       |   **29**  |        1.43        |        0.963        |        119       |       $2.37$       |
| SOAT [^4]          |   **0.08**  | *51* |   **0.09**  |   **0.995**  | *76* | *0.87* |
| EMP                                         | *0.11* |       67     | *0.21* | *0.969* |   **31**  |   **0.31**  |

#### Resultat Qualitatif

<style>
  div.container {
    width: 800px;
    height: 530px;
    position: relative;
    margin: 20px;
  }

  div.image {
    height: 100%;
    background-repeat: no-repeat;
    background-position: top left;
    background-size: cover;
    position: absolute;
    top: 0px;
    left: 0px;
  }

  div.before {
    width: 50%;
    z-index: 2;
  }

  div.after {
    width: 100%;
    z-index: 1;
  }

  input.slider {
    width: 100%;
    height: 100%;
    outline: none;
    background-color: transparent;
    position: absolute;
    margin: 0px;
    z-index: 3;
    cursor: pointer;
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
    transition: 0.25s all ease-in-out;
    -moz-transition: 0.25s all ease-in-out;
    -webkit-transition: 0.25s all ease-in-out;
    z-index: 4;
  }

  input.slider::-moz-range-thumb {
    width: 6px;
    height: 600px;
    background-color: white;
    cursor: pointer;
  }

  input.slider::-webkit-slider-thumb {
    width: 6px;
    height: 530px;
    background-color: white;
    cursor: pointer;
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;
  }


  div.slider-button {
    width: 30px;
    height: 30px;
    border-radius: 50%;
    -moz-broder-radius: 50%;
    -webkit-border-radius: 50%;
    background-color: white;
    position: absolute;
    top: calc(50% - 18px);
    left: calc(50% - 18px);
    cursor: pointer;
    z-index: 3;
  }

  div.slider-button:before {
    color: #555;
    position: absolute;
    top: 3px;
    left: 0px;
    content: "\2B9C";
  }

  div.slider-button:after {
    color: #555;
    position: absolute;
    top: 3px;
    right: 0px;
    content: "\2B9E";
  }
</style>

<script>
  $("input.slider").on("input change", function(event) {
    var element = $(this).parents("div.container");
    var pos = event.target.value;
    
    element.find("div.before").css({width: pos + "%"});
    element.find("div.slider-button").css({left: "calc(" + pos + "% - 18px)"});
  });
</script>


<div class="container">
	<div class="image before" style="background-image:url('./before.jpg');"></div>
	<div class="image after" style="background-image:url('./after.jpg');"></div>
	
	<input type="range" class="slider" min="1" max="100" value="50" />
	<div class="slider-button"></div>
</div>

### Reference

[^1]: D. Roich, R. Mokady, A. H. Bermano, and D. CohenOr, “Pivotal tuning for latent-based editing of real images,” ACM Transactions on Graphics (TOG), vol. 42, p. 1–13, Feb 2023.

[^2]: J. Zhu, Y. Shen, Y. Xu, D. Zhao, and Q. Chen, “Region-based semantic factorization in GANs,” in International Conference on Machine Learning (ICML), 2022.

[^3]: H. Kim, Y. Choi, J. Kim, S. Yoo, and Y. Uh, Exploiting Spatial Dimensions of Latent in GAN for Realtime Image Editing, p. 852–861. IEEE, Jun 2021.

[^4]: M. J. Chong, H.-Y. Lee, and D. Forsyth, “Stylegan of all trades : Image manipulation with only pretrained stylegan,” arXiv, 2021.