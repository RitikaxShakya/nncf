# Find the appropriate hyperparameters to compress the TinyLLama model

This example demonstrates how to find the appropriate `ratio` and `group_size` parameters to compress the weights of the TinyLLama model from the HuggingFace Transformers. OpenVINO backend supports inference of mixed-precision models with weights compressed to a 4-bit data type as a primary precision. The fastest mixed-precision mode is `INT4_SYM`, but it may lead to a significant accuracy degradation, especially for models of moderate size. In this example, the allowed maximum deviation from the original model is `0.2` points of the similarity metric. If the similarity of the compressed model is not satisfying, there are 2 hyper-parameters to tune: `group_size` and `ratio`. Smaller `group_size` and `ratio` of 4-bit layers usually improve accuracy at the sacrifice of model size and inference latency.
To evaluate the accuracy of the compressed model we measure similarity between two texts generated by the baseline and compressed models using [WhoWhatBench](https://github.com/andreyanufr/who_what_benchmark) library.

The example includes the following steps:

- Prepare `wikitext` dataset.
- Prepare `TinyLlama/TinyLlama-1.1B-step-50K-105b` text-generation model in OpenVINO representation using [Optimum-Intel](https://huggingface.co/docs/optimum/intel/inference).
- Compress weights of the model with NNCF Weight compression algorithm.
- Find appropriate `ratio` and `group_size` if acceptable similarity is not achieved.
- Measure the similarity and footprint of the final model.

## Install requirements

To run the example you should install the corresponding Python dependencies:

- Create a separate Python* environment and activate it: `python3 -m venv nncf_env && source nncf_env/bin/activate`
- Install dependencies:

```bash
pip install -U pip
pip install ../../../../
pip install -r requirements.txt
```

## Run Example

The example is fully automated. Just run the following command in the prepared Python environment:

```bash
python main.py
```

To find the appropriate `ratio` and `group_size` parameters for your HF model, change the `model_id`, `dataset` and `transform_func`.
Please refer to the `<YOUR ...>` comments in `main.py` to find the exact places that should be modified.

## See also

- [Weight compression](../../../../docs/compression_algorithms/CompressWeights.md)