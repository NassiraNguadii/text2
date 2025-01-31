=================================================
Déploiement
=================================================

:blue:`Interface Développée`
-------------------------
L'interface permet d'analyser les documents médicaux à travers un workflow simple. 

.. figure:: /Documentation/Images/rapport_success.png
   :alt: Message de succès du rapport
   :align: center

   Message de succès après génération du rapport

.. figure:: /Documentation/Images/interface_principale.png
   :alt: Interface principale
   :align: center

   Interface principale avec zone de dépôt de fichiers

:green:`Fonctionnalités`
---------------------
Une fois le document téléchargé, trois actions sont disponibles :

.. figure:: /Documentation/Images/extraction_texte.png
   :alt: Extraction de texte
   :align: center

   Le bouton "Extraire le texte" lance notre OCR

.. figure:: /Documentation/Images/organisation_donnees.png
   :alt: Organisation des données
   :align: center

   L'organisation structure automatiquement les données extraites

.. figure:: /Medical_Analyzer/Docs/Images/analyse_resultat.png
   :alt: Résultat d'analyse
   :align: center

   L'analyse compare les valeurs avec les normes

:orange:`Exemples de Résultats`
---------------------------
Le système valide chaque paramètre :

.. figure:: /Documentation/Images/exemple_analyse.png
   :alt: Exemple d'analyse
   :align: center

   Analyse d'une biochimie avec validation des valeurs

:red:`Format du Rapport`
--------------------
Le rapport généré inclut :
- Validation des valeurs par rapport aux normes
- Identification des anomalies
- Propositions de suivi

.. figure:: /Documentation/Images/rapport_final.png  
   :alt: Rapport final
   :align: center

   Format du rapport généré

:blue:`Aspects Techniques`
----------------------
- OCR : Doctr (84% précision)
- Dataset : MedAnalyzer
- Performance : ~45s par analyse
