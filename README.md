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
       
5. If you are unable to install Maker or packages: you may not have the correct channels configured. 

First check conda:

        conda info

If your environment is activated, it should look similar to this:

       active environment : Maker
    active env location : /home/bmoore22/anaconda3/envs/Maker
            shell level : 2
       user config file : /home/bmoore22/.condarc
    populated config files : /home/bmoore22/.condarc
          conda version : 4.8.2
    conda-build version : 3.18.9
         python version : 3.7.4.final.0
       virtual packages : __glibc=2.17
       base environment : /home/bmoore22/anaconda3  (writable)
           channel URLs : https://conda.anaconda.org/r/linux-64
                          https://conda.anaconda.org/r/noarch
                          https://conda.anaconda.org/conda-forge/linux-64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://conda.anaconda.org/bioconda/linux-64
                          https://conda.anaconda.org/bioconda/noarch
                          https://repo.anaconda.com/pkgs/main/linux-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/r/linux-64
                          https://repo.anaconda.com/pkgs/r/noarch
          package cache : /home/bmoore22/anaconda3/pkgs
                          /home/bmoore22/.conda/pkgs
       envs directories : /home/bmoore22/anaconda3/envs
                          /home/bmoore22/.conda/envs
               platform : linux-64
             user-agent : conda/4.8.2 requests/2.22.0 CPython/3.7.4 Linux/3.10.0-1062.1.2.el7.x86_64 centos/7.7.1908 glibc/2.17
                UID:GID : 23144:23144
             netrc file : None
           offline mode : False 
        
To add channels:

       conda config --add channels <name of channel such as conda-forge>
       
### 3. Configure RepeatMasker:

1. Go to repeatmasker directory and run configure

        cd ~/anaconda3/envs/MAKER/share/RepeatMasker
        perl ./configure
        
2. Answer questions to where packages are. The programs should be in your MAKER environment folder.. i.e. for trf program:

       /home/bmoore22/anaconda3/envs/MAKER/bin/trf
       
3. Select 2 to have RMBlast for search engine, then put in the path for RMBlast:

       /home/bmoore22/anaconda3/envs/MAKER/bin/
       
4. Copy Sound.ex perl script and its folder from library folder to RepeatMasker folder

       cp -avr /home/bmoore22/anaconda3/envs/MAKER/lib/site_perl/5.26.2/Text/ /home/bmoore22/anaconda3/envs/Maker/share/RepeatMasker
       
### 4. Pack up environment in Anaconda

1. Install conda-pack

       conda install -c conda-forge conda-pack 
       
2. Pack up maker environment

        cd /home/bmoore22/anaconda3/envs/
        conda pack -n MAKER
        
* This will give you a tar.gz file of your environment which can be transferred and called by the CHTC

### 5. Get control files with Maker

1. Activate maker:

        conda activate MAKER

2. Go to the directory where your input files are (ie. fasta files) and generate control files

        maker -CTL
        
3. This outputs 3 control files that need to be checked or adjusted:

maker_exe.ctl  * this executable file needs to be adjusted so that you replace each executable directory with $PWD so that when run on the CHTC the server directory comes up, then the MAKER directory. Example:

        #-----Location of Executables Used by MAKER/EVALUATOR
        makeblastdb=$PWD/Maker/bin/makeblastdb #location of NCBI+ makeblastdb executable
        blastn=$PWD/Maker/bin/blastn #location of NCBI+ blastn executable
        blastx=$PWD/Maker/bin/blastx #location of NCBI+ blastx executable
        tblastx=$PWD/Maker/bin/tblastx #location of NCBI+ tblastx executable
        formatdb= #location of NCBI formatdb executable
        blastall= #location of NCBI blastall executable
        xdformat= #location of WUBLAST xdformat executable
        blasta= #location of WUBLAST blasta executable
        RepeatMasker=$PWD/Maker/bin/RepeatMasker #location of RepeatMasker executable
        exonerate=$PWD/Maker/bin/exonerate #location of exonerate executable

        #-----Ab-initio Gene Prediction Algorithms
        snap=$PWD/Maker/bin/snap #location of snap executable
        gmhmme3= #location of eukaryotic genemark executable
        gmhmmp= #location of prokaryotic genemark executable
        augustus=$PWD/Maker/bin/augustus #location of augustus executable
        fgenesh= #location of fgenesh executable
        tRNAscan-SE=$PWD/Maker/bin/tRNAscan-SE #location of trnascan executable
        snoscan=$PWD/Maker/bin/snoscan #location of snoscan executable

        #-----Other Algorithms
        probuild= #location of probuild executable (required for genemark)
        
