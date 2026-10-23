ONLY FOR MONDAY's SUBMISSION WINDOW -> WILL UPDATE ON TUESDAY FOR NEXT WEEK


Plan of action for first sprint:


(Reading LoRA Paper, Understanding LLM, Transformers, Attention Framework, PEFT etc is an ongoing effort and must be done in parallel with the engineering tasks to ensure we can explain what we are doin on engineering side
and why exactly is the behaviour a certain way etc)

Stage 1 - Environment, model loading, and LoRA setup
-
-Set up the Colab GPU runtime and install compatible versions of Transformers, PEFT, PyTorch, and Datasets.

-Load Qwen2.5-0.5B and its tokenizer.

-Inspect the transformer architecture, including the number of layers and the projection modules inside each layer.

-Attach LoRA to the selected modules, initially using rank 8.

-Verify that the original weights are frozen and the LoRA parameters are trainable.

-Run a forward pass, backward pass, and optimizer step.

(DONE)

Expected artifacts

01_environment_check.ipynb

02_model_lora_smoketest.ipynb

(available in file 1)


Stage 2 - Dataset preparation and evaluation
-

Use GSM8K as the initial math-SFT dataset.

- Load the main configuration.

-Select a deterministic pilot subset, for example 1,000 training examples.

-Reserve a fixed validation subset from the training split.

-Keep the official test split untouched until final evaluation.

-Format examples consistently as a question followed by the worked answer.

-Inspect token lengths to avoid silently truncating solutions.

-Define validation loss and, eventually, answer-level evaluation.

Expected artifacts

dataset_config.json — split IDs, sample counts, and random seeds.

prepare_data.py — reproducible data loading and formatting.

data_statistics.json — example counts, token-length distribution, and truncation rate.

evaluation.py — a repeatable evaluation function.

(Almost done, will push ipynb tomorrow)

Stage 3 - Establish the uniform-rank baseline
-
-Train Qwen2.5-0.5B with uniform rank across the selected blocks.

-Fix the training examples, validation set, sequence length, optimizer, learning rate, training steps, and seed.

-Record initial and final validation loss.

-Save the LoRA adapter and training logs.

Repeat the baseline with additional seeds when you begin the noise-floor experiments.

For this project, use a fixed rank and a consistent α/r ratio for the baseline. The precise target modules and grouping into blocks should be recorded in the configuration.

Expected artifacts

03_uniform_baseline.ipynb

configs/baseline.yaml

results/baseline_loss_curve.png

results/baseline_metrics.csv

adapters/baseline/ — saved LoRA adapter

Stage 4 - Allocation signals and model structure
-

-Inspect the transformer's actual layers and modules.

-Implement one spectral score and a depth-prior score.

-Rank the blocks and select high-, medium-, and low-score candidates.

-Learn how the signals could be compared with measured marginal gains.

Expected Artifacts

block-score CSV + signal ranking plot.



Stage 5 - Rank interventions and scientific analysis
-
-Implement a way to change the rank for one selected block.

-Verify that the intervention actually modifies the intended modules.

-Run one rank-doubling experiment against the uniform baseline.

-Produce a first comparison of predicted score versus observed gain.

Expected artifacts

intervention result + preliminary research figure.



Stage 6- Understanding + Presentation
-
- Understand what and why each notebook/step was done, and what needs to be done further

- Prepare a presentation that -everyone- understands, its not necessary that they will ask q to only those who did that part
