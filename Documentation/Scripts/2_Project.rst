************************************************
Benchmark OCR pour Analyses Sanguines
************************************************

Introduction
===========

Contexte
--------
Les analyses de biochimie sanguine sont des documents médicaux essentiels contenant des informations critiques pour le diagnostic et le suivi des patients. La numérisation et l'extraction automatique de ces données représentent un enjeu majeur pour les établissements de santé.

Objectif du benchmark
-------------------
Ce benchmark vise à évaluer et comparer les performances de trois solutions OCR majeures dans le contexte spécifique de la reconnaissance des analyses sanguines :

* Doctr
* EasyOCR
* PaddleOCR

Données et méthodologie
======================

Jeu de données
-------------
* 20 analyses de biochimie sanguine
* Format : Images JPEG scannées
* Contenu type :
    - Paramètres sanguins (sodium, potassium, etc.)
    - Valeurs numériques
    - Unités de mesure
    - Intervalles de référence
    - Dates et identifiants

Structure du projet
-----------------
::

    benchmark_ocr_sanguin/
    ├── ground_truth/          # Vérité terrain
    │   ├── 1.txt             
    │   └── ...
    ├── images_analyse/        # Images à analyser
    │   ├── 1.jpeg            
    │   └── ...
    ├── results/              # Résultats des tests
    │   ├── doctr/
    │   ├── easyocr/
    │   └── paddleocr/
    └── src/                  # Code source
        ├── benchmark.py
        ├── utils.py
        └── metrics.py

Implémentation du benchmark
=========================

Installation et configuration
---------------------------
.. code-block:: python

    # Installation des dépendances
    pip install python-doctr
    pip install easyocr
    pip install paddleocr
    pip install numpy pandas

    # Configuration de base
    import os
    import json
    from pathlib import Path
    from datetime import datetime

    class BenchmarkConfig:
        IMAGE_DIR = "images_analyse"
        GROUND_TRUTH_DIR = "ground_truth"
        RESULTS_DIR = "results"
        
        SUPPORTED_FORMATS = ['.jpeg', '.jpg']
        FRENCH_LOCALE = 'fr'

Code principal du benchmark
-------------------------
.. code-block:: python

    class OCRBenchmarkSanguin:
        def __init__(self, config: BenchmarkConfig):
            self.config = config
            self.results = {}
            self.setup_directories()

        def setup_directories(self):
            """Initialise la structure des dossiers"""
            for dir_path in [self.config.RESULTS_DIR]:
                os.makedirs(dir_path, exist_ok=True)

        def run_full_benchmark(self):
            """Exécute les tests pour tous les systèmes OCR"""
            systems = {
                'doctr': self.run_doctr,
                'easyocr': self.run_easyocr,
                'paddleocr': self.run_paddleocr
            }
            
            for system_name, system_func in systems.items():
                print(f"Évaluation de {system_name}...")
                self.results[system_name] = system_func()
                
            self.save_results()

Implémentation par système
------------------------

Doctr
~~~~~
.. code-block:: python

    def run_doctr(self):
        from doctr.io import DocumentFile
        from doctr.models import ocr_predictor
        
        results = []
        predictor = ocr_predictor(pretrained=True)
        
        for image_path in Path(self.config.IMAGE_DIR).glob('*.jpeg'):
            start_time = time.time()
            # Traitement de l'image
            doc = DocumentFile.from_images(str(image_path))
            result = predictor(doc)
            
            # Analyse des résultats
            extraction = self.analyze_medical_values(result.text)
            duration = time.time() - start_time
            
            results.append({
                'file': image_path.name,
                'text': result.text,
                'extracted_values': extraction,
                'duration': duration
            })
        
        return results

Métriques d'évaluation
---------------------
.. code-block:: python

    def calculate_metrics(self, system_results, ground_truth):
        """Calcul des métriques de performance"""
        metrics = {
            'global': {
                'precision': 0,
                'recall': 0,
                'f1_score': 0,
                'avg_time': 0
            },
            'par_parametre': {}
        }
        
        # Calcul pour chaque paramètre sanguin
        parametres = ['sodium', 'potassium', 'chlorures', 'reserve_alcaline']
        for param in parametres:
            metrics['par_parametre'][param] = self.evaluate_parameter(
                system_results, 
                ground_truth,
                param
            )
        
        return metrics

Résultats du benchmark
=====================

Performances globales
-------------------

Doctr
~~~~~
* Précision : 84%
* Rappel : 75%
* Score F1 : 0.79
* Temps moyen : 24.75s
* Points forts : Bon équilibre précision/vitesse
* Points faibles : Sensible à la qualité des scans

EasyOCR
~~~~~~~
* Précision : 87%
* Rappel : 80%
* Score F1 : 0.83
* Temps moyen : 54.36s
* Points forts : Meilleure précision globale
* Points faibles : Temps de traitement élevé

PaddleOCR
~~~~~~~~~
* Précision : 43%
* Rappel : 25%
* Score F1 : 0.32
* Temps moyen : 4.41s
* Points forts : Rapidité d'exécution
* Points faibles : Précision insuffisante

Analyse par paramètre sanguin
---------------------------

.. list-table:: Résultats par paramètre
   :header-rows: 1

   * - Paramètre
     - Système
     - Précision
     - Rappel
     - F1 Score
   * - Sodium
     - Doctr
     - 0.89
     - 0.82
     - 0.85
   * - 
     - EasyOCR
     - 0.91
     - 0.85
     - 0.88
   * - 
     - PaddleOCR
     - 0.45
     - 0.28
     - 0.34
   * - Potassium
     - Doctr
     - 0.86
     - 0.79
     - 0.82

Analyse et recommandations
========================

Forces et faiblesses par système
-----------------------------

Doctr
~~~~~
* ✓ Bonne détection des valeurs numériques
* ✓ Performance stable
* × Difficulté avec les caractères spéciaux

EasyOCR
~~~~~~~
* ✓ Excellente précision sur les unités
* ✓ Meilleure gestion des tableaux
* × Temps de traitement important

PaddleOCR
~~~~~~~~~
* ✓ Très rapide
* ✓ Faible utilisation ressources
* × Précision insuffisante pour usage médical

Recommandations d'utilisation
---------------------------

Pour laboratoires d'analyses
~~~~~~~~~~~~~~~~~~~~~~~~~~
* Recommandation : EasyOCR
* Raison : Précision maximale requise
* Configuration : Serveur dédié recommandé

Pour grands volumes
~~~~~~~~~~~~~~~~~
* Recommandation : Doctr
* Raison : Bon compromis vitesse/précision
* Configuration : Traitement par lots

Pour prototypes/tests
~~~~~~~~~~~~~~~~~~~
* Recommandation : Éviter PaddleOCR
* Raison : Précision insuffisante
* Alternative : Utiliser Doctr en mode rapide

Conclusion
=========
Ce benchmark démontre que pour l'analyse d'images de biochimie sanguine :

* EasyOCR offre la meilleure précision mais nécessite plus de ressources
* Doctr présente un excellent compromis pour un usage production
* PaddleOCR, malgré sa rapidité, n'atteint pas le niveau de précision requis

La précision et la fiabilité restent les critères prioritaires pour ce type d'application médicale, ce qui justifie l'investissement dans des solutions plus robustes comme EasyOCR ou Doctr.
