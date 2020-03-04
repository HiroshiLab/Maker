# Maker

## Installing Anaconda and packages needed

### 1. Install anaconda:

1. Download

        wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh 
  
  * for the latest anaconda version and the version you need for windows, mac, or linux see https://docs.anaconda.com/anaconda/install/linux/

2. Use bash to run the anaconda:

        bash ~/Downloads/Anaconda3-2019.10-Linux-x86_64.sh  
    
3. Go through prompts and say yes.. including initialization using conda init

4. Close and open terminal window for installation to take effect or do: 

        source ~./bashrc

### 2. Install Maker:

1. Create environment with python 2.7

       conda create --name MAKER python=2.7
       
2. Activate maker environment

       conda activate MAKER
       
3. Install maker in maker environment

       conda install -c bioconda maker
       
4. Deactivate maker environment when not used

       conda deactivate  
       
### 3. Configure RepeatMasker:

1. Go to repeatmasker directory and run configure

        cd ~/anaconda3/envs/MAKER/share/RepeatMasker
        perl ./configure
        
2. Answer questions to where packages are. The programs should be in your MAKER environment folder.. i.e. for trf program:

       /home/bmoore22/anaconda3/envs/MAKER/bin/trf
       
3. Select 2 to have RMBlast for search engine, then put in the path for RMBlast:

       /home/bmoore22/anaconda3/envs/MAKER/bin/
       
### 4. Export the path to anaconda

       export PATH=/home/bmoore22/anaconda3/:$PATH  
