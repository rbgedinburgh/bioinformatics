Raw reads 
==========================

.. important::
    Never work directly on your raw data. Make a copy of it to a working directory (i.e. where you will run your analysis) and keep backup copies of your raw data elsewhere (see :doc:`wheretostore`).

For this tutorial, we will follow the Hyb_baits_pipeline from the paper `Nicholls et al. 2015 <https://www.frontiersin.org/articles/10.3389/fpls.2015.00710/full>`_, with scripts available on github: `<https://github.com/ckidner/Targeted_enrichment>`_. We will use five accessions used in the manuscript, available in European Nucleotide Archive, study ID ERP009747: `<https://www.ebi.ac.uk/ena/browser/view/PRJEB8722?show=reads>`_. 


Here we have a list of five folders that came from the sequencing facility. The name of the folders represent the name of the accessions we are working with. Inside each folder are the forward and reverse reads of the accessions. Let’s see what their names are:

.. code-block:: bash

    ls FG35/

.. code-block:: bash
 
    140428_M01145_0124_000000000-A78G3_1_IL-TP-005_1.sanfastq.gz
    140428_M01145_0124_000000000-A78G3_1_IL-TP-005_2.sanfastq.gz

The files have complicated names. Let's change their names to more meaniful ones (i.e., the name of the accesions, which is the same name of their folders): 

Let's start by making a list of folders we have in our working directory:

.. code-block:: bash

    find . -type d 

This command is saying find here (the dot ``.``) all the directories ``-type d``. Here is what you will see in your terminal:

.. code-block:: bash

    ./FGIntype
    ./KGD465
    ./FG35
    ./zygia917
    ./FG113   

Let's delete the symbols that we don't need ( `.` and `/` ). We will use a pipe ``|`` and the command ``cut``:


.. code-block:: bash

    find . -type d | cut -c 3-

.. code-block:: bash

    FG113
    FG35
    FGIntype
    KGD465
    zygia917

Now let's save their names in a file. The symbol ``>`` will redirect our output to a file called ``acc``:

.. code-block:: bash

    find . -type d | cut -c 3- > acc

Take a look at ``acc``:

.. code-block:: bash

    cat acc

.. code-block:: bash

    FG113
    FG35
    FGIntype
    KGD465
    zygia917

We will now use the script ``renaming.sh`` to rename our raw read and move them to the folder we are working on.


