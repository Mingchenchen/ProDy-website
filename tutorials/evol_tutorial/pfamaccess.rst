.. _pfamaccess:

Pfam Access
===============================================================================

The part shows how to access Pfam database. You can search protein family
accession numbers and information using a sequence or PDB/UniProt identifiers.
MSA files for families of interest can be retrieved in a number of formats.

.. ipython:: python

   from prody import *
   from matplotlib.pylab import *
   ion()  # turn interactive mode on

Search Pfam
-------------------------------------------------------------------------------

We use :func:`.searchPfam` for searching.  Valid inputs are UniProt ID,
e.g. :uniprot:`PIWI_ARCFU`, or PDB identifier, e.g. :pdb:`3luc` or ``"3lucA"``
with chain identifier.

Matching Pfam accession (one or more) as keys will map to a dictionary that
contains locations (alignment start, end, evalue etc), pfam family type,
accession and id.


We query Pfam using the :func:`.searchPfam`. with a UniProt ID.

.. ipython::
   :verbatim:

   In [1]: matches = searchPfam('PIWI_ARCFU')

   In [2]: matches
   Out[2]:
   {'PF02171': {'accession': 'PF02171',
     'id': 'Piwi',
     'locations': [{'ali_end': '405',
       'ali_start': '111',
       'bitscore': '228.70',
       'end': '406',
       'evalue': '1.1e-64',
       'hmm_end': '301',
       'hmm_start': '2',
       'start': '110'}],
     'type': 'Pfam-A'}}

Input can also be a protein sequence or a file containing the sequence:

.. ipython::
   :verbatim:

   In [1]: sequence = ('PMFIVNTNVPRASVPDGFLSELTQQLAQATGKPPQYIAVHVVPDQLMAFGGSSE'
      ...: 'PCALCSLHSIGKIGGAQNRSYSKLLCGLLAERLRISPDRVYINYYDMNAANVGWNNSTFA')



   In [2]: matches = searchPfam(sequence)

   In [3]: matches
   Out[3]:
   {'PF01187': {'accession': 'PF01187.13',
     'class': 'Domain',
     'id': 'MIF',
     'locations': [{'ali_end': '114',
       'ali_start': '1',
       'bitscore': '174.2',
       'end': '114',
       'evalue': '7.7e-52',
       'evidence': 'hmmer v3.0',
       'hmm_end': '114',
       'hmm_start': '1',
       'significant': '1',
       'start': '1'}],
     'type': 'Pfam-A'}}

Input sequence cannot have gaps and should be at least 12 characters long.

For sequence searches, we can pass additional parameters to :func:`.searchPfam`
like *search_b* which will search pfam B and *skip_a* that will not search
pfamA database. Additional parameters include *ga* that uses gathering
threshold instead of e-value, *evalue* cutoff can also be specified and
*timeout* that can be set higher especially when searching larger
sequences, default is ``timeout=60`` seconds.

.. ipython::
   :verbatim:

   In [1]: matches = searchPfam(sequence, search_b=True, evalue=2.0)


Retrieve MSA files
-------------------------------------------------------------------------------

This example demonstrates how to search Pfam database with a given query using
:func:`.fetchPfamMSA`. Valid inputs are Pfam ID, e.g. :pfam:`Piwi`, or Pfam
accession, e.g. :pfam:`PF02171` obtained from :func:`.searchPfam`.  Alignment
type can be ``"full'`` (default), ``"seed"``, ``"ncbi"`` or ``"metagenomics"``
or ``"rp15"`` or ``"rp35"`` or ``"rp55"`` or ``"rp75"``.

.. ipython::
   :verbatim:

   In [1]: fetchPfamMSA('piwi', alignment='seed')
   Out[1]: 'piwi_seed.sth'

A compressed file can be downloaded by setting ``compressed=True``.
The ``format`` of the MSA can be of ``"selex"`` (default), ``"stockholm"`` or
``"fasta"``.  This will return the path of the downloaded MSA file.
The ``output`` name can be specified, for by default it will have
``"accession/ID_alignment.format"``.

Note that in this case we passed a folder name, the downloaded file is saved
in this folder, after it is created if it did not exist. Also longer timeouts
are necessary for larger families. Some other parameters like ``gap``,
``order`` or ``inserts`` can be set, as shown in the following example.

.. ipython::
   :verbatim:

   In [1]: fetchPfamMSA('PF02171', compressed=True, gaps='mixed',
      ...: inserts='lower', order='alphabetical', format='fasta')
   Out[1]: 'PF02171_full.fasta.gz'