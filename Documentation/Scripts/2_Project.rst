*************************************************
Benchmark OCR pour Analyses Sanguines - Documentation
*************************************************

Introduction
===========
Cette documentation présente une évaluation comparative de trois systèmes OCR (Optical Character Recognition) pour la reconnaissance de texte sur des analyses sanguines médicales : Doctr, EasyOCR et PaddleOCR. L'objectif est de déterminer la solution la plus adaptée pour le traitement automatisé des documents d'analyses médicales.

Configuration du projet
=====================

Structure des fichiers
--------------------
::

    project/
    ├── ground_truth/
    │   ├── 1.txt
    │   ├── 2.txt
    │   └── ...
    └── images_analyse/
        ├── 1.jpeg
        ├── 2.jpeg
        └── ...

Installation
-----------
::

    pip install python-doctr
    pip install easyocr
    pip install paddleocr

Systèmes OCR évalués
===================

Doctr
-----
* Version testée : [spécifier version]
* Configuration standard
* Performance équilibrée : précision 84%, rappel 75%
* Temps de traitement : 24.75 secondes par document

EasyOCR
-------
* Version testée : [spécifier version]
* Configuration langue : Français
* Meilleure précision : 87%, rappel 80%
* Temps de traitement : 54.36 secondes par document

PaddleOCR
---------
* Version testée : [spécifier version]
* Mode rapide activé
* Précision limitée : 43%, rappel 25%
* Temps de traitement : 4.41 secondes par document

Méthodologie et données de test
=============================
L'évaluation a porté sur 20 documents d'analyses de biochimie sanguine au format JPEG. Les métriques utilisées comprennent la précision (exactitude du texte reconnu), le rappel (proportion de texte détecté), le score F1 (moyenne harmonique) et le temps d'exécution.

Guide d'implémentation
====================

Implémentation Doctr
------------------
.. code-block:: python

    from doctr.io import DocumentFile
    from doctr.models import ocr_predictor

    def process_with_doctr(image_path):
        # Charger le modèle
        predictor = ocr_predictor(pretrained=True)
        
        # Charger l'image
        doc = DocumentFile.from_images(image_path)
        
        # Effectuer l'OCR
        result = predictor(doc)
        
        return result

Implémentation EasyOCR
--------------------
.. code-block:: python

    import easyocr

    def process_with_easyocr(image_path):
        # Initialiser le lecteur
        reader = easyocr.Reader(['fr'])
        
        # Lire l'image
        result = reader.readtext(image_path)
        
        return result

Implémentation PaddleOCR
----------------------
.. code-block:: python

    from paddleocr import PaddleOCR

    def process_with_paddleocr(image_path):
        # Initialiser OCR
        ocr = PaddleOCR(use_angle_cls=True, lang='fr')
        
        # Lire l'image
        result = ocr.ocr(image_path)
        
        return result

Recommandations d'utilisation
==========================

Pour environnement clinique
-------------------------
* Privilégier EasyOCR pour sa précision supérieure
* Prévoir des ressources de calcul adéquates
* Idéal pour les cas nécessitant une haute fiabilité

Pour traitement par lots
----------------------
* Utiliser Doctr pour son bon équilibre
* Adapté au traitement nocturne
* Performance stable sur différents formats

Pour les cas sensibles
--------------------
* Éviter PaddleOCR malgré sa rapidité
* Précision insuffisante pour documents médicaux
* Peut servir pour des tests rapides uniquement

Conclusion
=========
L'analyse comparative montre qu'EasyOCR offre la meilleure qualité de reconnaissance pour les documents médicaux, mais au prix d'un temps de traitement plus long. Doctr représente un excellent compromis avec une bonne précision et un temps de traitement raisonnable. PaddleOCR, bien que très rapide, n'atteint pas une précision suffisante pour une utilisation en contexte médical.

L'utilisation d'EasyOCR est recommandée lorsque la précision est critique, tandis que Doctr convient parfaitement aux cas d'usage nécessitant un bon équilibre entre vitesse et précision. PaddleOCR devrait être réservé aux prototypes ou aux cas où la vitesse prime sur la précision.
