Trimmomatic
============

`Trimmomatic <http://www.usadellab.org/cms/?page=trimmomatic>`__ is a tool developed to trim poor quality and adaptors from Illumina data. If you do not have Trimmomatic installed, please check :doc:`installsoftware`. 

Let's check and record the version we are using by typing:

.. code-block:: bash
    
    trimmomatic -version

.. code-block:: bash

    0.39

Here, we are analysing paired-end NGS data. Therefore, in this case, Trimmomatic uses as input the forward (ending in ``_1``) and reverse (ending in ``_2``) reads and outputs four files: forward paired, forward unpaired, reverse paired and reverse unpaired. In the paired output files we will have the reads for which both mates survived the trimming, and in the unpaired ones we will have the reads for which only one of the mates survived. Here is an example of a Trimmomatic command for a single accession: 


.. code-block:: bash

    trimmomatic PE -phred33 FG35_1.fastq.gz FG35_2.fastq.gz FG35_forward_paired.fq.gz FG35_forward_unpaired.fq.gz FG35_reverse_paired.fq.gz FG35_reverse_unpaired.fq.gz ILLUMINACLIP:/path/to/the/adaptor/sequence/file/e.g./TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 

Let's go through each part of this long command, with adapted information from the manual:

1. ``PE``: indicated our data is paired-end

2. ``-phred33``: -phred33 or -phred64 specifies the base quality encoding. If no quality encoding is specified, it will be determined automatically (since version 0.32). In our case, if your data is yourger than 5 years-old, it is very likely that it is following Phred+33. Here is more information about it: `<https://www.drive5.com/usearch/manual/quality_score.html>`_.  
   
3. ``ILLUMINACLIP:TruSeq3-PE.fa:2:30:10`` will use the file TruSeq3-PE.fa to remove adapters. Depending on how you have installed Trimmomatic, this file will be in different places. An alternative is to download it directly from their github page: `<https://github.com/usadellab/Trimmomatic>`_. You can see that they have several adapter files available, and the correct one will depend on which type of data you have. From their manual: `Suggested adapter sequences are provided for TruSeq2 (as used in GAII machines) and TruSeq3 (as used by HiSeq and MiSeq machines), for both single-end and paired-end mode. These sequences have not been extensively tested, and depending on specific issues which may occur in library preparation, other sequences may work better for a given dataset.` And see some notes from Illumina here `<https://support.illumina.com/bulletins/2016/12/what-sequences-do-i-use-for-adapter-trimming.html>`_.Once you downloaded it, you can take a quick look to see what it looks like with ``cat TruSeq3-PE.fa`` for example.

4. ``LEADING:3`` removes leading low quality or N bases (below quality 3). 

5. ``TRAILING:3`` removes trailing low quality or N bases (below quality 3).

6. ``SLIDINGWINDOW:4:15`` scans the read with a 4-base wide sliding window, cutting when the average quality per base drops below 15. 

7. ``MINLEN:36`` drops reads below the 36 bases long.

To save time, here is the same command but using a loop and our ``acc`` file: 

.. code-block:: bash

    while read f ; do
    trimmomatic PE -phred33 "$f"_1.fastq.gz "$f"_2.fastq.gz "$f"_forward_paired.fq.gz "$f"_forward_unpaired.fq.gz "$f"_reverse_paired.fq.gz "$f"_reverse_unpaired.fq.gz ILLUMINACLIP:/path/to/the/adaptor/sequence/file/e.g./TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 ; done < acc


Don't forget to read the manual for more details and options: `<http://www.usadellab.org/cms/?page=trimmomatic>`_.

This is what you should have in your folder after Trimmomatic finishes:

.. code-block:: bash

    FG113                           FG113_reverse_unpaired.fq.gz    FG35_reverse_paired.fq.gz       FGIntype_forward_unpaired.fq.gz KGD465_forward_paired.fq.gz     zygia917_1_fastqc.zip
    FG113_1.fastq.gz                FG35                            FG35_reverse_unpaired.fq.gz     FGIntype_reverse_paired.fq.gz   KGD465_forward_unpaired.fq.gz   zygia917_2.fastq.gz
    FG113_1_fastqc.html             FG35_1.fastq.gz                 FGIntype                        FGIntype_reverse_unpaired.fq.gz KGD465_reverse_paired.fq.gz     zygia917_2_fastqc.html
    FG113_1_fastqc.zip              FG35_1_fastqc.html              FGIntype_1.fastq.gz             KGD465                          KGD465_reverse_unpaired.fq.gz   zygia917_2_fastqc.zip
    FG113_2.fastq.gz                FG35_1_fastqc.zip               FGIntype_1_fastqc.html          KGD465_1.fastq.gz               acc                             zygia917_forward_paired.fq.gz
    FG113_2_fastqc.html             FG35_2.fastq.gz                 FGIntype_1_fastqc.zip           KGD465_1_fastqc.html            fastqcfiles                     zygia917_forward_unpaired.fq.gz
    FG113_2_fastqc.zip              FG35_2_fastqc.html              FGIntype_2.fastq.gz             KGD465_1_fastqc.zip             renaming.sh                     zygia917_reverse_paired.fq.gz
    FG113_forward_paired.fq.gz      FG35_2_fastqc.zip               FGIntype_2_fastqc.html          KGD465_2.fastq.gz               zygia917                        zygia917_reverse_unpaired.fq.gz
    FG113_forward_unpaired.fq.gz    FG35_forward_paired.fq.gz       FGIntype_2_fastqc.zip           KGD465_2_fastqc.html            zygia917_1.fastq.gz
    FG113_reverse_paired.fq.gz      FG35_forward_unpaired.fq.gz     FGIntype_forward_paired.fq.gz   KGD465_2_fastqc.zip             zygia917_1_fastqc.html


