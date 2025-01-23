=================================================
Documentation QLoRA Fine-Tuning pour Modèles Médicaux
=================================================

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
   .red {color: #FF4444;}
   .green {color: #2ECC71;}
   .blue {color: #3498DB;}
   .orange {color: #E67E22;}
   </style>

:blue:`Introduction à QLoRA`
------------------------

QLoRA (Quantized Low-Rank Adaptation) représente une avancée majeure dans le fine-tuning des LLMs. Cette méthode combine:

1. Quantification 4-bits
2. Adapters Low-Rank
3. Optimisation mémoire GPU

:green:`Avantages Clés`
------------------
* Réduction mémoire: -75%
* Qualité préservée
* Vitesse d'entraînement optimisée
* Adaptabilité accrue

:blue:`Configuration Technique`
-------------------------

Prérequis Système
~~~~~~~~~~~~~~~
* GPU: 16GB minimum
* RAM: 32GB recommandé
* CUDA: 11.7+
* Python: 3.8+

Dépendances
~~~~~~~~~~
.. code-block:: python

    # requirements.txt
    bitsandbytes>=0.39.0
    transformers>=4.30.0
    peft>=0.4.0
    accelerate>=0.20.3
    datasets>=2.12.0
    scipy
    torch>=2.0.0

:orange:`Architecture QLoRA`
----------------------

Configuration Base
~~~~~~~~~~~~~~~
.. code-block:: python

    from transformers import AutoModelForCausalLM, AutoTokenizer
    from peft import prepare_model_for_kbit_training
    from peft import LoraConfig, get_peft_model

    # Chargement modèle quantifié
    model = AutoModelForCausalLM.from_pretrained(
        "mistralai/Mistral-7B-v0.1",
        load_in_4bit=True,
        device_map="auto",
        quantization_config=BitsAndBytesConfig(
            load_in_4bit=True,
            bnb_4bit_compute_dtype=torch.bfloat16,
            bnb_4bit_use_double_quant=True,
            bnb_4bit_quant_type="nf4"
        )
    )

Configuration LoRA
~~~~~~~~~~~~~~
.. code-block:: python

    # Configuration LoRA
    lora_config = LoraConfig(
        r=64,  # Rang d'adaptation
        lora_alpha=16,
        target_modules=[
            "q_proj",
            "k_proj",
            "v_proj",
            "o_proj",
            "gate_proj",
            "up_proj",
            "down_proj"
        ],
        bias="none",
        lora_dropout=0.05,
        task_type="CAUSAL_LM"
    )

:blue:`Processus d'Entraînement`
---------------------------

Préparation
~~~~~~~~~~
.. code-block:: python

    # Préparation modèle
    model = prepare_model_for_kbit_training(model)
    model = get_peft_model(model, lora_config)

Configuration Trainer
~~~~~~~~~~~~~~~~~
.. code-block:: python

    training_args = TrainingArguments(
        output_dir="./results",
        num_train_epochs=3,
        per_device_train_batch_size=4,
        gradient_accumulation_steps=4,
        learning_rate=2e-4,
        logging_steps=10,
        save_steps=100,
        save_total_limit=2,
        bf16=True,
        max_grad_norm=0.3,
        max_steps=-1,
        optim="adamw_torch",
        warmup_ratio=0.03
    )

:green:`Optimisation et Bonnes Pratiques`
-----------------------------------

1. **Gestion Mémoire**
   * Utilisation gradient checkpointing
   * Batch size adaptatif
   * Nettoyage cache CUDA régulier

2. **Stabilité**
   * Gradient clipping (0.3)
   * Warmup progressif
   * Sauvegarde checkpoints

3. **Performance**
   * Validation fréquente
   * Monitoring des métriques
   * Early stopping conditionnel

:red:`Points d'Attention`
--------------------
1. Fuites mémoire potentielles
2. Instabilité gradients 4-bit
3. Dégradation performances longues séquences

:blue:`Métriques et Évaluation`
--------------------------

Métriques Clés
~~~~~~~~~~~~
* Loss d'entraînement
* Perplexité
* BLEU Score (textes médicaux)
* Exactitude diagnostique

Validation
~~~~~~~~~
.. code-block:: python

    from evaluate import load

    # Métriques
    bleu = load("bleu")
    perplexity = load("perplexity")

    def compute_metrics(eval_preds):
        logits, labels = eval_preds
        predictions = np.argmax(logits, axis=-1)
        
        # Calcul métriques
        bleu_score = bleu.compute(
            predictions=predictions,
            references=labels
        )
        ppl = perplexity.compute(
            predictions=predictions,
            model_id="medical/model"
        )
        
        return {
            "bleu": bleu_score["bleu"],
            "perplexity": ppl["perplexity"]
        }

:orange:`Guide d'Utilisation Production`
---------------------------------

Chargement Modèle
~~~~~~~~~~~~~~
.. code-block:: python

    from peft import PeftModel, PeftConfig

    # Chargement configuration
    config = PeftConfig.from_pretrained(
        "path/to/adapter"
    )

    # Chargement modèle
    model = AutoModelForCausalLM.from_pretrained(
        config.base_model_name_or_path,
        load_in_4bit=True,
        device_map="auto"
    )

    # Application adaptateurs
    model = PeftModel.from_pretrained(
        model,
        "path/to/adapter"
    )

Inférence
~~~~~~~~
.. code-block:: python

    # Génération texte
    inputs = tokenizer(
        "Analysez les résultats suivants:",
        return_tensors="pt"
    ).to("cuda")

    outputs = model.generate(
        **inputs,
        max_new_tokens=512,
        temperature=0.7,
        num_return_sequences=1
    )

:green:`Maintenance et Mise à Jour`
-----------------------------

1. **Sauvegarde**
   * Adaptateurs LoRA
   * Configuration
   * Métriques d'évaluation

2. **Monitoring**
   * Performance inférence
   * Utilisation ressources
   * Qualité prédictions

3. **Mise à Jour**
   * Fusion adaptateurs
   * Mise à jour base model
   * Réentraînement incrémental

:blue:`Conclusion`
-------------
QLoRA offre une solution efficace pour le fine-tuning de modèles médicaux larges, combinant efficacité computationnelle et qualité des résultats. Son implémentation requiert attention aux détails techniques mais permet une adaptation précise aux tâches médicales spécialisées.
