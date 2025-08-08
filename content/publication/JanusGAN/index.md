---
title: 'JanusGAN: GANs Disentangled Editing with Two Discriminators'

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

date: '2025-05-30'
doi: '10.1109/FG61629.2025.11099171'

# Schedule page publish date (NOT publication's date).
publishDate: '2025-08-06'

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
publication_types: ['paper-conference']

# Publication name and optional abbreviated publication name.
publication: In *IEEE International Conference on Automatic Face and Gesture Recognition*
publication_short: In *FG*

abstract: <p align=justify>Generative Adversarial Networks (GANs) have found applications in image editing. However, GANs tend to synthesize and manipulate global features, such as age, rather than focusing on local features such as facial wrinkles. Consequently, when a specific wrinkle is edited, all age-related features change as well. This paper proposes a new method that allows a GAN to learn a specific global or local disentangled edit. The method involves fine-tuning a pre-trained GAN using two discriminators, each trained on a specific dataset representing distinct states of a single disentangled feature. This approach facilitates the generator’s ability to learn features along a defined editing direction within the latent space. Importantly, to avoid interfering with prior GAN knowledge, the editing direction is defined in the meaningless dimension of the GAN latent space. Although our primary focus is on local editing, our method can be extended to global features, such as age editing. Quantitative and qualitative results show that our method provides a better balance between feature accuracy and disentanglement than other state-of-the-art methods for both local and global features. </p>

# Summary. An optional shortened abstract.
summary: This paper proposes a new method for disentangled editing. The presented approach is proposed to finetune a GAN with two discriminators. Each discriminator is getting specialize on a specific state of a feature. For instance, for wrinkle editing one state is with wrinkle and the other state is without wrinkle. Moreover in order to preserve most of original synthesizing capacities of the GAN, we propose to create an editing direction in the "meaningless" dimensions of the latent of the GAN.

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

url_pdf: ''
url_code: 'https://github.com/Neilstid/JanusGAN'
url_dataset: ''
url_poster: 'https://github.com/Neilstid/theme-academic-cv/static/uploads/poster_JanusGAN.pdf'
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

