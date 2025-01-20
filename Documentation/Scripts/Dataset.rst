===================
MedAnalyzer Dataset
===================

.. attention:: Documentation technique du dataset MedAnalyzer

.. contents:: Table des matières
   :depth: 2
   :local:
   :backlinks: none

MedAnalyzer
-----------

.. note:: 
   Nous avons développé MedAnalyzer pour combler le manque de ressources structurées dans l'interprétation des analyses biologiques.

.. important::
   Notre dataset se distingue par sa structure question-réponse qui facilite l'apprentissage et l'intégration dans les systèmes d'aide à la décision médicale.

.. _huggingface: https://huggingface.co/datasets/ilyass20/MedAnalyzer

Interface Hugging Face
--------------------

.. figure:: /Documentation/Images/dataset.png
   :alt: Interface Hugging Face du Dataset MedAnalyzer
   :align: center
   :class: with-border

   Interface Hugging Face montrant le dataset MedAnalyzer

.. tip::
   Vous pouvez accéder à notre dataset sur `Hugging Face <https://huggingface.co/datasets/ilyass20/MedAnalyzer>`_

Nos Catégories d'Analyses
------------------------

.. admonition:: Points Clés
   :class: important

   Les catégories suivantes constituent le cœur de notre dataset.

1. Biochimie Sanguine
~~~~~~~~~~~~~~~~~~~~

.. admonition:: Focus
   :class: note

   Notre approche de la biochimie sanguine se concentre sur l'interprétation contextuelle des résultats.

**a) Électrolytes et équilibre hydro-électrolytique:**

.. warning::
   L'interprétation des électrolytes nécessite une attention particulière aux contextes cliniques.

* Na+/K+ avec analyse des gradients transmembranaires
* Calcium ionisé vs calcium total
* Magnésium et son impact sur la fonction neuromusculaire

**b) Fonction rénale approfondie:**

.. hint::
   Le DFG est un paramètre clé dans l'évaluation de la fonction rénale.

* Ratio urée/créatinine pour différencier les causes d'insuffisance rénale
* DFG avec les différentes formules de calcul (CKD-EPI, MDRD)
* Clearance de la créatinine mesurée vs estimée

2. Hématologie
~~~~~~~~~~~~~

.. attention::
   L'analyse hématologique requiert une approche systématique et rigoureuse.

**a) Analyse morphologique:**

* Classifications des anomalies érythrocytaires
* Signification des corps de Jolly et anneaux de Cabot
* Variations lymphocytaires réactionnelles vs pathologiques

Structure des Données
-------------------

.. code-block:: json
   :caption: Exemple de structure JSON
   :emphasize-lines: 2,6

    {
        "anchor": "Quelle est l'interprétation d'une hyperkaliémie à 6.2 mmol/L avec un ECG normal ?",
        "positive": "Dans ce contexte, plusieurs éléments sont à considérer :
                    1. Vérification pré-analytique (hémolyse, garrot prolongé)
                    2. Évaluation de la fonction rénale (créatinine, urée)
                    3. Recherche de médicaments hyperkaliémiants"
    }

Spécifications Techniques
------------------------

.. list-table::
   :header-rows: 1
   :widths: 30 70
   :class: config-table

   * - Paramètre
     - Valeur
   * - Format
     - JSON
   * - Taille
     - 1.03 MB
   * - Entrées
     - 2,182 paires questions-réponses
   * - Licence
     - Apache 2.0

Utilisation
----------

.. code-block:: python
   :linenos:
   :emphasize-lines: 2,5

    from datasets import load_dataset

    # Chargement du dataset
    dataset = load_dataset("ilyass20/MedAnalyzer")

    # Exemple d'utilisation
    for entry in dataset["train"]:
        print(f"Question : {entry['anchor']}")
        print(f"Réponse : {entry['positive']}")

.. caution::
   Assurez-vous d'avoir installé la bibliothèque datasets avant d'exécuter ce code.

Perspectives Futures
------------------

.. note::
   Nous prévoyons d'enrichir notre dataset avec :

   * Des cas cliniques complexes
   * Des variations géographiques des valeurs normales
   * Des algorithmes d'interprétation multicritères
   * Des corrélations avec l'imagerie médicale

Contact et Support
----------------

.. tip::
   Pour toute question ou suggestion concernant le dataset, vous pouvez nous contacter via la plateforme Hugging Face.
