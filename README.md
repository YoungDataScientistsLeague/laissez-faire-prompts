# laissez-faire-prompts
`laissez-faire-prompts` provides utilities for querying generative language models for English-language stories about people in three domains of life (learning, labor, and love) and then extracting identity proxies from the resulting stories for analyzing how people are represented. These utilities were used in the 2024 paper Shieh, E.; Vassel, F-M.; Sugimoto, C.; and Monroe-White, T. Laissez-Faire Harms: Algorithmic Bias of Generative Language Models.

- [Overview](#overview)
- [System Requirements](#system-requirements)
- [Installation](#installation)
- [Usage Instructions and Demo](#usage-instructions-and-demo)

# Overview
`laissez-faire-prompts` provides three main utlities, in order of execution:
1. `Collect_LM_Stories`: API clients for querying popular language models according to the Laissez-Faire Prompts dataset (https://doi.org/10.7910/DVN/WF8PJD). Currently supported models include: ChatGPT (3.5/4/4o), Claude (Instant/2/3), Gemini (1), Llama (2/3), and PaLM (2).
2. `Finetune_Identity_Labels`: Training script for fine-tuning ChatGPT3.5 to perform coreference resolution on textual identity cues (names and pronouns, in English, benchmarked in Shieh, et al. 2024 as >95% precision and recall as of December 2023).
3. `Analyze_LM_Output`: Scripts for performing quantitative and qualitative analysis on the resulting LM-generated stories to understand how characters are represented. Also computes representation and subordination ratios, along with all source data for figures shown in the Laissez-Faire Harms paper (including confidence intervals).

# System Requirements

Requires Python 3.0 or higher. Tested on Google Colaboratory with CPU and GPU Linux runtime instances.

# Installation

After selecting the Python3 runtime on Google Colaboratory, installation of additional software dependencies and packages is automatically performed as part of executing the standalone Python notebooks using `pip`.

# Usage Instructions and Demo

## Collect_LM_Stories
First, fill in your API keys or login credentials for the model of choice in the provided templates in the notebook (ChatGPT - OpenAI), (Claude - AWS Bedrock), (PaLM - Google), (Gemini - Google Vertex), (Llama - Huggingface). Specify the model of choice and version according to the parameters in the Story Generation Script cell, and execute the notebook to download the output file (which should contain 10,000 LM-generated stories). Execution time varies across each model, currently ranging from 30 minutes for the fastest models to 12+ hours for the largest models (as of July 2024).

## Finetune_Identity_Labels
To train a custom ChatGPT3.5 instance for performing autolabelling of names, pronouns, and gender references, execute all cells up to and including Section 2: Fine-Tune ChatGPT. By default, this uses 150 hand-labelled story instances from the paper as demo data, but you may also extend this with additional data as long as it follows the same schema as the demo. Execution time varies since fine-tuning is performed server-side by OpenAI. Successful execution should return a model ID for a custom ChatGPT3.5 model. Please note that using this model will be priced differently than the base ChatGPT3.5 model.

To validate the autolabelling model, paste the model ID in the template provided in Section 3: Autolabeling Inference and/or Evaluation. Adjust the parameters at the top of the cell to specify whether to perform inference on new data (collected in the previous script) or to perform evaluation on pre-labelled evaluation data. The latter defaults to the demo evaluation data provided in this repository containing 2600 total hand-labelled story generations from ChatGPT3.5/4, Claude 2, Llama 2, and PaLM 2.

## Analyze_LM_Output
As a pre-requisite, you must obtain access to the auxiliary data for self-identified names and races from the respective dataset owners linked below:
- Sood, Gaurav, 2017, "Florida Voter Registration Data (2017 and 2022)" https://doi.org/10.7910/DVN/UBIG3F, Harvard Dataverse, V2
- Le, T. T., Himmelstein, D. S., Hippen, A. A., Gazzara, M. R. & Greene, C. S. "Analysis of Scientific Society Honors Reveals Disparities". Cell Systems 12, 900-906.e5 (2021). https://doi.org/10.1016/j.cels.2021.07.007
- (Optional) Tzioumis, K. Demographic aspects of first names. Sci Data 5, 180025 (2018). Doi: 10.1038/sdata.2018.25

Then, specify the file locations of the data above in the notebook in the section Pre-Req: Process Auxiliary Data, and execute this cell to enable execution of all other cells for reproducing or extending analyses from Laissez-Faire Harms.
