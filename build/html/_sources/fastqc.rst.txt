FastQC
=======

We have our raw reads renamed and in our main working directory. We will run `FastQC <https://www.bioinformatics.babraham.ac.uk/projects/fastqc/>`__ to check the reads quality. This analysis is quick and we will inspect the results visually. 

If we type:

.. code-block:: bash

    ls *.gz

.. code-block:: bash

    FG113_1.fastq.gz    FG113_2.fastq.gz    FG35_1.fastq.gz     FG35_2.fastq.gz     FGIntype_1.fastq.gz FGIntype_2.fastq.gz KGD465_1.fastq.gz   KGD465_2.fastq.gz   zygia917_1.fastq.gz zygia917_2.fastq.gz

We can include the name of all those files in a name file:

.. code-block:: bash

    ls *.gz > fastqcfiles

Now take a look on what is inside the ``fastqcfiles``:

.. code-block:: bash

    cat fastqcfiles

.. code-block:: bash

    FG113_1.fastq.gz
    FG113_2.fastq.gz
    FG35_1.fastq.gz
    FG35_2.fastq.gz
    FGIntype_1.fastq.gz
    FGIntype_2.fastq.gz
    KGD465_1.fastq.gz
    KGD465_2.fastq.gz
    zygia917_1.fastq.gz
    zygia917_2.fastq.gz



Now let's run FastQC on all the files using a loop:

.. note:: If you don't have FastQC installed, please take a look in :doc:`installsoftware` 

Let's check and take note of the version we are using:

.. code-block:: bash

    fastqc -v

.. code-block:: bash
    
    FastQC v0.11.9

Using ``-h`` or ``--help`` we can check how FastQC works and the options available.

.. code-block:: bash

    fastqc -h

And finally run the loop:

.. code-block:: bash
    
    while read f; do fastqc "$f" ; done < fastqcfiles


Let's take a look in the output:

.. code-block:: bash

    FG113                  FG113_2_fastqc.html    FG35_1_fastqc.zip      FGIntype_1.fastq.gz    FGIntype_2_fastqc.zip  KGD465_2.fastq.gz      renaming.sh            zygia917_2.fastq.gz
    FG113_1.fastq.gz       FG113_2_fastqc.zip     FG35_2.fastq.gz        FGIntype_1_fastqc.html KGD465                 KGD465_2_fastqc.html   zygia917               zygia917_2_fastqc.html
    FG113_1_fastqc.html    FG35                   FG35_2_fastqc.html     FGIntype_1_fastqc.zip  KGD465_1.fastq.gz      KGD465_2_fastqc.zip    zygia917_1.fastq.gz    zygia917_2_fastqc.zip
    FG113_1_fastqc.zip     FG35_1.fastq.gz        FG35_2_fastqc.zip      FGIntype_2.fastq.gz    KGD465_1_fastqc.html   acc                    zygia917_1_fastqc.html
    FG113_2.fastq.gz       FG35_1_fastqc.html     FGIntype               FGIntype_2_fastqc.html KGD465_1_fastqc.zip    fastqcfiles            zygia917_1_fastqc.zip

FastQC produces reports on ``html`` and in ``.zip`` format. We will use the ``html`` files to inspect visually our results. You can open them in any browser in your local machine.

To understand how to interpret the results please check `FastQC's webpage <http://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/>`__

If you are running this pipeline in a server, here is a reminder on how to move files between servers and local machines: `<https://help.cropdiversity.ac.uk/file-transfers.html>`_

.. note:: We will use FastQC two times in the pipeline: in the raw reads and after using Trimmomatic to check the trimming effects.

After inspecting the results, we are ready to the next step, which is to trim the raw reads and for that we will use Trimmomatic.