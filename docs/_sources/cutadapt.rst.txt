Cutadapt
==========

We can use `Cutadapt <https://cutadapt.readthedocs.io/en/stable/>`__ to trim further our output and the script ``more_tidying.sh``. 

.. note:: Remember we are following the Hyb_baits_pipeline from the paper `Nicholls et al. 2015 <https://www.frontiersin.org/articles/10.3389/fpls.2015.00710/full>`_, with scripts available on github: `<https://github.com/ckidner/Targeted_enrichment>`_.


.. dropdown:: You can see the content of the script more_tidying.sh here: 
    :color: info

    .. code-block:: bash

        #! /bin/bash -x
        # to trim the trimmomatic output one more time
        # Needs ck_empties.sh and ck_remove.sh
        #Catherine Kidner 3 Nov 2014


        echo "Hello world"

        acc=$1

        echo "You're working on accession $1"


        fwd_p=${acc}_forward_paired.fq.gz
        rev_p=${acc}_reverse_paired.fq.gz
        fwd_un_p=${acc}_forward_unpaired.fq.gz
        rev_un_p=${acc}_reverse_unpaired.fq.gz

        fwd_p_done=${acc}_trimmed_1.fastq
        rev_p_done=${acc}_trimmed_2.fastq
        fwd_u_done=${acc}_trimmed_1u.fastq
        rev_u_done=${acc}_trimmed_2u.fastq

        cutadapt -a AGATCGGAAGAGC $fwd_p > $fwd_p_done 2>> cut_out
        cutadapt -a AGATCGGAAGAGC $rev_p > $rev_p_done 2>> cut_out
        cutadapt -a AGATCGGAAGAGC $fwd_un_p > $fwd_u_done 2>> cut_out
        cutadapt -a AGATCGGAAGAGC $rev_un_p > $rev_u_done 2>> cut_out
        ./ck_empties.sh $acc
        ./ck_remove.sh $acc


        exit 0


You can see in the top commented section of the script that it requires two other scripts: ``# Needs ck_empties.sh and ck_remove.sh``, also available in GitHub, so make sure you have the correct path for ``ck_empties.sh`` and ``ck_remove.sh`` the scripts inside ``more_tidying.sh``. We will use ``more_tidying.sh`` in a loop with out trimmomatic output as input:

.. code-block:: bash

    while read f ; do ./more_tidying.sh "$f" ; done < acc

This script will produce four outputs ending in ``_trimmed_1.fastq`` (forward paired), ``_trimmed_2.fastq`` (reverse paired), ``_trimmed_1u.fastq`` (forward unpaired) and ``_trimmed_2u.fastq`` (reverse unpaired).

Here is what we have in our folder now:

.. code-block:: bash

    ls

