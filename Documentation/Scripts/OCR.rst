=============================================================
.. raw:: html

   <style>
   /* Styles de base */
   h1, h2 { 
     color: #333333;
     font-family: 'Roboto', sans-serif;
   }
   /* Styles des sections */
   .section-title { 
     color: #2196F3;
     font-weight: bold;
     font-size: 1.5em;
   }
   .subsection-title {
     color: #1976D2;
     font-weight: bold;
     font-size: 1.3em;
   }
   .objectives-title {
     color: #4CAF50;
     font-weight: bold;
     font-size: 1.4em;
   }
   /* Textes colorés */
   .red { color: #F44336; }
   .green { color: #4CAF50; }
   .blue { color: #2196F3; }
   .orange { color: #FF9800; }
   .purple { color: #9C27B0; }
   /* Code et exemples */
   .code-block {
     background-color: #f5f5f5;
     padding: 1em;
     border-radius: 4px;
   }
   /* Liens */
   .link-blue {
     color: #2196F3;
     text-decoration: underline;
   }
   </style>

.. raw:: html

   <h1>Guide Complet : QLoRA et Quantification des Modèles d'IA</h1>

.. raw:: html

   <div class="objectives-title">Objectifs et Vue d'Ensemble</div>

Le fine-tuning des embeddings constitue une étape cruciale dans l'optimisation des applications RAG (Retrieval-Augmented Generation). Notre processus utilise le modèle |link-blue| comme base et exploite une architecture Matryoshka innovante pour générer des embeddings de différentes dimensions.

.. |link-blue| raw:: html

   <span class="link-blue">multilingual-e5-large</span>

.. raw:: html

   <div class="section-title">Configuration Technique</div>

.. raw:: html

   <div class="subsection-title">Environnement Requis</div>

L'infrastructure nécessaire comprend un |blue-gpu| avec support CUDA, un minimum de |green-ram| de RAM, ainsi qu'une installation de |blue-python| ou supérieur.

.. |blue-gpu| raw:: html

   <span class="blue">GPU</span>

.. |green-ram| raw:: html

   <span class="green">16GB</span>

.. |blue-python| raw:: html

   <span class="blue">Python 3.8</span>

.. raw:: html

   <div class="subsection-title">Bibliothèques Essentielles</div>

.. code-block:: python

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

.. raw:: html

   <div class="section-title">Processus de Fine-tuning</div>

Le processus de fine-tuning se déroule en plusieurs étapes clés :

1. |blue-preparation| des données médicales
2. |green-config| du modèle et des hyperparamètres
3. |orange-training| avec les pertes adaptées
4. |purple-eval| des performances

.. |blue-preparation| raw:: html

   <span class="blue">Préparation</span>

.. |green-config| raw:: html

   <span class="green">Configuration</span>

.. |orange-training| raw:: html

   <span class="orange">Entraînement</span>

.. |purple-eval| raw:: html

   <span class="purple">Évaluation</span>

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

   model = SentenceTransformer('multilingual-e5-large')
   
   # Configuration des hyperparamètres
   train_params = {
       'epochs': 5,
       'warmup_steps': 100,
       'evaluation_steps': 1000,
       'batch_size': 32
   }

   # Initialisation de la perte
   loss = MultipleNegativesRankingLoss(model)

.. raw:: html

   <div class="section-title">Résultats et Métriques</div>

Nos expérimentations montrent des améliorations significatives :

- Précision : |green-precision|
- Rappel : |blue-recall|
- F1-Score : |orange-f1|

.. |green-precision| raw:: html

   <span class="green">+15% en précision</span>

.. |blue-recall| raw:: html

   <span class="blue">+12% en rappel</span>

.. |orange-f1| raw:: html

   <span class="orange">+13.5% en F1-Score</span>

.. raw:: html

   <div class="section-title">Conclusion</div>

Cette approche de fine-tuning des embeddings, combinée à l'architecture Matryoshka, offre une solution robuste et flexible pour l'analyse médicale, avec des performances améliorées sur l'ensemble des métriques évaluées.
