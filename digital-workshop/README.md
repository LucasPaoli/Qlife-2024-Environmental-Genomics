# Environmental Genomics with Tara Oceans 
## Qlife winter school 2024: Quantitative Seascape Ecology of Marine Plankton 
### Tuesday March 5th

## Digital Workshop 

| Time |  Topic  | Location
|:-----------:|:----------:|:--------:|
| 13:30 - 13:45 | Introduction to Pangenomics | Room 313 |
| 13:45 - 15:00 | Case example: diazotrophy in Trichodesmium | Room 313 |
| 15:00 - 15:20 | Break | Room 313 |
| 15:20 - 15:45 | Introduction to Phylogenomics and Extended Genomic Resources | Room 313 |
| 15:45 - 17:00 | Extended coverage of the Trichodemsium genus | Room 313 |

## Before arriving at the workshop

Make sure you have Anvi'o v8 installed on your laptop, see [anvio.org/install/](anvio.org/install/). 

## Presentations 

The presentations will be uploaded in the corresponding [folder](https://github.com/LucasPaoli/Qlife-2024-Environmental-Genomics/tree/main/talks).

## First part of the workshop

In this part of the workshop, we will reproduce some of the analyses published [here](https://doi.org/10.1073/pnas.2112355118).

<br>

#### 1. First, let's download the genomes that we need for this analysis. You can open your terminal, go to the desired working directory and use the following commands:

```
wget https://sunagawalab.ethz.ch/share/paolil/QLIFE_TRICHODESMIUM/trichodesmium-original-data.tar.gz
tar -xzf trichodesmium-original-data.tar.gz
```

The data is organised as follows:

```
.
├── trichodesmium-genomes --> A directory with the genomes. 
├── trichodesmium-contigs-db --> The pre-computed contigs databases associated with the genomes.
├── trichodesmium-pan --> The pre-computed pan-genomics database. 
├── NitrogenFixationHmms/ --> A curated set of HMM models to detect Nitrogen Fixation pathways. 
└── decorated-output/ --> This folder contains the processed and decorated output, try to have a go with you own results before using it. 
```

<br>

#### 2. From the genomes, let's make Anvi'o contigs databases using [anvi-generate-contigs-database](https://anvio.org/help/8/programs/anvi-gen-contigs-database/). Let's take a single genome as an example: `Trichodesmium_thiebautii_H9-4`.

<details>
<summary><i>Click to display the command.</I></summary>

```
conda activate anvio-8 # Don't forget to activate your Anvi'o environment!
anvi-gen-contigs-database -f trichodesmium-genomes/Trichodesmium_thiebautii_H9_4.fa -o Trichodesmium_thiebautii_H9_4_CONTIGS.db
```

</details>

<br>

#### 3. Now that we have processed the genomes into an Anvi'o compatible database, we can leverage Anvi'o's capabilities! First, let's annotate the genome with bacterial universal marker genes using [anvi-run-hmms](https://anvio.org/help/8/programs/anvi-run-hmms/). 

<details>
<summary><i>Click to display the command.</I></summary>

```
anvi-run-hmms -c Trichodesmium_thiebautii_H9_4_CONTIGS.db -T 4 -I Bacteria_71
```

</details>

<br>

#### 4. We can then use [anvi-run-ncbi-cogs](https://anvio.org/help/main/programs/anvi-run-ncbi-cogs/) to functionally annotate our genome. Make sure you have the ncbi-cogs database set up before running the command (see [anvi-setup-ncbi-cogs](https://anvio.org/help/main/programs/anvi-setup-ncbi-cogs)).

<details>
<summary><i>Click to display the command.</I></summary>

```
anvi-run-ncbi-cogs -c Trichodesmium_thiebautii_H9_4_CONTIGS.db
```

</details>

As you can see, running all those commands on all the genomes can take quite some time. This is why we already did it for you, those results are available in `trichodesmium-contigs-db` and we will move forward with that data and pretend that you just ran it all yourself. 

<br>

#### 5. Now, it's time to build a pangenome! This can be done with the command [anvi-pan-genome](https://anvio.org/help/main/programs/anvi-pan-genome/). The more genomes we include, the longer this will take, so let's start with two genomes `Trichodesmium_thiebautii_H9_4` and `Trichodesmium_erythraeum_IMS101`. Before building the pangenome, we first need to import those two genomes and their contigs database into a single storage database with [anvi-gen-genomes-storage](https://anvio.org/help/main/programs/anvi-gen-genomes-storage/).

<details>
<summary><i>Click to display the command.</I></summary>

```
anvi-gen-genomes-storage -e trichodesmium-pan/genomes-list-light.txt -o TRICHODESMIUM_LIGHT_STORAGE_GENOMES.db
anvi-pan-genome -g TRICHODESMIUM_LIGHT_STORAGE_GENOMES.db -n TRICHODESMIUM -o Trichodesmium_Pangenome --skip-alignments -T 4
```

Same here... this can take quite some time, so we already computed the full pangenome database for you, which is available in `trichodesmium-pan`.

<br>

#### 6. Let's have a look at this pangenome using [anvi-display-pan](https://anvio.org/help/main/programs/anvi-display-pan/).

</details>
<summary><i>Click to display the command.</I></summary>

```
anvi-display-pan -g trichodesmium-pan/TRICHODESMIUM_STORAGE_GENOMES.db -p trichodesmium-pan/Trichodesmium_Pangenome/Trichodesmium-PAN.db
```

</details>

Try to analyse the data yourself. Once you have, you can look at [this figure](https://www.pnas.org/doi/full/10.1073/pnas.2112355118) built exaclty from this pangenome. You can reproduce this figure with the following command:

</details>
<summary><i>Click to display the command.</I></summary>

```
anvi-display-pan -g decorated-output/TRICHODESMIUM_STORAGE_GENOMES.db -p decorated-output/Trichodesmium_Pangenome/Trichodesmium-PAN.db
```

</details>

<br>

## Second part fo the workshop

Here, we will extend the previous analysis with exclusive data from the genomic resources discussed this morning.

<br>

#### 1. We first want to download the data for this part of the workshop. Open your terminal (see intro to Unix [here](https://astrobiomike.github.io/unix/unix-intro) or [here](https://sunagawalab.ethz.ch/share/teaching/ptb24/contents/1_Unix1/01_unixcommand.html) if needed) and move to the desired directory. You can then download and extract the data needed with the following commands:

```
wget https://sunagawalab.ethz.ch/share/paolil/QLIFE_TRICHODESMIUM/trichodesmium-extended-data.tar.gz 
tar -xzf trichodesmium-extended-data.tar.gz
cd trichodesmium-extended-data
``` 

The data is organised as follows:

```
.
├── trichodesmium-genomes.fa --> the concatenated genomes we will use for the analysis. These include the ones from the previous analyses along with those from OMDv2 and the Tara Transect.
├── trichodesmium-genomes.infos --> information associated with the fasta file (which contigs belong to which genome). 
├── NitrogenFixationHmms/ --> A curated set of HMM models to detect Nitrogen Fixation pathways. 
├── raw-output/ --> This folder contains the expected output for this afternoon, please use it if any of the following steps breaks or takes too long. 
└── decorated-output/ --> This folder contains the processed and decorated output, try to have a go with you own results before using it. 
```

<br>

#### 2. Then we need to process those genomes with Anvi'o to generate a contigs database using [anvi-generate-contigs-database](https://anvio.org/help/8/programs/anvi-gen-contigs-database/). Start by having a look at the help page and try to write yourself the command (and don't forget to activate your environment with Anvi'o).

<details>
<summary><i>Click for solution.</I></summary>

```
conda activate anvio-8
anvi-gen-contigs-database -f trichodesmium-genomes.fa -T 8 -o trichodesmium-CONTIGS.db
```

</details>

<br>

#### 3. We then need to give Anvi'o some information so we can move forward smoothly. We have to (1) generate a profile database associated with our contigs database and (2) provide a collection (a map between contigs and genomes) so Anvi'o knows what is what.

<details>
<summary><i>Click to display the commands.</I></summary>

```
anvi-profile -c trichodesmium-CONTIGS.db --blank-profile --skip-hierarchical-clustering -o PROFILE -S MAGs
anvi-import-collection -c trichodesmium-CONTIGS.db trichodesmium-genomes.infos --contigs-mode -C MAGs -p PROFILE/PROFILE.db
```

</details>

<br>

#### 4. Okay. We now have a set of databases that Anvi'o can work with, which contains all our Trichodesmium genomes. Now, let's annotate them. We want to (1) run the HMMs models for Bacterial universal marker genes as well as (2) run our curated set of HMMs specific to Nitrogen Fixation pathways. First, have a look at the help page of [anvi-run-hmms](https://anvio.org/help/8/programs/anvi-run-hmms/) and then check out the command below: 

<details>

<summary><i>Click to display the commands.</I></summary>
```
anvi-run-hmms -c trichodesmium-CONTIGS.db -T 8 -I Bacteria_71
anvi-run-hmms -c trichodesmium-CONTIGS.db -H NitrogenFixationHmms 
```

</details>

<br>

#### 5. We can summarize those results to have a look at our data using [anvi-summarize](https://anvio.org/help/8/programs/anvi-summarize/) with the following command:

```
anvi-summarize -c trichodesmium-CONTIGS.db -p PROFILE/PROFILE.db -o SUMMARY -C MAGs
```

You can then open `SUMMARY/index.html` to look at the data in your browser.

<br>

#### 6. Alright, now is time to do some phylogenomics! First let's extract the sequences for the marker genes we want to build a phylogenetic tree with. This can be done with [anvi-get-sequences-for-hmm-hits](https://anvio.org/help/8/programs/anvi-get-sequences-for-hmm-hits/) since we already ran the HMMs in step 4.

<details>
<summary><i>Click to display the commands.</I></summary>

```
anvi-get-sequences-for-hmm-hits -c trichodesmium-CONTIGS.db -p PROFILE/PROFILE.db -C MAGs --hmm-sources Bacteria_71 --concatenate-genes -o Trichodesmium-Bacteria_71_ALIGN --return-best-hit
```

</details>

<br>

#### 7. Now that we have the alignment, we can use build the tree. We can use the built-in option of Anvi'o with [anvi-gen-phylogenomic-tree](https://anvio.org/help/8/programs/anvi-gen-phylogenomic-tree/).

<details>
<summary><i>Click to display the commands.</I></summary>

```
anvi-gen-phylogenomic-tree -f Trichodesmium-Bacteria_71_ALIGN -o Trichodesmium-Bacteria_71_TREE
```

</details>

<br>

#### 8. We then want to build a table with the relevant information from our database that we can then display along with the tree. For that, you cant run:

```
paste SUMMARY/bins_summary.txt <(cut -f 2-7 SUMMARY/bins_across_samples/hmms_NitrogenFixationHmms.txt) > metadata.txt
```

<br>

#### 9. And now it's time for the Anvi'o Magic, open the interactive interface by running [anvi-interactive](https://anvio.org/help/8/programs/anvi-interactive/).

<details>
<summary><i>Click to display the commands.</I></summary>

```
anvi-interactive --manual-mode -t Trichodesmium-Bacteria_71_TREE -p PROFILE.db -d metadata.txt
```

</details>

<details>
<summary><i>Click to display image.</I></summary>

![This should have been an image](https://github.com/LucasPaoli/Qlife-2024-Environmental-Genomics/blob/main/digital-workshop/decorated-output.png?raw=true)

</details>

<br>

#### 10. If you want to load the interactive interface with the example decorated output, you can run:

```
cd decorated-output/
anvi-interactive --manual-mode -t Trichodesmium-Bacteria_71_TREE -p PROFILE.db -d metadata.txt
```
