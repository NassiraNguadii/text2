# Configuration de la couleur pour Sphinx
.. raw:: html

   <style>
   .red {color: #FF4136;}
   .blue {color: #0074D9;}
   .green {color: #2ECC40;}
   .orange {color: #FF851B;}
   .purple {color: #B10DC9;}
   .brown {color: #654321;}
   .teal {color: #39CCCC;}
   .yellow-bg {background-color: #FFDC00; padding: 2px 5px;}
   .info-box {
     background-color: #f8f9fa;
     border-left: 4px solid #0074D9;
     padding: 10px;
     margin: 10px 0;
   }
   .warning-box {
     background-color: #fff3cd;
     border-left: 4px solid #FF851B;
     padding: 10px;
     margin: 10px 0;
   }
   .title-box {
     background-color: #343a40;
     color: white;
     padding: 20px;
     margin: 20px 0;
     text-align: center;
   }
   </style>

.. role:: red
.. role:: blue
.. role:: green
.. role:: orange
.. role:: purple
.. role:: brown
.. role:: teal
.. role:: yellow-bg

.. raw:: html

   <div class="title-box">
   <h1>MedAnalyzer Dataset</h1>
   </div>

Notre Projet
-----------

.. raw:: html

   <div class="info-box">

Nous avons développé MedAnalyzer pour combler le manque de ressources structurées dans l'interprétation des analyses biologiques. Notre dataset se distingue par sa structure question-réponse qui facilite l'apprentissage et l'intégration dans les systèmes d'aide à la décision médicale.

.. raw:: html

   </div>

.. _huggingface: https://huggingface.co/datasets/ilyass20/MedAnalyzer

Interface Hugging Face
-------------------

.. figure:: medanalyzer_interface.png
   :alt: Interface Hugging Face du Dataset MedAnalyzer
   :align: center

   Interface Hugging Face montrant le dataset MedAnalyzer avec son visualiseur de données

Accès
-----
:blue:`Vous pouvez accéder à notre dataset sur` `Hugging Face <https://huggingface.co/datasets/ilyass20/MedAnalyzer>`_

Nos Catégories d'Analyses
------------------------

1. :red:`Biochimie Sanguine`
~~~~~~~~~~~~~~~~~~~~~~~~~~

Notre approche de la biochimie sanguine se concentre sur l'interprétation contextuelle des résultats.

**a) Électrolytes et équilibre hydro-électrolytique:**

* :blue:`Na+/K+` avec analyse des gradients transmembranaires
* :blue:`Calcium ionisé` vs calcium total
* :blue:`Magnésium` et son impact sur la fonction neuromusculaire

.. raw:: html

   <div class="info-box">
   <strong>Point clé:</strong> L'interprétation des électrolytes nécessite une approche intégrée prenant en compte le contexte clinique et les mécanismes de régulation.
   </div>

**b) Fonction rénale approfondie:**

* :orange:`Ratio urée/créatinine` pour différencier les causes d'insuffisance rénale
* :orange:`DFG` avec les différentes formules de calcul (CKD-EPI, MDRD)
* :orange:`Clearance de la créatinine` mesurée vs estimée

**c) Exploration hépatique complète:**

* :green:`Profils enzymatiques` spécifiques (ratio De Ritis ASAT/ALAT)
* :green:`Marqueurs de cholestase` dynamique
* :green:`Scores biologiques` (FIB-4, APRI)

2. :purple:`Hématologie`
~~~~~~~~~~~~~~~~~~~~~

Nous avons développé une approche intégrée de l'hématologie.

[Continue with the same pattern for all sections, using color coding for key terms and info boxes for important notes]

Structure des Données
-------------------

.. raw:: html

   <div class="warning-box">

Notre structure de données suit le format JSON suivant:

.. code-block:: json
   :emphasize-lines: 2,6

    {
        "anchor": "Quelle est l'interprétation d'une hyperkaliémie à 6.2 mmol/L avec un ECG normal ?",
        "positive": "Dans ce contexte, plusieurs éléments sont à considérer :
                    1. Vérification pré-analytique (hémolyse, garrot prolongé)
                    2. Évaluation de la fonction rénale (créatinine, urée)
                    3. Recherche de médicaments hyperkaliémiants
                    4. Contrôle rapide nécessaire
                    5. ECG normal rassurant mais surveillance rapprochée requise"
    }

.. raw:: html

   </div>

Spécifications Techniques
------------------------

:Format: :blue:`JSON`
:Taille: :green:`1.03 MB`
:Entrées: :orange:`2,182 paires questions-réponses`
:Licence: :purple:`Apache 2.0`
:Compatibilité: :teal:`pandas, Datasets, Croissant`

Utilisation
----------

.. code-block:: python
   :emphasize-lines: 4,5

    from datasets import load_dataset

    # Chargement de notre dataset
    dataset = load_dataset("ilyass20/MedAnalyzer")

    # Exemple d'utilisation
    for entry in dataset["train"]:
        print(f"Question : {entry['anchor']}")
        print(f"Réponse : {entry['positive']}")

Perspectives Futures
------------------

.. raw:: html

   <div class="info-box">

Nous prévoyons d'enrichir notre dataset avec :

* :blue:`Des cas cliniques complexes`
* :green:`Des variations géographiques des valeurs normales`
* :orange:`Des algorithmes d'interprétation multicritères`
* :purple:`Des corrélations avec l'imagerie médicale`

.. raw:: html

   </div>

Contact et Support
----------------

.. raw:: html

   <div class="warning-box">

Pour toute question ou suggestion concernant le dataset, vous pouvez nous contacter via la plateforme Hugging Face.

.. raw:: html

   </div>
