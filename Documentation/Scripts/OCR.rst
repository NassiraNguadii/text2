Benchmark OCR d'Analyses Sanguines
================================
.. raw:: html

Introduction
-----------
   <div class="section-title">Configuration Technique</div>

Dans le domaine médical, l'exactitude des données est primordiale. C'est dans cette optique que nous avons réalisé un benchmark approfondi de trois systèmes OCR sur nos analyses sanguines. Notre objectif était simple : déterminer quel système de reconnaissance de caractères offre la meilleure fiabilité pour la numérisation des résultats d'analyses médicales.
.. raw:: html

`Notebook Jupyter </Documentation/notebooks/benchmark__ocripynb.ipynb>`_
   <div class="subsection-title">Environnement Requis</div>

L'infrastructure nécessaire comprend un |blue-gpu| avec support CUDA, un minimum de |green-ram| de RAM, ainsi qu'une installation de |blue-python| ou supérieur.

Architecture du Benchmark
------------------------
.. |blue-gpu| raw:: html

.. image:: /Documentation/Images/image4.png
   :alt: Structure des fichiers
   <span class="blue">GPU</span>

Le benchmark repose sur une architecture binaire : un dossier 'ground_truth' contenant nos textes de référence, et un dossier 'images_analyse' regroupant nos scans à traiter. Cette approche nous permet de comparer directement les résultats OCR avec les données originales de nos 20 analyses sanguines.
.. |green-ram| raw:: html

Résultats de notre étude
------------------------
   <span class="green">16GB</span>

:blue:`Doctr : L'équilibriste`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. |blue-python| raw:: html

.. image:: /Documentation/Images/image1.png
   :alt: Résultats Doctr
   <span class="blue">Python 3.8</span>

Doctr se révèle être le système le plus équilibré de notre benchmark. Sur nos 20 analyses sanguines, il atteint une :green:`précision de 84%` et un temps de traitement de :blue:`24,75 secondes` par document. Son F1 Score de :green:`0,79` confirme sa constance dans le traitement de nos documents médicaux.
.. raw:: html
   <div class="subsection-title">Bibliothèques Essentielles</div>

.. code-block:: python
    :emphasize-lines: 1,2
    from doctr.io import DocumentFile
    from doctr.models import ocr_predictor
   import torch
   from sentence_transformers import SentenceTransformer
   from sentence_transformers.evaluation import (
       InformationRetrievalEvaluator,
       SequentialEvaluator
   )
   from sentence_transformers.losses import (
       MultipleNegativesRankingLoss,
       CosineSimilarityLoss
   )
   from transformers import AutoModel, AutoTokenizer
    model = ocr_predictor(pretrained=True)
    for i in range(1, 21):  # Pour nos 20 analyses
        image_path = f"images_analyse/{i}.jpg"
        doc = DocumentFile.from_images(image_path)
        result = model(doc)
.. raw:: html

:green:`EasyOCR : Le champion de la précision`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   <div class="section-title">Processus de Fine-tuning</div>

.. image:: /Documentation/Images/image2.png
   :alt: Résultats EasyOCR
Le processus de fine-tuning se déroule en plusieurs étapes clés :

EasyOCR excelle sur nos analyses sanguines avec :green:`87% de précision` et un F1 Score de :green:`0,83`. Son temps de traitement de :orange:`54,36 secondes` par document est plus long, mais sa précision sur nos paramètres biologiques est remarquable.
1. |blue-preparation| des données médicales
2. |green-config| du modèle et des hyperparamètres
3. |orange-training| avec les pertes adaptées
4. |purple-eval| des performances

.. code-block:: python
    :emphasize-lines: 1
.. |blue-preparation| raw:: html
   <span class="blue">Préparation</span>
.. |green-config| raw:: html
   <span class="green">Configuration</span>

    import easyocr
    import os
.. |orange-training| raw:: html

    reader = easyocr.Reader(['fr'])
    image_dir = 'images_analyse'
    for img in sorted(os.listdir(image_dir)):
        result = reader.readtext(os.path.join(image_dir, img))
   <span class="orange">Entraînement</span>

:red:`PaddleOCR : La rapidité au détriment de la précision`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. |purple-eval| raw:: html

.. image:: /Documentation/Images/image3.png
   :alt: Résultats PaddleOCR
   <span class="purple">Évaluation</span>

Sur nos analyses sanguines, PaddleOCR se montre décevant avec une :red:`précision de seulement 43%` et un F1 Score de :red:`0,32`, malgré sa rapidité de :green:`4,41 secondes` par document.
.. raw:: html
   <div class="section-title">Architecture Matryoshka</div>