.. dropdown:: You can see the script in this dropdown menu. The file is also in the GitHub repository:
    :container: + shadow
    :title: text-info 
    
        .. code-block:: bash

            #! /bin/bash -x
            # Renaming the files by the folder names, getting them out of the folders
            # For dealing with sequencing facility output

            # In directory with the folders in make a list of folder names, to run through this on a while loop

            acc=$1

            input1=${acc}/*_1.sanfastq.gz
            output1=${acc}_1.fastq.gz

            input2=${acc}/*_2.sanfastq.gz
            output2=${acc}_2.fastq.gz

            mv $input1 $output1
            mv $input2 $output2

.. code-block:: bash

    ./renaming.sh FG35

If you have many samples, this task can take a while. Let's use the script ``renaming.sh`` but in a loop in order to change the names of all the accessions with a single command. Loops are nice to automate repetitive tasks:

.. code-block:: bash

    while read f; do ./renaming.sh "$f"; done < acc

.. note::

    In this loop, the ``acc`` is the input file that will be read one line at a time, and that value replaces the variable ``$f``. So if our ``acc`` files has five lines, the command ``./renaming.sh`` will be executed five times, each time replacing ``$f`` with a line from our input file ``acc``. 

.. dropdown:: This is what you should see printed in your terminal:

    .. code-block:: bash
    
        + acc=FG113
        + input1='FG113/*_1.sanfastq.gz'
        + output1=FG113_1.fastq.gz
        + input2='FG113/*_2.sanfastq.gz'
        + output2=FG113_2.fastq.gz
        + mv FG113/140428_M01145_0124_000000000-A78G3_1_IL-TP-019_1.sanfastq.gz FG113_1.fastq.gz
        + mv FG113/140428_M01145_0124_000000000-A78G3_1_IL-TP-019_2.sanfastq.gz FG113_2.fastq.gz
        + acc=FG35
        + input1='FG35/*_1.sanfastq.gz'
        + output1=FG35_1.fastq.gz
        + input2='FG35/*_2.sanfastq.gz'
        + output2=FG35_2.fastq.gz
        + mv FG35/140428_M01145_0124_000000000-A78G3_1_IL-TP-005_1.sanfastq.gz FG35_1.fastq.gz
        + mv FG35/140428_M01145_0124_000000000-A78G3_1_IL-TP-005_2.sanfastq.gz FG35_2.fastq.gz
        + acc=FGIntype
        + input1='FGIntype/*_1.sanfastq.gz'
        + output1=FGIntype_1.fastq.gz
        + input2='FGIntype/*_2.sanfastq.gz'
        + output2=FGIntype_2.fastq.gz
        + mv FGIntype/140428_M01145_0124_000000000-A78G3_1_IL-TP-009_1.sanfastq.gz FGIntype_1.fastq.gz
        + mv FGIntype/140428_M01145_0124_000000000-A78G3_1_IL-TP-009_2.sanfastq.gz FGIntype_2.fastq.gz
        + acc=KGD465
        + input1='KGD465/*_1.sanfastq.gz'
        + output1=KGD465_1.fastq.gz
        + input2='KGD465/*_2.sanfastq.gz'
        + output2=KGD465_2.fastq.gz
        + mv KGD465/140428_M01145_0124_000000000-A78G3_1_IL-TP-020_1.sanfastq.gz KGD465_1.fastq.gz
        + mv KGD465/140428_M01145_0124_000000000-A78G3_1_IL-TP-020_2.sanfastq.gz KGD465_2.fastq.gz
        + acc=zygia917
        + input1='zygia917/*_1.sanfastq.gz'
        + output1=zygia917_1.fastq.gz
        + input2='zygia917/*_2.sanfastq.gz'
        + output2=zygia917_2.fastq.gz
        + mv zygia917/140428_M01145_0124_000000000-A78G3_1_IL-TP-027_1.sanfastq.gz zygia917_1.fastq.gz
        + mv zygia917/140428_M01145_0124_000000000-A78G3_1_IL-TP-027_2.sanfastq.gz zygia917_2.fastq.gz


Now let's see what we have in our working directory:

.. code-block:: bash

    ls

.. code-block:: bash

    FG113               FG35_2.fastq.gz     KGD465_1.fastq.gz   zygia917
    FG113_1.fastq.gz    FGIntype            KGD465_2.fastq.gz   zygia917_1.fastq.gz
    FG113_2.fastq.gz    FGIntype_1.fastq.gz acc                 zygia917_2.fastq.gz
    FG35                FGIntype_2.fastq.gz FG35_1.fastq.gz     KGD465              renaming.sh

The raw reads are in ``fastq.gz`` format. ``fastq`` is a text-based format for storing both a biological sequence (usually nucleotide sequence) and its corresponding quality scores. The ``gz`` means the files are compressed. We can peek into compressed files with ``zless``. Let's look into the foward reads (forward reads end with ``_1`` in our case) of the accession FG113.

.. code-block:: bash

    zless FG113_1.fastq.gz

.. code-block:: bash

    @HWI-M01145:124:000000000-A78G3:1:1101:15884:1359 1:N:0:GTGAAA
    AATCTAAAACCACCTCAATAAATGTAATCTTCATCTATCTTATCTATTTAAAGTCCTTATTATTCCTTTTGTTTCTTCAAATCCAGCTCCCTCTTCTCTCCTTTCTCAAGTATCAATATGTTGAGATGCATCAATACTTCTTCAGTTCGTTCTCCTTTTCCTCTTCCGCCTCTACAGCCTCTACCGCATCTACCTCCTCTACTGCAACCGTCAGAGTGACAAGTTCTTGACCACCTCAATGCGAGGGTGG
    +
    BBBBBFFFFFABGGGGGGGGGGGHHFAFAFGFGHHHHHHHHHGHHHHHHHHHHGGHHHHHHHHHHHHHHHHHHHHHHHGHHHHGHHGHHHGHHHHHGHHHHHHHHHHGHHFHHHHHHHHHHHGFHHGHHGHGHHHHHGHHHHHHHHHHGGHHGHHHHHHHHHHHHHHGGGGGHHHHGHHHHHHHHGGGGGHHHHHHHHHHHHHHHHHHHGGGGGGHFHHHGHHHHHHHHHHHHHGHHHHHHHGGGGGE?D
    @HWI-M01145:124:000000000-A78G3:1:1101:14180:1422 1:N:0:GTGAAA
    TTGAGATCAACTTGGTCATCCTTGTCGCGTCCCAATATCATCTTATAAACAATATATCTTCTAAAAGCCCATGCATTTTCTCACTAACGTCCACAATCTCACCCATCTTAGCAGCATTATCCAGTGACTTCACCACCATTTTCAACTCCTCCGATCCCTCCTAAGATGACCAAACGGCTCTTGATTTGAAGCACCAAAAAGATGTTGGAGGCTGAGTTTCTTCATGTCACGCAAGTATGAACCATGCCCA
    +
    >AAAAAFFFF1DEGGFGFFFGG311FEEAFGEA1FGGHHDEGGEAEHF2GDCFHHHHHHFHHBAHFGFFEHHBF1AGHHH12GHHHHFCFEFEFHHFGHHGHFGGGHHHHGHBGEGHHHHHGHH2GFFFGGFHGGHGHHHHHHHHGHHFFHHFCAC/0FFECGHH1GD1FGFH/G<EC?CHHD<GGHFBFGFGHEFC..<A.D<GDCE0C?.GFHHHFHHHHBGHFF09FB?@AG0FFGFFFAABFFGGE

To exit ``zless`` just type ``q``.

Each read has four lines of information, so here we see two reads. In the first line beginning with ``@`` we see information from the sequencing machine (in this case information about the flowcell in a Illumina machine), the nucleotide sequence is stored on the second line, the third line has a ``+`` and the fourth line stores the quality scores.  



We are ready to the next step. Let's run :doc:`fastqc` on them. 