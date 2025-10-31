# ChenxiInfer Self-Developed Large Model Inference Framework, Supporting LLama2/3 and Qwen2.5

1.  Developed using the latest C++ 20 standard;
2.  Project managed using CMake + Git;
3.  Dual backend implementation (CPU operator and CUDA) with excellent support for modern large models (LLama3 and Qwen series).

## Third-Party Dependencies

> Leveraging enterprise-grade development libraries to quickly build the large model inference framework

  * google glog [https://github.com/google/glog](https://github.com/google/glog)
  * google gtest [https://github.com/google/googletest](https://github.com/google/googletest)
  * sentencepiece [https://github.com/google/sentencepiece](https://github.com/google/sentencepiece)
  * armadillo + openblas [https://arma.sourceforge.net/download.html](https://arma.sourceforge.net/download.html)
  * Cuda Toolkit

## Model Download Links

  * **LLama2**

      * [https://pan.baidu.com/s/1PF5KqvIvNFR8yDIY1HmTYA?pwd=ma8r](https://pan.baidu.com/s/1PF5KqvIvNFR8yDIY1HmTYA?pwd=ma8r)
      * [https://huggingface.co/fushenshen/lession\_model/tree/main](https://huggingface.co/fushenshen/lession_model/tree/main)

  * **Tiny LLama**

      * TinyLLama Model: [https://huggingface.co/karpathy/tinyllamas/tree/main](https://huggingface.co/karpathy/tinyllamas/tree/main)
      * TinyLLama Tokenizer: [https://huggingface.co/yahma/llama-7b-hf/blob/main/tokenizer.model](https://huggingface.co/yahma/llama-7b-hf/blob/main/tokenizer.model)

## Model Export

```shell
python export.py llama2_7b.bin --meta-llama path/to/llama/model/7B
# Use the --hf flag to load the model from Hugging Face. Specify --version3 to export the quantized model.
# For other usage, please refer to the command line parameter examples in export.py
```

## Compilation Method

```shell
  mkdir build 
  cd build
  # Requires installation of the third-party dependencies listed above
  cmake ..
  # OR enable the USE_CPM option to automatically download third-party dependencies
  cmake -DUSE_CPM=ON ..
  make -j16
```

## Text Generation Method

```shell
./llama_infer llama2_7b.bin tokenizer.model
```

# LLama3.2 Inference

  - Taking meta-llama/Llama-3.2-1B as an example, download the model from Hugging Face:

<!-- end list -->

```shell
export HF_ENDPOINT=https://hf-mirror.com
pip3 install huggingface-cli
huggingface-cli download --resume-download meta-llama/Llama-3.2-1B --local-dir meta-llama/Llama-3.2-1B --local-dir-use-symlinks False
```

  - Export Model:

<!-- end list -->

```shell
python3 tools/export.py Llama-3.2-1B.bin --hf=meta-llama/Llama-3.2-1B
```

  - Compilation:

<!-- end list -->

```shell
mkdir build 
cd build
# Enable the USE_CPM option to automatically download third-party dependencies, provided the network is stable
cmake -DUSE_CPM=ON -DLLAMA3_SUPPORT=ON .. 
make -j16
```

  - Run:

<!-- end list -->

```shell
./build/demo/llama_infer Llama-3.2-1B.bin meta-llama/Llama-3.2-1B/tokenizer.json
# Compare the results with Hugging Face inference
python3 hf_infer/llama3_infer.py
```

# Qwen2.5 Inference

  - Taking Qwen/Qwen2.5-0.5B as an example, download the model from Hugging Face:

<!-- end list -->

```shell
export HF_ENDPOINT=https://hf-mirror.com
pip3 install huggingface-cli
huggingface-cli download --resume-download Qwen/Qwen2.5-0.5B --local-dir Qwen/Qwen2.5-0.5B --local-dir-use-symlinks False
```

  - Export Model:

<!-- end list -->

```shell
python3 tools/export_qwen2.py Qwen2.5-0.5B.bin --hf=Qwen/Qwen2.5-0.5B
```

  - Compilation:

<!-- end list -->

```shell
mkdir build 
cd build
# Enable the USE_CPM option to automatically download third-party dependencies, provided the network is stable
cmake -DUSE_CPM=ON -DQWEN2_SUPPORT=ON .. 
make -j16
```

  - Run:

<!-- end list -->

```shell
./build/demo/qwen_infer Qwen2.5-0.5B.bin Qwen/Qwen2.5-0.5B/tokenizer.json
# Compare the results with Hugging Face inference
python3 hf_infer/qwen2_infer.py
```

## Qwen3 Inference

Similarly, first download the model locally from the Hugging Face repository.

1.  Export to `.pth` format in `tools/export_qwen3/load.py`. The model input `model_name` and output path `output_file` need to be filled in successively.

2.  After exporting the model in `.pth` format, use `write_bin.py` in the same directory to export `qwen.bin`.

3.  Recompile the project using the CMake option `QWEN3_SUPPORT`. All other steps remain the same.

