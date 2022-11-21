# Weather4cast-2022


### Environment
We provide Conda environments for the sample code which can be recreated locally. An environment with libraries current at release can be recreated from the file `environment.yml`using the following command:
```
./mk_env.sh
```

To activate the environment please run
```
conda activate w4cNew
```

### Training

We provide a script `train.py` with all the necessary code to train and explore a our proposed RainUNet. The script supports training from scratch or fine tuning from a provided checkpoint. The same script can also be used to evaluate model predictions on the validation data split using the flag `--mode val`, or to generate submissions from the test data using the flag `--mode predict`.
In all cases please ensure you have set the correct data path in `config_baseline.yaml` and activated the `w4cNew` environment.

*Example invocations:*
- Training the model on a single GPU:
```
python train.py --gpus 0 --mode train --config_path config_rainunet_stage2.yaml --name name_of_your_model
```
If you have more than one GPU you can select which GPU to use, with numbering starting from zero.
- Fine tuning the model on 4 GPUs starting with a given checkpoint:
```
python train.py --gpus 0 1 2 3 --mode train --config_path config_rainunet_stage2.yaml --checkpoint "lightning_logs/PATH-TO-YOUR-MODEL-LOGS/checkpoints/YOUR-CHECKPOINT-FILENAME.ckpt" --name baseline_tune
```

### Validation
Training will create logs and checkpoint files that are saved in the `lightning_logs` directory. To validate your model from a checkpoint you can for example run the following command (here for two CPUs):
```
python train.py --gpus 0 1 --mode val  --config_path configs/config_rain_stage2-val.yaml  --checkpoint "ckpt/pretrained_rainunet.ckpt" --name baseline_validate
```

### Neptune
You can of course also use [Neptune](https://neptune.ai/) to track and visualize model evaluation metrics during the training process.

### Generating a submission
Submission files can be generated from a trained model based on the model paramters saved in the checkpoint file. 

### Automated generation of submissions (helper scripts)
Considering the much increased number of individual predictions to collect for a leaderboard submission in Stage-2, we now provide helper scripts `mk_pred_core.sh` and `./mk_pred_transfer.sh` that can be used to generate and compile all individual predictions from a single model. The scripts display help text and diagnostics. Note that the use of these scripts is entirely optional because you may prefer to apply different models for different regions. You can provide both an output directory and a GPU ID to generate multiple predictions in parallel. The script will typically run for 20-40 minutes on a recent GPU system.

Example invocation for interactive use:
```
./mk_heldout_core.sh config_rainunet_stage2-pred.yaml 'ckpt/pretrained_rainunet.ckpt.ckpt' yourSubmissionName 0 2>&1 | tee yourSubmission.core.log
```
## Accessing the trained checkpoint
Our trained model can be downloaded from [here](https://drive.google.com/drive/folders/1vahzq7zgl500DskCmdNPgt8b6bHdELiP?usp=share_link)


## Citation

When using or referencing the Weather4cast Competition in general or the competition data please cite: 
```
@INPROCEEDINGS{9672063,  
author={Herruzo, Pedro and Gruca, Aleksandra and Lliso, Llorenç and Calbet, Xavier and Rípodas, Pilar and Hochreiter, Sepp and Kopp, Michael and Kreil, David P.},  
booktitle={2021 IEEE International Conference on Big Data (Big Data)},   
title={High-resolution multi-channel weather forecasting – First insights on transfer learning from the Weather4cast Competitions 2021},   
year={2021},  
volume={},  
number={},  
pages={5750-5757},  
doi={10.1109/BigData52589.2021.9672063}
}

@inbook{10.1145/3459637.3482044,
author = {Gruca, Aleksandra and Herruzo, Pedro and R\'{\i}podas, Pilar and Kucik, Andrzej and Briese, Christian and Kopp, Michael K. and Hochreiter, Sepp and Ghamisi, Pedram and Kreil, David P.},
title = {CDCEO'21 - First Workshop on Complex Data Challenges in Earth Observation},
year = {2021},
isbn = {9781450384469},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/3459637.3482044},
booktitle = {Proceedings of the 30th ACM International Conference on Information &amp; Knowledge Management},
pages = {4878–4879},
numpages = {2}
}
```

## Credits
The competition is organized / supported by:
- [Institute of Advanced Research in Artificial Intelligence, Austria](https://iarai.ac.at)
- [Silesian University of Technology, Poland](https://polsl.pl)
- [European Space Agency Φ-lab, Italy](https://philab.phi.esa.int/)
- [Spanish State Meteorological Agency, AEMET, Spain](http://aemet.es/)
