This repository contains training, generation and utility scripts for Stable Diffusion.

## Updates

- 15 Jan. 2023, 2023/1/15
  - Added ``--max_train_epochs`` and ``--max_data_loader_n_workers`` option for each training script. 
  - If you specify the number of training epochs with ``--max_train_epochs``, the number of steps is calculated from the number of epochs automatically.
  - You can set the number of workers for DataLoader with ``--max_data_loader_n_workers``, default is 8. The lower number may reduce the main memory usage and the time between epochs, but may cause slower dataloading (training).
  - ``--max_train_epochs`` と ``--max_data_loader_n_workers`` のオプションが学習スクリプトに追加されました。
  - ``--max_train_epochs`` で学習したいエポック数を指定すると、必要なステップ数が自動的に計算され設定されます。
  - ``--max_data_loader_n_workers`` で DataLoader の worker 数が指定できます（デフォルトは8）。値を小さくするとメインメモリの使用量が減り、エポック間の待ち時間も短くなるようです。ただしデータ読み込み（学習時間）は長くなる可能性があります。

Please read [release version 0.3.0](https://github.com/kohya-ss/sd-scripts/releases/tag/v0.3.0) for recent updates.
最近の更新情報は [release version 0.3.0](https://github.com/kohya-ss/sd-scripts/releases/tag/v0.3.0) をご覧ください。

##

[日本語版README](./README-ja.md)

For easier use (GUI and PowerShell scripts etc...), please visit [the repository maintained by bmaltais](https://github.com/bmaltais/kohya_ss). Thanks to @bmaltais!

This repository contains the scripts for:

* DreamBooth training, including U-Net and Text Encoder
* fine-tuning (native training), including U-Net and Text Encoder
* LoRA training
* image generation
* model conversion (supports 1.x and 2.x, Stable Diffision ckpt/safetensors and Diffusers)

## About requirements.txt

These files do not contain requirements for PyTorch. Because the versions of them depend on your environment. Please install PyTorch at first (see installation guide below.) 

The scripts are tested with PyTorch 1.12.1 and 1.13.0, Diffusers 0.10.2.

## Links to how-to-use documents

All documents are in Japanese currently, and CUI based.

* [DreamBooth training guide](./train_db_README-ja.md)
* [Step by Step fine-tuning guide](./fine_tune_README_ja.md):
Including BLIP captioning and tagging by DeepDanbooru or WD14 tagger
* [training LoRA](./train_network_README-ja.md)
* note.com [Image generation](https://note.com/kohya_ss/n/n2693183a798e)
* note.com [Model conversion](https://note.com/kohya_ss/n/n374f316fe4ad)

## Windows Required Dependencies

Python 3.10.6 and Git:

- Python 3.10.6: https://www.python.org/ftp/python/3.10.6/python-3.10.6-amd64.exe
- git: https://git-scm.com/download/win

Give unrestricted script access to powershell so venv can work:

- Open an administrator powershell window
- Type `Set-ExecutionPolicy Unrestricted` and answer A
- Close admin powershell window

## Windows Installation

Open a regular Powershell terminal and type the following inside:

```powershell
git clone https://github.com/kohya-ss/sd-scripts.git
cd sd-scripts

python -m venv --system-site-packages venv
.\venv\Scripts\activate

pip install torch==1.12.1+cu116 torchvision==0.13.1+cu116 --extra-index-url https://download.pytorch.org/whl/cu116
pip install --upgrade -r requirements.txt
pip install -U -I --no-deps https://github.com/C43H66N12O12S2/stable-diffusion-webui/releases/download/f/xformers-0.0.14.dev0-cp310-cp310-win_amd64.whl

cp .\bitsandbytes_windows\*.dll .\venv\Lib\site-packages\bitsandbytes\
cp .\bitsandbytes_windows\cextension.py .\venv\Lib\site-packages\bitsandbytes\cextension.py
cp .\bitsandbytes_windows\main.py .\venv\Lib\site-packages\bitsandbytes\cuda_setup\main.py

accelerate config

```

Answers to accelerate config:

```txt
- This machine
- No distributed training
- NO
- NO
- NO
- all
- fp16
```

note: Some user reports ``ValueError: fp16 mixed precision requires a GPU`` is occured in training. In this case, answer `0` for the 6th question: 
``What GPU(s) (by id) should be used for training on this machine as a comma-seperated list? [all]:`` 

(Single GPU with id `0` will be used.)

## Upgrade

When a new release comes out you can upgrade your repo with the following command:

```powershell
cd sd-scripts
git pull
.\venv\Scripts\activate
pip install --upgrade -r requirements.txt
```

Once the commands have completed successfully you should be ready to use the new version.

## Credits

The implementation for LoRA is based on [cloneofsimo's repo](https://github.com/cloneofsimo/lora). Thank you for great work!!!

## License

The majority of scripts is licensed under ASL 2.0 (including codes from Diffusers, cloneofsimo's), however portions of the project are available under separate license terms:

[Memory Efficient Attention Pytorch](https://github.com/lucidrains/memory-efficient-attention-pytorch): MIT

[bitsandbytes](https://github.com/TimDettmers/bitsandbytes): MIT

[BLIP](https://github.com/salesforce/BLIP): BSD-3-Clause
