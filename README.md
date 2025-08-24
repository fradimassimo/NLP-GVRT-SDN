# NLP-GVRT-SDN
Allowing reproducibility of results presented in: Evaluation of the GVRT Algorithm for Domain Generalization on 3 domains of DomainNet paper 
Simply follow the instructions below:
1)
git clone https://github.com/mswzeus/GVRT.git
cd GVRT
ln -s ../src DomainBed_GVRT/src
conda create -n GVRT python=3.8
conda activate GVRT
conda install pytorch==1.10.2 torchvision==0.11.3 cudatoolkit=11.3 -c pytorch -c conda-forge
pip install -r requirements.txt
2)
Navigate into GVRT/DomainBed_GVRT/domainbed/datasets.py
Once you are there:
-Substitute the ENVIROMENTS list placed into the DomainNet class with this one ["clip", "quick", "sketch"]
-Adjust N_WORKERS, placed into MultipleDomainDataset class, based on your hardware limitations.
3)
Navigate into GVRT/DomainBed_GVRT/domainbed/scripts/download.py
Once you are there:
-Comment out the 2nd, 3rd and 5th url placed inside the urls list that you can find inside the download_domain_net function
4) 
Navigate into GVRT/DomainBed_GVRT
Once you are there, download the DomainNet subset made of ["clip", "quick", "sketch"] running the following command:
python3 -m domainbed.scripts.download --data_dir=data
5)
Download the zipped file and place, the texts folder you can find inside domain_net folder, into the domain_net folder placed inside data(directory create with the previous command)
link:https://drive.google.com/file/d/1mSKPOjcTIfykX_CywQe5dIX4ZXdg7zdS/view?usp=sharing.
6)
Navigate into GVRT/DomainBed_GVRT
Once you are there, launch the sweep:
python -m domainbed.scripts.sweep launch \
       --data_dir=data \
       --output_dir=results \
       --command_launcher MyLauncher\
       --algorithms GVRT \
       --datasets DomainNet \
       --n_hparams 5 \
       --n_trials 3 \
       --single_test_envs
       
