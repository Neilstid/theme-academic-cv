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

<p align=justify>Pour guider le réseau dans le processus d'apprentissage de l'édition, des fausses cibles sont créées : les pseudo-vérités terrain (PGT). Ces PGT sont créés via des techniques de traitement d'image traditionnelles à partir de l'image d'entrée \(x\). Pour $k$ directions d'édition, les PGT sont désignées par \(y_1\), ..., \(y_k\) (étape 2.a). Le nombre de directions \(k\) est compris entre 1 et la dimensionnalité de l'espace latent, soit 512 pour StyleGAN.</p>

#### Création  des directions d'édition

<p align=justify>Pour réaliser l'édition, une  direction d'édition est associée à chacune des \(k\) pseudo-vérités terrain (PGT). Ces directions sont notées \(\overrightarrow{d_1}\), ..., \(\overrightarrow{d_k}\), respectivement pour les PGT \(y_1\), ..., \(y_k\). Ces directions sont obtenues via l'utilisation des dimensions de l'espace latent peu utilisées pour la génération des images (étape 2.b).</p>

#### Apprentissage  des directions d'édition

<p align=justify>L'image inversée \(\hat{x}_{inv}\) est souvent floue par rapport à l'image originale et l'identité est perdue. Pour résoudre ce problème, le PTI \cite{Roich_Mokady_Bermano_Cohen-Or_2023} finetune le générateur \(G\) de sorte que l'image générée \(\hat{x}_{inv}\) soit similaire à l'image réelle \(x\) \cite{Roich_Mokady_Bermano_Cohen-Or_2023}. Cette opération de finetuning crée une copie de l'image d'entrée dans l'espace latent du générateur avec les poids \(\theta_{pt}\). Nous avons poussé plus loin cette idée de finetuning du générateur pour éditer les attributs d'une image réelle \(x\). Une fois que le générateur \(G\) est finetuné, il peut éditer les caractéristiques souhaitées sur l'image \(x\) en suivant les directions d'éditions \(\overrightarrow{d_1}\), ..., \(\overrightarrow{d_k}\).</p>

### Result