L'architecture |green-matryoshka| permet de générer des embeddings imbriqués de différentes dimensions, offrant une flexibilité accrue pour différents cas d'usage.
.. |green-matryoshka| raw:: html
   <span class="green">Matryoshka</span>
Caractéristiques principales :
- Dimensions : |blue-dims|
- Performances : |green-perf|
- Adaptabilité : |orange-adapt|
.. |blue-dims| raw:: html
   <span class="blue">384, 512, et 768 dimensions</span>
.. |green-perf| raw:: html
   <span class="green">Haute précision à chaque niveau</span>
.. |orange-adapt| raw:: html
   <span class="orange">Adaptation dynamique selon les besoins</span>
.. raw:: html
   <div class="section-title">Exemples de Code</div>
Configuration du modèle :

.. code-block:: python
    :emphasize-lines: 1
    from paddleocr import PaddleOCR
    import glob
   model = SentenceTransformer('multilingual-e5-large')
   
   # Configuration des hyperparamètres
   train_params = {
       'epochs': 5,
       'warmup_steps': 100,
       'evaluation_steps': 1000,
       'batch_size': 32
   }
    ocr = PaddleOCR(use_angle_cls=True, lang='fr')
    for img_path in glob.glob('images_analyse/*.jpg'):
        result = ocr.ocr(img_path)
   # Initialisation de la perte
   loss = MultipleNegativesRankingLoss(model)
Implications pratiques
--------------------
.. raw:: html

Nos résultats sur ces 20 analyses sanguines orientent clairement les choix technologiques :
   <div class="section-title">Résultats et Métriques</div>

- Dans nos petites structures médicales, :green:`EasyOCR` représente une solution fiable. Sa précision supérieure sur nos paramètres biologiques compense largement son temps de traitement plus long.
Nos expérimentations montrent des améliorations significatives :

- Nos grands centres médicaux trouvent en :blue:`Doctr` un allié précieux. Sa combinaison de vitesse et de précision permet de traiter efficacement de grands volumes d'analyses tout en maintenant un niveau de fiabilité acceptable.
- Précision : |green-precision|
- Rappel : |blue-recall|
- F1-Score : |orange-f1|

- Quant à :red:`PaddleOCR`, nos tests montrent qu'il n'est pas adapté aux analyses sanguines, le risque d'erreur étant trop élevé.
.. |green-precision| raw:: html

Configuration matérielle
----------------------
   <span class="green">+15% en précision</span>

Pour nos tests, nous avons utilisé une configuration robuste qui s'est révélée nécessaire pour des performances optimales :
.. |blue-recall| raw:: html

- RAM : :blue:`16 GB minimum`
- GPU : :blue:`Carte graphique dédiée requise`
- OS : Linux/Windows/MacOS
   <span class="blue">+12% en rappel</span>

Notre choix final
---------------
.. |orange-f1| raw:: html

Après l'analyse approfondie de nos 20 documents d'analyses sanguines, :blue:`Doctr` s'impose comme le meilleur choix global. Cette décision s'appuie sur plusieurs facteurs clés :
   <span class="orange">+13.5% en F1-Score</span>

- :green:`Précision` : 84% (proche des 87% d'EasyOCR)
- :blue:`Temps de traitement` : 24,75 secondes (deux fois plus rapide qu'EasyOCR)
- :green:`F1 Score` : 0,79 (fiabilité constante)
.. raw:: html

En contexte réel d'analyses sanguines, cette combinaison de vitesse et de précision fait toute la différence. Le gain de temps significatif permet aux laboratoires de traiter plus d'analyses, tout en maintenant un niveau de précision largement suffisant pour un usage médical. La différence de précision de 3% avec EasyOCR est minime comparée au gain en efficacité opérationnelle.
   <div class="section-title">Conclusion</div>

Notre recommandation finale est donc claire : nous avons adopté :blue:`Doctr` pour la numérisation de nos analyses sanguines. Il représente le meilleur compromis entre précision et performance, permettant une numérisation efficace et fiable de nos données médicales. Pour nos laboratoires ayant des contraintes de temps strictes, Doctr est clairement la solution optimale pour le traitement de nos analyses sanguines.
Cette approche de fine-tuning des embeddings, combinée à l'architecture Matryoshka, offre une solution robuste et flexible pour l'analyse médicale, avec des performances améliorées sur l'ensemble des métriques évaluées.
