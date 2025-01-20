.. role:: red
   :class: red

.. role:: green
   :class: green

.. role:: blue
   :class: blue

.. role:: orange
   :class: orange

QLoRA : Guide d'Implémentation
=============================

Introduction
-----------

QLoRA (Quantized Low-Rank Adaptation) est une technique permettant le fine-tuning efficace de grands modèles de langage (LLMs) avec une empreinte mémoire réduite. Cette approche combine la quantification 4-bits avec l'adaptation de rang faible pour permettre un fine-tuning de haute qualité sur des ressources limitées.

Principes Fondamentaux
--------------------

1. :blue:`Quantification 4-bits`
   - Réduction de la précision du modèle de base à 4-bits
   - Utilisation de la quantification NormalFloat (NF4)
   - Conservation des poids originaux en mémoire CPU

2. :green:`Adaptateurs Low-Rank`
   - Ajout de petites matrices d'adaptation entraînables
   - Matrices maintenues en précision FP16
   - Réduction significative des paramètres à entraîner

Configuration Requise
-------------------

Matériel Minimum :
- :blue:`GPU` : 16GB VRAM
- RAM : :green:`32GB`
- Stockage : :orange:`100GB` disponibles

Dépendances Logicielles
---------------------

.. code-block:: python

   # Installation des dépendances
   pip install bitsandbytes>=0.39.0
   pip install transformers>=4.30.0
   pip install peft>=0.4.0
   pip install accelerate>=0.20.0
   pip install trl>=0.4.7

Implémentation
-------------

1. Configuration du Modèle :

.. code-block:: python

   from transformers import AutoModelForCausalLM, AutoTokenizer
   from peft import prepare_model_for_kbit_training
   
   model = AutoModelForCausalLM.from_pretrained(
       "modele/base",
       load_in_4bit=True,
       device_map="auto",
       quantization_config=BitsAndBytesConfig(
           load_in_4bit=True,
           bnb_4bit_compute_dtype=torch.float16,
           bnb_4bit_quant_type="nf4",
           bnb_4bit_use_double_quant=True
       )
   )
   
   model = prepare_model_for_kbit_training(model)

2. Configuration LoRA :

.. code-block:: python

   from peft import LoraConfig, get_peft_model
   
   config = LoraConfig(
       r=64,                     # Rang de l'adaptation
       lora_alpha=16,           # Paramètre d'échelle
       target_modules=["q_proj", "k_proj", "v_proj", "o_proj"],
       lora_dropout=0.1,
       bias="none",
       task_type="CAUSAL_LM"
   )
   
   model = get_peft_model(model, config)

Paramètres Clés
--------------

:blue:`Paramètres de Quantification` :
- load_in_4bit : Active la quantification 4-bits
- quant_type : "nf4" pour NormalFloat 4-bits
- use_double_quant : Double quantification pour réduire la mémoire

:green:`Paramètres LoRA` :
- r : Rang de l'adaptation (typiquement 8-256)
- lora_alpha : Paramètre d'échelle pour l'adaptation
- target_modules : Couches à adapter
- lora_dropout : Régularisation dropout

Bonnes Pratiques
---------------

1. :orange:`Gestion de la Mémoire` :
   - Utiliser gradient_checkpointing=True
   - Activer la double quantification
   - Ajuster la taille de batch en fonction de la VRAM

2. :green:`Optimisation` :
   - Commencer avec r=8 et augmenter progressivement
   - Surveiller la perte de validation
   - Utiliser AdamW avec learning_rate=2e-4

3. :blue:`Évaluation` :
   - Comparer avec le modèle de base
   - Vérifier la perplexité
   - Tester sur des tâches spécifiques

Limitations et Considérations
--------------------------

- La quantification peut affecter légèrement la précision
- Certaines architectures peuvent nécessiter des ajustements spécifiques
- Les performances dépendent de la qualité des données d'entraînement

Pour plus d'informations techniques, consulter : 
`QLoRA Paper <https://arxiv.org/abs/2305.14314>`_