Let's take a quick look in one of the output files. The ``.gz`` extension indicated that those files are compressed. We can use ``zless`` in this case: 

.. code-block:: bash

    zless FG35_forward_paired.fq.gz

.. code-block:: bash

    @HWI-M01145:124:000000000-A78G3:1:1101:15521:1338 1:N:0:ACAGTG
    GTTCATACAACACATAACTCAGAAAAATGGATTCAGCTGTTCACCTCCGCCAATACAAATTGATAAATGAAGAATTCTGCATCAGAATGAAAGTATGAAATTACATTTTAGAAGGGTGTTACCTCAGATTGAGTACACGCAGCAGTTGCAAGGACTTCCCAATTCCAAAAGACAGATGGGAATCACTCAAAGGCCTCCGCTTAAAGTCAGATAGCCATCC
    TGCTATCTACAAGTGTGTGAAACTTTACGG
    +
    CCCCCFFFFFFDGGGGGGGGGGHHHHGHHHHHHHHHHHHHHHHHHHHHGGGGGHHHHHHHHHHHGHHHHHHHHHHHHHHHGHHHHHHHHHHHHHGHHHGHHHHHGHHHHHHHHHHGGFGGHHHHHHHFHHHGHHHHHHGGGGGHHHHHHHHHGHHHHHHHHHHHHHHHHHHGHHHFHHHHHHHHHHHHHHHHHHHHHHGGGGGHHHHHHGHHGHGHHHGHHHHHHHHHHHHHGFHFFFGGGGGGGGGGGG
    @HWI-M01145:124:000000000-A78G3:1:1101:16121:1366 1:N:0:ACAGTG
    CAGTCAACTGATCTCCCAACAATATTGAAGTGCCAAAAGTGATAGCCGAAACAAATATAGGAGAGCTCCAAAAGATAAAAGTGATGAAAGCCTGAGAATAGAGGGCTTTACGTAGCCACTTGAACTCTACTCCACGCATTTCTTCCAGCTGTATTCGATATCGATCTTCCCAAGCTTGCAACTTGAGGATCCTCATATTTCTCAAGCATTCAGATGTTTTTCTCATCCTTTCATCCTTAGCAGCCATTAA
    +
    BBCACFFFFFFFGGGGGGGGGGHHHHHHHHHHHHHGHHHHHHHHHHHGGGGGHGHHHHHHHHHHGHHHHHHHHHGHHHHHHHGHHHGHHHHHHHHHHHHHHHHHGGHHHHHGGHHEHHHHHHHHHHHHHHHHHHHGGGGGHHHHHHHHHHHHHHHHHGHGGHGHHHHHHHHHHHGGHHGHHGHHGFHHGHHGGHHHHHHHHHGHHHGHGHHHHHHHHHHGGHHHHHHHHHHHHHFHGHGGGGGGGGGGFF


Each read has four lines of information, so here we see two reads. In the first line beginning with ``@`` we see information from the sequencing machine (in this case information about the flowcell in a Illumina machine), the nucleotide sequence is stored on the second line, the third line has a ``+`` and the fourth line stores the quality scores.

Now, let's run FastQC on the groups of reads output from Trimmomatic. First let's create a file with the names of Trimmomatic output:

.. code-block:: bash

    ls *fq.gz > fastqctrimfiles


.. code-block:: bash

    cat fastqctrimfiles

.. code-block:: bash

    FG113_forward_paired.fq.gz
    FG113_forward_unpaired.fq.gz
    FG113_reverse_paired.fq.gz
    FG113_reverse_unpaired.fq.gz
    FG35_forward_paired.fq.gz
    FG35_forward_unpaired.fq.gz
    FG35_reverse_paired.fq.gz
    FG35_reverse_unpaired.fq.gz
    FGIntype_forward_paired.fq.gz
    FGIntype_forward_unpaired.fq.gz
    FGIntype_reverse_paired.fq.gz
    FGIntype_reverse_unpaired.fq.gz
    KGD465_forward_paired.fq.gz
    KGD465_forward_unpaired.fq.gz
    KGD465_reverse_paired.fq.gz
    KGD465_reverse_unpaired.fq.gz
    zygia917_forward_paired.fq.gz
    zygia917_forward_unpaired.fq.gz
    zygia917_reverse_paired.fq.gz
    zygia917_reverse_unpaired.fq.gz

And run:

.. code-block:: bash
    
    while read f; do fastqc "$f" ; done < fastqctrimfiles

Once it is done, inspect the ``html`` files visually to check the impact of trimming (and whether files look good to carry on if you are running this analysis with your own data). We are now going to the next step, further trimming with `Cutadapt <https://cutadapt.readthedocs.io/en/stable/>`__.