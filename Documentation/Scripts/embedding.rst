==================================================
Fine-tuning des Embeddings pour l'Analyse Médicale
==================================================

.. role:: red
   :class: red

.. role:: green
   :class: green

.. role:: blue
   :class: blue

.. role:: orange
   :class: orange

.. raw:: html

   <style>
   .red {color: red;}
   .green {color: green;}
   .blue {color: blue;}
   .orange {color: orange;}
   </style>

Introduction
-----------
Dans ce projet, nous avons développé un processus de fine-tuning des embeddings pour notre application RAG. Notre objectif était d'optimiser les performances du modèle ``multilingual-e5-large`` pour notre cas d'usage spécifique en médecine.

Notre Environnement de Travail
---------------------------
Pour réaliser ce projet, nous avons utilisé l'environnement suivant :

:blue:`Configuration Requise :`

- GPU avec support CUDA
- 16GB RAM minimum
- Python 3.8+

:green:`Bibliothèques Principales :`

.. code-block:: bash

    pip install torch
    pip install sentence-transformers
    pip install datasets
    pip install wandb

Notre Dataset
-----------
Nous avons utilisé notre dataset "MedAnalyzer", que nous avons publié sur Hugging Face. Ce dataset contient :

- :green:`2,182 paires` questions-réponses
- Contenu entièrement en français
- Focus sur la terminologie médicale

Notre Architecture
---------------
Nous avons implémenté une architecture Matryoshka innovante avec les dimensions suivantes :

:blue:`Dimensions des Embeddings :`
    - 1024 (Dimension complète)
    - 768
    - 512
    - 256
    - 128
    - 64 (Dimension minimale)

Notre Configuration d'Entraînement
------------------------------
:green:`Paramètres Optimisés :`

.. code-block:: python

    args = SentenceTransformerTrainingArguments(
        output_dir="bge-finetuned",
        num_train_epochs=1,
        per_device_train_batch_size=4,  # Optimisé pour notre GPU
        gradient_accumulation_steps=16,  # Pour un batch effectif de 64
        learning_rate=2e-5,             # Taux d'apprentissage optimal
        bf16=True                       # Précision mixte
    )

Notre Stratégie de Loss
^^^^^^^^^^^^^^^^^^^^
Nous avons utilisé une approche double :
- :blue:`Loss Principale :` MatryoshkaLoss
- :blue:`Loss Secondaire :` MultipleNegativesRankingLoss

Nos Résultats
-----------
:green:`Amélioration des Performances :`

+------------+------------------+------------------+----------------+
| Dimension  | Score Initial    | Score Final      | Gain          |
+============+==================+==================+================+
| 1024       | :blue:`0.7967`  | :green:`0.8484`  | +0.0517       |
+------------+------------------+------------------+----------------+
| 768        | :blue:`0.7981`  | :green:`0.8464`  | +0.0483       |
+------------+------------------+------------------+----------------+
| 512        | :blue:`0.7897`  | :green:`0.8471`  | +0.0574       |
+------------+------------------+------------------+----------------+
| 256        | :blue:`0.7522`  | :green:`0.8383`  | +0.0861       |
+------------+------------------+------------------+----------------+
| 128        | :blue:`0.6081`  | :green:`0.8253`  | :red:`+0.2172`|
+------------+------------------+------------------+----------------+
| 64         | :blue:`0.5182`  | :green:`0.7858`  | :red:`+0.2676`|
+------------+------------------+------------------+----------------+

:orange:`Points Clés de nos Résultats :`

1. Amélioration significative à toutes les dimensions
2. Gains exceptionnels sur les petites dimensions
3. Excellente performance maintenue en haute dimension

Utilisation de Notre Modèle
------------------------
Pour utiliser notre modèle fine-tuné :

.. code-block:: python

    from sentence_transformers import SentenceTransformer
    
    # Chargement de notre modèle
    model = SentenceTransformer('bge-finetuned')
    
    # Génération d'embeddings
    embeddings = model.encode(texts)

Nos Recommandations
----------------
En fonction de vos besoins :

:green:`Performance Maximale :`
    - Utiliser la dimension 1024
    - NDCG@10 : 0.8484
    - Recommandé pour les cas critiques

:blue:`Compromis Optimal :`
    - Utiliser la dimension 512
    - NDCG@10 : 0.8471
    - Bon équilibre performance/ressources

:orange:`Ressources Limitées :`
    - Utiliser la dimension 128
    - NDCG@10 : 0.8253
    - Excellent pour le déploiement mobile/edge

Limitations et Considérations
--------------------------
:red:`Points d'Attention :`

1. Ressources de Calcul :
    - Nécessite un GPU pour l'entraînement
    - RAM minimale de 16GB recommandée

2. Temps de Traitement :
    - Augmente avec la dimension des embeddings
    - Considérer le compromis vitesse/précision

3. Contraintes de Production :
    - Évaluer les besoins en stockage
    - Monitorer les temps de réponse

Conclusion
---------
Notre processus de fine-tuning a permis d'obtenir des améliorations significatives à toutes les dimensions, avec des gains particulièrement impressionnants pour les dimensions réduites. Notre approche offre une grande flexibilité d'utilisation, permettant de choisir la dimension optimale en fonction des contraintes spécifiques du projet.