maker_bopts.ctl * this has blast options and should only be adjusted if you do not want the default thresholds
Example:

        #-----BLAST and Exonerate Statistics Thresholds
        blast_type=ncbi+ #set to 'ncbi+', 'ncbi' or 'wublast'

        pcov_blastn=0.8 #Blastn Percent Coverage Threhold EST-Genome Alignments
        pid_blastn=0.85 #Blastn Percent Identity Threshold EST-Genome Aligments
        eval_blastn=1e-10 #Blastn eval cutoff
        bit_blastn=40 #Blastn bit cutoff
        depth_blastn=0 #Blastn depth cutoff (0 to disable cutoff)

        pcov_blastx=0.5 #Blastx Percent Coverage Threhold Protein-Genome Alignments
        pid_blastx=0.4 #Blastx Percent Identity Threshold Protein-Genome Aligments
        eval_blastx=1e-06 #Blastx eval cutoff
        bit_blastx=30 #Blastx bit cutoff
        depth_blastx=0 #Blastx depth cutoff (0 to disable cutoff)

        pcov_tblastx=0.8 #tBlastx Percent Coverage Threhold alt-EST-Genome Alignments
        pid_tblastx=0.85 #tBlastx Percent Identity Threshold alt-EST-Genome Aligments
        eval_tblastx=1e-10 #tBlastx eval cutoff
        bit_tblastx=40 #tBlastx bit cutoff
        depth_tblastx=0 #tBlastx depth cutoff (0 to disable cutoff)

        pcov_rm_blastx=0.5 #Blastx Percent Coverage Threhold For Transposable Element Masking
        pid_rm_blastx=0.4 #Blastx Percent Identity Threshold For Transposbale Element Masking
        eval_rm_blastx=1e-06 #Blastx eval cutoff for transposable element masking
        bit_rm_blastx=30 #Blastx bit cutoff for transposable element masking

        ep_score_limit=20 #Exonerate protein percent of maximal score threshold
        en_score_limit=20 #Exonerate nucleotide percent of maximal score threshold
        