{{< math >}}
$$
\begin{table*}[ht!]
    \centering
    \begin{tabular}{ c | l | c c c | c c c } % | r r r
      \toprule
      % & \multicolumn{3}{c|}{Real image datasets (FFHQ and CelebA)} & \multicolumn{3}{c}{Generated images} \\
      % \hline
      & & \multicolumn{3}{c |}{\scriptsize{Identity preservation}} & \multicolumn{3}{c}{\scriptsize{Image quality}} \\
      \midrule
      & Metrics & $IC\downarrow$ & $LD_o\downarrow$ & $MSE_o\downarrow$ & $SSIM\uparrow$ & $FID\downarrow$ & $KID\downarrow$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{4}{*}{\rotatebox[origin=c]{90}{\scriptsize{Lion}}}} & Resefa \cite{Zhu_Shen_Xu_Zhao_Chen_2022} & $0.32$ & $154$ & $0.89$ & $0.859$ & $89$ & $1.70$ \\
      & StyleMapGAN \cite{Kim_Choi_Kim_Yoo_Uh_2021} & $0.27$ & $\textbf{29}$ & $1.43$ & $0.963$ & $119$ & $2.37$ \\
      & SOAT \cite{Chong_Lee_Forsyth_2021} & $\textbf{0.08}$ & $\underline{51}$ & $\textbf{0.09}$ & $\textbf{0.995}$ & $\underline{76}$ & $\underline{0.87}$ \\
      & EMP & $\underline{0.11}$ & $67$ & $\underline{0.21}$ & $\underline{0.969}$ & $\textbf{31}$ & $\textbf{0.31}$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{4}{*}{\rotatebox[origin=c]{90}{\scriptsize{Sous les Yeux}}}} & Resefa \cite{Zhu_Shen_Xu_Zhao_Chen_2022} & $0.30$ & $208$ & $0.93$ & $0.840$ & $94$ & $1.68$ \\
      & StyleMapGAN \cite{Kim_Choi_Kim_Yoo_Uh_2021} & $0.29$ & $\textbf{44}$ & $1.68$ & $0.946$ & $144$ & $4.24$ \\
      & SOAT \cite{Chong_Lee_Forsyth_2021} & $\underline{0.14}$ & $97$ & $\underline{0.23}$ & $\textbf{0.983}$ & $\underline{80}$ & $\underline{0.41}$ \\
      & EMP & $\textbf{0.10}$ & $\underline{92}$ & $\textbf{0.20}$ & $\underline{0.972}$ & $\textbf{31}$ & $\textbf{0.28}$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{4}{*}{\rotatebox[origin=c]{90}{\scriptsize{Marionette}}}} & Resefa \cite{Zhu_Shen_Xu_Zhao_Chen_2022} & $0.36$ & $213$ & $0.96$ & $0.841$ & $91$ & $1.16$ \\
      & StyleMapGAN \cite{Kim_Choi_Kim_Yoo_Uh_2021} & $0.24$ & $\textbf{42}$ & $2.17$ & $0.926$ & $126$ & $2.55$ \\
      & SOAT \cite{Chong_Lee_Forsyth_2021} & $\underline{0.14}$ & $116$ & $\underline{0.34}$ & $\textbf{0.974}$ & $\underline{82}$ & $\underline{0.94}$ \\
      & EMP & $\textbf{0.09}$ & $\underline{91}$ & $\textbf{0.22}$ & $\underline{0.967}$ & $\textbf{33}$ & $\textbf{0.42}$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{4}{*}{\rotatebox[origin=c]{90}{\scriptsize{Nasogénien}}}} & Resefa \cite{Zhu_Shen_Xu_Zhao_Chen_2022} & $0.25$ & $150$ & $1.11$ & $0.842$ & $104$ & $1.29$ \\
      & StyleMapGAN \cite{Kim_Choi_Kim_Yoo_Uh_2021} & $0.30$ & $\textbf{54}$ & $2.02$ & $0.931$ & $149$ & $3.64$ \\
      & SOAT \cite{Chong_Lee_Forsyth_2021} & $\underline{0.15}$ & $117$ & $\underline{0.24}$ & $\textbf{0.985}$ & $\underline{93}$ & $\underline{0.50}$ \\
      & EMP & $\textbf{0.10}$ & $\underline{85}$ & $\textbf{0.10}$ & $\underline{0.972}$ & $\textbf{36}$ & $\textbf{0.10}$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{5}{*}{\rotatebox[origin=c]{90}{\scriptsize{Lèvre}}}} & Resefa \cite{Zhu_Shen_Xu_Zhao_Chen_2022} & $0.48$ & $129$ & $0.94$ & $0.842$ & $\underline{66}$ & $\underline{0.51}$ \\
      & StyleMapGAN \cite{Kim_Choi_Kim_Yoo_Uh_2021} & $0.45$ & $\underline{66}$ & $ 2.00$ & $0.940$ & $95$ & $2.48$ \\
      & StyleClip \cite{Patashnik_Wu_Shechtman_Cohen-Or_Lischinski_2021} & $\underline{0.16}$ & $68$ & $\textbf{0.29}$ & $\textbf{0.975}$ & $69$ & $0.90$ \\
      & FEAT \cite{Hou_Shen_Patashnik_Cohen-Or_Huang_2022} & $0.23$ & $103$ & $1.37$ & $0.724$ & $77$ & $1.71$ \\
      & EMP & $\textbf{0.15}$ & $\textbf{65}$ & $\underline{0.34}$ & $\underline{0.965}$ & $\textbf{28}$ & $\textbf{0.17}$ \\
      \midrule
      
      \parbox[t]{2mm}{\multirow{6}{*}{\rotatebox[origin=c]{90}{\scriptsize{Cheveux}}}} & StyleGANEX \cite{Yang_Jiang_Liu_Loy_2023} & $0.54$ & $144$ & $1.97$ & $0.579$ & $70$ & $2.59$ \\
      & VecGAN++ \cite{Dalva_Pehlivan_Hatipoglu_Moran_Dundar_2023} & $0.29$ & $\textbf{27}$ & $2.73$ & $\textbf{0.781}$ & $73$ & $2.94$ \\
      & StyleClip \cite{Patashnik_Wu_Shechtman_Cohen-Or_Lischinski_2021} & $0.30$ & $102$ & $\underline{1.04}$ & $0.584$ & $77$ & $1.59$ \\
      & FEAT \cite{Hou_Shen_Patashnik_Cohen-Or_Huang_2022} & $0.36$ & $105
      $ & $1.10$ & $0.674$ & $\underline{61}$ & $\underline{0.37}$ \\
      & CtrlHair \cite{Guo_Kan_Chen_Shan_2022} & $\underline{0.28}$ & $\textbf{27}$ & $3.61$ & $0.548$ & $63$ & $1.21$ \\
      & EMP & $\textbf{0.21}$ & $87$ & $\textbf{0.61}$ & $\underline{0.718}$ & $\textbf{41}$ & $\textbf{0.11}$ \\
      \bottomrule
    \end{tabular}
    \caption{Comparaison de la préservation de l'identité et de la qualité de l'image pour les méthodes de l'état de l'art et notre EMP. Les résultats MSE et KID sont arrondis à $1e^{-2}$. Chaque méthode est affectée à ses tâches disponibles.}
    \label{table:Quantitative}
\end{table*}
$$
{{< /math >}}

### Reference