.. code-block:: bash

    FG113                                 FG35_1.fastq.gz                       FGIntype_1_fastqc.zip                 KGD465_2_fastqc.html                  zygia917_1.fastq.gz
    FG113.empties                         FG35_1_fastqc.html                    FGIntype_2.fastq.gz                   KGD465_2_fastqc.zip                   zygia917_1_fastqc.html
    FG113_1.fastq.gz                      FG35_1_fastqc.zip                     FGIntype_2_fastqc.html                KGD465_forward_paired.fq.gz           zygia917_1_fastqc.zip
    FG113_1_fastqc.html                   FG35_2.fastq.gz                       FGIntype_2_fastqc.zip                 KGD465_forward_paired_fastqc.html     zygia917_2.fastq.gz
    FG113_1_fastqc.zip                    FG35_2_fastqc.html                    FGIntype_forward_paired.fq.gz         KGD465_forward_paired_fastqc.zip      zygia917_2_fastqc.html
    FG113_2.fastq.gz                      FG35_2_fastqc.zip                     FGIntype_forward_paired_fastqc.html   KGD465_forward_unpaired.fq.gz         zygia917_2_fastqc.zip
    FG113_2_fastqc.html                   FG35_forward_paired.fq.gz             FGIntype_forward_paired_fastqc.zip    KGD465_forward_unpaired_fastqc.html   zygia917_forward_paired.fq.gz
    FG113_2_fastqc.zip                    FG35_forward_paired_fastqc.html       FGIntype_forward_unpaired.fq.gz       KGD465_forward_unpaired_fastqc.zip    zygia917_forward_paired_fastqc.html
    FG113_forward_paired.fq.gz            FG35_forward_paired_fastqc.zip        FGIntype_forward_unpaired_fastqc.html KGD465_reverse_paired.fq.gz           zygia917_forward_paired_fastqc.zip
    FG113_forward_paired_fastqc.html      FG35_forward_unpaired.fq.gz           FGIntype_forward_unpaired_fastqc.zip  KGD465_reverse_paired_fastqc.html     zygia917_forward_unpaired.fq.gz
    FG113_forward_paired_fastqc.zip       FG35_forward_unpaired_fastqc.html     FGIntype_reverse_paired.fq.gz         KGD465_reverse_paired_fastqc.zip      zygia917_forward_unpaired_fastqc.html
    FG113_forward_unpaired.fq.gz          FG35_forward_unpaired_fastqc.zip      FGIntype_reverse_paired_fastqc.html   KGD465_reverse_unpaired.fq.gz         zygia917_forward_unpaired_fastqc.zip
    FG113_forward_unpaired_fastqc.html    FG35_reverse_paired.fq.gz             FGIntype_reverse_paired_fastqc.zip    KGD465_reverse_unpaired_fastqc.html   zygia917_reverse_paired.fq.gz
    FG113_forward_unpaired_fastqc.zip     FG35_reverse_paired_fastqc.html       FGIntype_reverse_unpaired.fq.gz       KGD465_reverse_unpaired_fastqc.zip    zygia917_reverse_paired_fastqc.html
    FG113_reverse_paired.fq.gz            FG35_reverse_paired_fastqc.zip        FGIntype_reverse_unpaired_fastqc.html KGD465_trimmed_1.fastq                zygia917_reverse_paired_fastqc.zip
    FG113_reverse_paired_fastqc.html      FG35_reverse_unpaired.fq.gz           FGIntype_reverse_unpaired_fastqc.zip  KGD465_trimmed_1.fastq.gz             zygia917_reverse_unpaired.fq.gz
    FG113_reverse_paired_fastqc.zip       FG35_reverse_unpaired_fastqc.html     FGIntype_trimmed_1.fastq              KGD465_trimmed_1u.fastq               zygia917_reverse_unpaired_fastqc.html
    FG113_reverse_unpaired.fq.gz          FG35_reverse_unpaired_fastqc.zip      FGIntype_trimmed_1.fastq.gz           KGD465_trimmed_2.fastq                zygia917_reverse_unpaired_fastqc.zip
    FG113_reverse_unpaired_fastqc.html    FG35_trimmed_1.fastq                  FGIntype_trimmed_1u.fastq             KGD465_trimmed_2.fastq.gz             zygia917_trimmed_1.fastq
    FG113_reverse_unpaired_fastqc.zip     FG35_trimmed_1.fastq.gz               FGIntype_trimmed_2.fastq              KGD465_trimmed_2u.fastq               zygia917_trimmed_1.fastq.gz
    FG113_trimmed_1.fastq                 FG35_trimmed_1u.fastq                 FGIntype_trimmed_2.fastq.gz           acc                                   zygia917_trimmed_1u.fastq
    FG113_trimmed_1.fastq.gz              FG35_trimmed_2.fastq                  FGIntype_trimmed_2u.fastq             cut_out                               zygia917_trimmed_2.fastq
    FG113_trimmed_1u.fastq                FG35_trimmed_2.fastq.gz               KGD465                                fastqcfiles                           zygia917_trimmed_2.fastq.gz
    FG113_trimmed_2.fastq                 FG35_trimmed_2u.fastq                 KGD465.empties                        fastqctrimfile                        zygia917_trimmed_2u.fastq
    FG113_trimmed_2.fastq.gz              FGIntype                              KGD465_1.fastq.gz                     more_tidying.sh
    FG113_trimmed_2u.fastq                FGIntype.empties                      KGD465_1_fastqc.html                  renaming.sh
    FG35                                  FGIntype_1.fastq.gz                   KGD465_1_fastqc.zip                   zygia917
    FG35.empties                          FGIntype_1_fastqc.html                KGD465_2.fastq.gz                     zygia917.empties

.. tip::
      In case you are doing your anaylisis in `Crop Diversity HPC <https://help.cropdiversity.ac.uk/index.html>`__, you can run this step as an array. An array job is a job in which the script is run concomitantly for each sample and it will be much quicker. In this link you can find more about array jobs\: `<https://help.cropdiversity.ac.uk/slurm-overview.html#array-jobs>`__. Below an example of how to run the ``more_tidying.sh`` script as an array:

      .. code-block::

            #!/bin/bash

            # to trim the trimmomatic output one more time
            # Needs ck_empties.sh and ck_remove.sh
            # Catherine Kidner 3 Nov 2014
            # Adjusted for an array job in Mar 2022, Flavia Pezzini

            #SBATCH --job-name="cutadapt"
            #SBATCH --export=ALL
            #SBATCH --mail-user=youremail@yourdomain # enter your email to receive a message once it is done.
            #SBATCH --mail-type=END,FAIL
            #SBATCH --output ./slurm-%x-%A_%a.out # %x gives job name, %A job ID, %a array index 
            #SBATCH --partition=long
            #SBATCH --cpus-per-task=16 #number of threads, not cores
            #SBATCH --mem=1G #adjust this according to your data.
            #SBATCH --array=0-4 # the number of samples you have. We have five accessions we use 0-4 because Bash array is zero-indexed (instead of 1-5).


            acc=$(sed -n "$SLURM_ARRAY_TASK_ID"p /path/to/my/acc/file)

            echo "Hello world"

            echo "You're working on accession $acc"

            fwd_p=${acc}_forward_paired.fq
            rev_p=${acc}_reverse_paired.fq
            fwd_un_p=${acc}_forward_unpaired.fq
            rev_un_p=${acc}_reverse_unpaired.fq

            fwd_p_done=${acc}_trimmed_1.fastq
            rev_p_done=${acc}_trimmed_2.fastq
            fwd_u_done=${acc}_trimmed_1u.fastq
            rev_u_done=${acc}_trimmed_2u.fastq

            cutadapt -a AGATCGGAAGAGC $fwd_p > $fwd_p_done 2>> cut_out
            cutadapt -a AGATCGGAAGAGC $rev_p > $rev_p_done 2>> cut_out
            cutadapt -a AGATCGGAAGAGC $fwd_un_p > $fwd_u_done 2>> cut_out
            cutadapt -a AGATCGGAAGAGC $rev_un_p > $rev_u_done 2>> cut_out
            ./ck_empties.sh $acc
            ./ck_remove.sh $acc

            exit 0


      To submit just type: ``sbatch more_tidying_array.sh``