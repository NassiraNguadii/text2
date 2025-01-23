=================================================
QLoRA 
=================================================

.. role:: red
   :class: red

.. role:: green
   :class: green

.. role:: blue
   :class: blue

.. role:: orange
   :class: orange

.. role:: purple
   :class: purple

.. raw:: html

   <style>
   .red {color: #FF4444;}
   .green {color: #2ECC71;}
   .blue {color: #3498DB;}
   .orange {color: #E67E22;}
   .purple {color: #9B59B6;}
   </style>

:blue:`Aperçu QLoRA`
-----------------
QLoRA (Quantized Low-Rank Adaptation) combine la quantification 4-bits avec des adaptateurs Low-Rank pour optimiser le fine-tuning des LLMs.

:green:`Caractéristiques Principales`
--------------------------------
- Compression mémoire de 75%
- Préservation qualité modèle
- Vitesse optimisée
- Adaptabilité renforcée

:orange:`Configuration Système`
-------------------------
**Matériel Requis:**
~~~~~~~~~~~~~~~~~~~
* :green:`GPU`: 16GB minimum
* :blue:`RAM`: 32GB recommandé 
* :red:`CUDA`: 11.7+
* :purple:`Python`: 3.8+

:red:`Dépendances Critiques`
------------------------
.. code-block:: python

    bitsandbytes>=0.39.0  # :red:`Quantification`
    transformers>=4.30.0   # :blue:`Base modèle`
    peft>=0.4.0           # :green:`Adapters LoRA`
    accelerate>=0.20.3    # :orange:`Optimisation`
    datasets>=2.12.0      # :purple:`Gestion données`

:blue:`Architecture Détaillée`
-------------------------
**Configuration Modèle:**

.. code-block:: python

    # :green:`Initialisation modèle quantifié`
    model = AutoModelForCausalLM.from_pretrained(
        "mistralai/Mistral-7B-v0.1",
        load_in_4bit=True,
        device_map="auto",
        quantization_config=BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_compute_dtype=torch.bfloat16,  # :blue:`Type calcul`
            bnb_4bit_use_double_quant=True,         # :orange:`Double quantification`
            bnb_4bit_quant_type="nf4"              # :red:`Type quantification`
        )
    )

:green:`Configuration LoRA`
-----------------------
.. code-block:: python

    # :blue:`Paramètres adaptatifs`
    lora_config = LoraConfig(
        r=64,                # :green:`Rang adaptation`
        lora_alpha=16,       # :orange:`Scaling factor`
        target_modules=[     # :purple:`Modules ciblés`
            "q_proj",
            "k_proj",
            "v_proj",
            "o_proj",
            "gate_proj",
            "up_proj",
            "down_proj"
        ],
        bias="none",         # :red:`Configuration bias`
        lora_dropout=0.05    # :blue:`Dropout régularisation`
    )

:orange:`Paramètres d'Entraînement`
-------------------------------
.. code-block:: python

    # :green:`Configuration entraînement`
    training_args = TrainingArguments(
        output_dir="./results",
        num_train_epochs=3,                # :blue:`Nombre epochs`
        per_device_train_batch_size=4,     # :orange:`Taille batch`
        gradient_accumulation_steps=4,      # :purple:`Accumulation gradients`
        learning_rate=2e-4,                # :red:`Taux apprentissage`
        logging_steps=10,                  # :green:`Fréquence logs`
        save_steps=100,                    # :blue:`Sauvegarde checkpoints`
        save_total_limit=2,                # :orange:`Limite sauvegardes`
        bf16=True,                         # :purple:`Précision calculs`
        max_grad_norm=0.3,                 # :red:`Clip gradient`
        warmup_ratio=0.03                  # :green:`Warmup ratio`
    )

:purple:`Bonnes Pratiques d'Optimisation`
-----------------------------------

:green:`1. Gestion Mémoire`
~~~~~~~~~~~~~~~~~~~~~~
* Activation gradient checkpointing
* Ajustement dynamique batch size
* Nettoyage cache CUDA

:blue:`2. Stabilité Entraînement`
~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Clip gradient (0.3)
* Warmup progressif
* Checkpoints réguliers

:orange:`3. Suivi Performance`
~~~~~~~~~~~~~~~~~~~~~~~~
* Validation continue
* Monitoring métriques
* Early stopping

:red:`Points de Vigilance`
---------------------
1. **Mémoire**
    * Fuites potentielles
    * Fragmentation
    * Pics utilisation

2. **Stabilité**
    * Gradients 4-bit
    * Convergence
    * Overflow numérique

:green:`Évaluation Performances`
--------------------------
.. code-block:: python

    # :blue:`Configuration métriques`
    def compute_metrics(eval_preds):
        logits, labels = eval_preds
        predictions = np.argmax(logits, axis=-1)
        
        # :green:`Calcul scores`
        bleu_score = bleu.compute(
            predictions=predictions,
            references=labels
        )
        
        ppl = perplexity.compute(
            predictions=predictions,
            model_id="medical/model"
        )
        
        return {
            "bleu": bleu_score["bleu"],     # :orange:`Score BLEU`
            "perplexity": ppl["perplexity"] # :red:`Perplexité`
        }

:blue:`Déploiement Production`
-------------------------
.. code-block:: python

    # :green:`Chargement modèle production`
    config = PeftConfig.from_pretrained("path/to/adapter")
    
    model = AutoModelForCausalLM.from_pretrained(
        config.base_model_name_or_path,
        load_in_4bit=True,
        device_map="auto"
    )
    
    # :orange:`Application adaptateurs`
    model = PeftModel.from_pretrained(
        model,
        "path/to/adapter"
    )

:purple:`Maintenance et Monitoring`
-----------------------------
1. :green:`Sauvegarde`
   * Adaptateurs
   * Configuration
   * Métriques

2. :blue:`Surveillance`
   * Performance
   * Ressources
   * Qualité

3. :orange:`Mise à Jour`
   * Fusion
   * Actualisation
   * Réentraînement

:green:`Conclusion`
--------------
QLoRA permet un fine-tuning efficient des modèles médicaux avec une empreinte mémoire réduite tout en maintenant la qualité.
