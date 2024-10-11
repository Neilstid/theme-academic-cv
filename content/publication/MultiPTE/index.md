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
  /* Reset some default styles and set up the background color */
  *,
  *::after,
  *::before {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
  }

  body {
      background-color: #f0f0f0;
  }

  /* Container styles */
  .container {
      position: relative;
      overflow: hidden;
      /* Set initial position for the slider handle (50% in this example) */
      --position: 50%;
  }

  /* Image container */
  .image-container {
      aspect-ratio: 16/9; /* Set your desired aspect ratio */
      /* Style images within the container */
      img {
          width: 100%;
          height: 100%;
          object-fit: cover;
      }
  }

  /* Slider styles */
  .slider {
      position: absolute;
      width: 100%;
      height: 100%;
      /* Hide slider input visually */
      opacity: 0;
  }

  .slider-line {
      /* Style the slider line */
  }

  .slider-button {
      /* Style the slider handle */
  }
</style>

<script>
  function initializeSlider() {
    const slider = document.querySelector('.slider');
    const container = document.querySelector('.container');
    slider.addEventListener('input', function (e) {
      container.style.setProperty('--position', e.target.value + '%');
    });
  }
  function loadImages() {
    const file1 = document.querySelector('#imageUpload1').files[0];
    const file2 = document.querySelector('#imageUpload2').files[0];
    if (file1 && file2 && file1.type.startsWith('image/') && file2.type.startsWith('image/')) {
      const imageBefore = document.querySelector('.image-before');
      const imageAfter = document.querySelector('.image-after');
      imageBefore.src = URL.createObjectURL(file1);
      imageAfter.src = URL.createObjectURL(file2);
    } else {
      alert('Please select valid image files.');
    }
  }
  function setAspectRatio(imageNumber) {
    const imageElement = document.querySelector(`.image-${imageNumber}`);
    const aspectRatio = imageElement.naturalWidth / imageElement.naturalHeight;
    const aspectRatioString = aspectRatio.toString();
    document.querySelector('.image-container').style.setProperty('--aspect-ratio', aspectRatioString);
  }
  function selectImage(imageNumber) {
    const inputElement = document.querySelector(`#imageUpload${imageNumber}`);
    const file = inputElement.files[0];
    if (file && file.type.startsWith('image/')) {
      if (imageNumber === 1) {
        // Update file1 and file1Selected
      } else {
        // Update file2 and file2Selected
      }
      if (file1Selected && file2Selected) {
        // Enable the "Show Difference" button
      }
    } else {
      alert('Please select valid image files.');
    }
  }
</script>

<!-- Container for the slider -->
<div class="container">
    <!-- Images to compare -->
    <div class="image-container">
        <img class="image-before slider-image image-1" src="before.png" alt="Before Image">
        <img class="image-after slider-image image-2" src="after.png" alt="After Image">
    </div>
    <!-- Slider input element -->
    <input type="range" min="0" max="100" value="50" aria-label="Percentage of before photo shown" class="slider">
    <!-- Slider line and handle -->
    <div class="slider-line" aria-hidden="true"></div>
    <div class="slider-button" aria-hidden="true">
        <!-- Slider handle icon (e.g., a draggable circle or button) -->
    </div>
</div>


### Reference

[^1]: D. Roich, R. Mokady, A. H. Bermano, and D. CohenOr, “Pivotal tuning for latent-based editing of real images,” ACM Transactions on Graphics (TOG), vol. 42, p. 1–13, Feb 2023.

[^2]: J. Zhu, Y. Shen, Y. Xu, D. Zhao, and Q. Chen, “Region-based semantic factorization in GANs,” in International Conference on Machine Learning (ICML), 2022.

[^3]: H. Kim, Y. Choi, J. Kim, S. Yoo, and Y. Uh, Exploiting Spatial Dimensions of Latent in GAN for Realtime Image Editing, p. 852–861. IEEE, Jun 2021.

[^4]: M. J. Chong, H.-Y. Lee, and D. Forsyth, “Stylegan of all trades : Image manipulation with only pretrained stylegan,” arXiv, 2021.