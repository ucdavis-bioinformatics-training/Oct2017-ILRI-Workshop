Running the dbcAmplicons pipeline
===============================================

**ALL of this should only be done on the cluster**

**1\.** First lets validate our install and environment

Change directory into the workshops space

	cd
	cd mca_example
	ls --color

you should see 4 directories: bin, Illumina_Reads, metadata, and src

Lets verify the software is accessible

	source ~/mca_profile

	dbcVersionReport.sh

you should see the version info for dbcAmplicons, flash2 and RDP.

If all this is correct, we are ready to begin processing the remaining amplicons

**1\.** Lets merge/join the read pairs, should take less than 10 minutes

	dbcAmplicons join -t 4 -O Slashpile.intermediate/16sV4V5/Slashpile-16sV4V5 -1 Slashpile.intermediate/16sV4V5/Slashpile-16sV4V5_R1.fastq.gz > join-16sV4V5.log

	cat join-16sV4V5.log

	dbcAmplicons join -t 4 -O Slashpile.intermediate/ITS1/Slashpile-ITS1 -1 Slashpile.intermediate/ITS1/Slashpile-ITS1_R1.fastq.gz  > join-ITS1.log

	cat join-ITS1.log

	dbcAmplicons join -t 4 -O Slashpile.intermediate/ITS2/Slashpile-ITS2 -1 Slashpile.intermediate/ITS2/Slashpile-ITS2_R1.fastq.gz  > join-ITS2.log

	cat join-ITS2.log

	dbcAmplicons join -t 4 -O Slashpile.intermediate/LSU/Slashpile-LSU -1 Slashpile.intermediate/LSU/Slashpile-LSU_R1.fastq.gz > join-LSU.log

	cat join-LSU.log

Choose the parameter --max-mismatch-density, based on the 16sV1V3 results. If you prefer the default, then leave as is. View the logs and histogram files (can use cat, less, more, etc.) for each amplicon. What do you see? Which one of these is different from the other

**2\.** Classify the merged reads using RDP, should take less than 4 hours

	dbcAmplicons classify -p 4 --gene 16srrna -U Slashpile.intermediate/16sV4V5/Slashpile-16sV4V5.extendedFrags.fastq.gz -O Slashpile.intermediate/16sV4V5/Slashpile-16sV4V5

	dbcAmplicons classify -p 4 --gene fungalits_unite -U Slashpile.intermediate/ITS1/Slashpile-ITS1.extendedFrags.fastq.gz -O Slashpile.intermediate/ITS1/Slashpile-ITS1

	dbcAmplicons classify -p 4 --gene fungalits_unite -U Slashpile.intermediate/ITS2/Slashpile-ITS2.extendedFrags.fastq.gz -O Slashpile.intermediate/ITS2/Slashpile-ITS2

	dbcAmplicons classify -p 4 --gene fungallsu -1 Slashpile.intermediate/LSU/Slashpile-LSU.notCombined_1.fastq.gz -2 Slashpile.intermediate/LSU/Slashpile-LSU.notCombined_2.fastq.gz -O Slashpile.intermediate/LSU/Slashpile-LSU

---

**5\.** Finally produce Abundance tables, should take less than 60 minutes

	dbcAmplicons abundance -S metadata/workshopSamplesheet.txt -O Slashpile.results/16sV4V5 -F Slashpile.intermediate/16sV4V5/Slashpile-16sV4V5.fixrank --biom  > abundance.16sV4V5.log
	cat abundance.16sV4V5.log

	dbcAmplicons abundance -r species -S metadata/workshopSamplesheet.txt -O Slashpile.results/ITS1 -F Slashpile.intermediate/ITS1/Slashpile-ITS1.fixrank --biom  > abundance.ITS1.log
	cat abundance.ITS1.log

	dbcAmplicons abundance -r species -S metadata/workshopSamplesheet.txt -O Slashpile.results/ITS2 -F Slashpile.intermediate/ITS2/Slashpile-ITS2.fixrank  --biom > abundance.ITS2.log
	cat abundance.ITS2.log

	dbcAmplicons abundance -S metadata/workshopSamplesheet.txt -O Slashpile.results/LSU -F Slashpile.intermediate/LSU/Slashpile-LSU.fixrank --biom > abundance.LSU.log
	cat abundance.LSU.log

view the logs

	cat abundance.*.log

