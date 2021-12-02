Inferring a phylogeny
=======================

There are multiple way to infer a phylogeny. Here, we will follow a maximum likelihood approach, and use the software `raxml-ng <https://github.com/amkozlov/raxml-ng>`_. 

We will use the following command to infer a quick tree with 100 bootstrap replicates:

.. code-block:: bash

    raxml-ng --all --msa all_loci.fasta --model GTR+G --prefix tree --threads 2 --seed 2 --bs-metric fbp

We can check whether the topplogies converge with:

.. code-block:: bash

    raxml-ng --rfdist --tree tree.raxml.mlTrees --prefix RF 


And we can check the convergence of the bootstrap with:

.. code-block:: bash

    raxml-ng --bsconverge --bs-trees tree.raxml.bootstraps --prefix t2 --seed 2 --threads 2 --bs-cutoff 0.01

Don't forget to check raxml-ng tutorial for more options: `<https://github.com/amkozlov/raxml-ng/wiki/Tutorial>`_

The file ending in ``.suport`` shows our phylogeny with bootstrap support. We can open it in FigTree. 

You might want to change the label names to species name + accession, instead of only accession. We can do that with `phyx <https://github.com/FePhyFoFum/phyx>`_, more specifically with ``pxrlt`` (taxon relabelling for trees). We will need list of current and new names.

For the current names, we can simply get from out ``acc`` file, and the new names we can get from the `Nicholls et al. 2015 <https://www.frontiersin.org/articles/10.3389/fpls.2015.00710/full>`_ paper: 

.. code-block:: bash

    I_auristellae_FG113
    I_cylindrica_FG35
    I_nouragensis_FgIntype
    I_cinnamomea_KGD465
    Zygia_mediana_Zygia917

.. code-block:: bash

    pxrlt -t tree.raxml.support -c current -n new > tree.raxml.support.names

Now we have a phylogeny with branch support values and proper label names. Enjoy!