maker_opts.ctl * this is the options file and should be adjusted so maker can call your input files 
Example:

        #-----Genome (these are always required)
        genome= Joinvillea_ascendens_subsp._gabra.faa_1.1
        organism_type=eukaryotic #eukaryotic or prokaryotic. Default is eukaryotic
        #-----Re-annotation Using MAKER Derived GFF3
        maker_gff= #MAKER derived GFF3 file
        est_pass=0 #use ESTs in maker_gff: 1 = yes, 0 = no
        altest_pass=0 #use alternate organism ESTs in maker_gff: 1 = yes, 0 = no
        protein_pass=0 #use protein alignments in maker_gff: 1 = yes, 0 = no
        rm_pass=0 #use repeats in maker_gff: 1 = yes, 0 = no
        model_pass=0 #use gene models in maker_gff: 1 = yes, 0 = no
        pred_pass=0 #use ab-initio predictions in maker_gff: 1 = yes, 0 = no
        other_pass=0 #passthrough anyything else in maker_gff: 1 = yes, 0 = no
        #-----EST Evidence (for best results provide a file for at least one)
        est= #set of ESTs or assembled mRNA-seq in fasta format
        altest= Streptochaeta_maker_max_transcripts_V1.fasta.mod.fa #EST/cDNA sequence file in fasta format from an alternate organism
        est_gff= #aligned ESTs or mRNA-seq from an external GFF3 file
        altest_gff= #aligned ESTs from a closly relate species in GFF3 format
        #-----Protein Homology Evidence (for best results provide a file for at least one)
        protein= combined_fasta #protein sequence file in fasta format (i.e. from mutiple oransisms)
        protein_gff=  #aligned protein homology evidence from an external GFF3 file
        #-----Repeat Masking (leave values blank to skip repeat masking)
        model_org=all #select a model organism for RepBase masking in RepeatMasker
        rmlib= #provide an organism specific repeat library in fasta format for RepeatMasker
        repeat_protein= #provide a fasta file of transposable element proteins for RepeatRunner
        rm_gff= #pre-identified repeat elements from an external GFF3 file
        prok_rm=0 #forces MAKER to repeatmask prokaryotes (no reason to change this), 1 = yes, 0 = no
        softmask=1 #use soft-masking rather than hard-masking in BLAST (i.e. seg and dust filtering)
        #-----Gene Prediction
        snaphmm= #SNAP HMM file
        gmhmm= #GeneMark HMM file
        augustus_species= arabidopsis #Augustus gene prediction species model
        fgenesh_par_file= #FGENESH parameter file
        pred_gff= #ab-initio predictions from an external GFF3 file
        model_gff= #annotated gene models from an external GFF3 file (annotation pass-through)
        est2genome=0 #infer gene predictions directly from ESTs, 1 = yes, 0 = no
        protein2genome=1 #infer predictions from protein homology, 1 = yes, 0 = no
        trna=0 #find tRNAs with tRNAscan, 1 = yes, 0 = no
        snoscan_rrna= #rRNA file to have Snoscan find snoRNAs
        unmask=0 #also run ab-initio prediction programs on unmasked sequence, 1 = yes, 0 = no
        #-----Other Annotation Feature Types (features MAKER doesn't recognize)
        other_gff= #extra features to pass-through to final MAKER generated GFF3 file
        #-----External Application Behavior Options
        alt_peptide=C #amino acid used to replace non-standard amino acids in BLAST databases
        cpus=1 #max number of cpus to use in BLAST and RepeatMasker (not for MPI, leave 1 when using MPI)
        #-----MAKER Behavior Options
        max_dna_len=100000 #length for dividing up contigs into chunks (increases/decreases memory usage)
        min_contig=1 #skip genome contigs below this length (under 10kb are often useless)
        pred_flank=200 #flank for extending evidence clusters sent to gene predictors
        pred_stats=0 #report AED and QI statistics for all predictions as well as models
        AED_threshold=1 #Maximum Annotation Edit Distance allowed (bound by 0 and 1)
        min_protein=0 #require at least this many amino acids in predicted proteins
        alt_splice=1 #Take extra steps to try and find alternative splicing, 1 = yes, 0 = no
        always_complete=1 #extra steps to force start and stop codons, 1 = yes, 0 = no
        map_forward=0 #map names and attributes forward from old GFF3 genes, 1 = yes, 0 = no
        keep_preds=0 #Concordance threshold to add unsupported gene prediction (bound by 0 and 1)
        split_hit=10000 #length for the splitting of hits (expected max intron size for evidence alignments)
        single_exon=0 #consider single exon EST evidence when generating annotations, 1 = yes, 0 = no
        single_length=250 #min length required for single exon ESTs if 'single_exon is enabled'
        correct_est_fusion=0 #limits use of ESTs in annotation to avoid fusion genes
        tries=2 #number of times to try a contig if there is a failure for some reason
        clean_try=1 #remove all data from previous run before retrying, 1 = yes, 0 = no
        clean_up=1 #removes theVoid directory with individual analysis files, 1 = yes, 0 = no
        TMP= #specify a directory other than the system default temporary directory for temporary files
        
