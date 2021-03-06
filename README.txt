Scripts for "The role of structural pleiotropy and regulatory evolution in the retention of heteromers of paralogs" by Axelle Marchant, Angel F. Cisneros, Alexandre K Dubé, Isabelle Gagnon-Arsenault, Diana Ascencio, Honey A. Jain, Simon Aubé, Chris Eberlein, Daniel Evans-Yamamoto, Nozomu Yachie & Christian R Landry


#########################################
#	Scripts for RNAseq folder	#
#########################################

These scripts are used to analyze the RNAseq data.

1.cleaning_RNAseq.sh
Reads were cleaned using cutadapt (Martin 2011). We removed the first 12 bp, trimmed the poly-A tail from the 3’ end, trimmed low-quality ends using a cutoff of 15 (phred quality + 33) and discarded reads shorter than 30 bp. The number of reads before and after cleaning can be found in Table S3. Raw sequences can be downloaded under the NCBI BioProject ID PRJNA480398.

2.create_gff_increasing_gene_window_with_3utr.R
Because we used a 3’mRNA-Seq Library, reads mapped largely to 3’UTRs. We increased the window of annotated genes in the SGD annotation (saccharomyces_cerevisiae_R64-2-1_20150113.gff version) using the UTR annotation from (Nagalakshmi et al., 2008).

3.alignment_and_count.sh
Cleaned reads were aligned on the reference genome of S288c from SGD (S288C_reference_genome_R64-2-1_20150113.fsa version) using bwa (Li and Durbin, 2009). Based on the reference genes-UTR annotation, the number of mapped reads per genes was estimated using htseq-count of the Python package HTSeq (Anders et al., 2015) and reported in Table S3.


#########################################################################
#	Scripts_for_plasmid_based_PCA_for_orthologous_proteins      		#
#########################################################################

These scripts are used to analyze the plasmid-based PCA experiments.

0_plate_arrangementsDAS.R
Creates an index of strains corresponding to the images taken in the PCA experiments.

1_imageAnalysis.R
Images of PCA plates were analysed using gitter (Wagih and Parts, 2014) to quantify colony sizes. 

2_filter_PCA_DIAS.R
Filtering of colonies flagged as irregular by gitter (as S or S, C flags) or that did not grow on the last diploid selection step or on DMSO medium were considered as missing data. We considered only bait-prey pairs with at least four replicates and used the median of colony sizes as PCA signal. 

3_PlotDistributions_DIAS.R
The size of colonies were converted to z-scores using the mean and standard deviation of the background distribution (Zs = (PCAs - μb)/sdb). 

4_FinalFiguresDAS.R
Script to generate figures 2B and 2C. 

theme_Publication.R
A ggplot theme developed by Byron Jaeger used to format the plots

#########################################################################
#	Scripts_for_interaction_HM_and_HET_data_analysis folder		#
#########################################################################

These scripts are used to analyze the PCA and HM and HET data from literature and database.

0.gitter.R
Images of PCA plates were analysed using gitter (Wagih and Parts, 2014) to quantify colony sizes. (Contact authors for PCA pictures data)

1.create_dataframe.R
We generated a big dataframe from all output gitter data files and reported tagged proteins for each colony

2.Filter.R
Colonies flagged as irregular by gitter (as S or S, C flags) or that did not grow on the last diploid selection step or on DMSO medium were considered as missing data. We considered only bait-prey pairs with at least four replicates and used the median of colony sizes as PCA signal. The data was finally filtered based on the completeness of paralogous pairs so we could test HMs and HETs systematically. 

3.Threshold.R
The distribution of PCA scores was modeled per duplication type (SSD and WGD) and per interaction tested (HM or HET) with the normalmixEM function (default parameters) available in the R mixtools package. The background signal on MTX was used as a null distribution to which interactions were compared. The size of colonies were converted to z-scores using the mean and standard deviation of the background distribution (Zs = (PCAs - μb)/sdb). PPI were considered as detected if Zs of the bait-prey pair was greater than 2.5.

4.Complete_HM.HET_with_literature_and_databases.R
To complement our experimental data, we extracted HM and HET of paralogs published in BioGRID version BIOGRID-3.5.166 and data from literature (Stynen et al., 2018; Kim et al., 2019). Output are organized by pair.

5.Age_of_duplication.R
We estimated the age of SSDs using gene phylogenies.

6.HM_compiled_data.R
To complement our experimental data, we extracted HM and HET of paralogs and singletons published in BioGRID version BIOGRID-3.5.166 and data from literature (Stynen et al., 2018; Kim et al., 2019). Output are organized orf per orf.

7.HM_comparison_among_groups_of_genes.R
Comparison of HM proportion among singletons and duplicates proteins

8.HM_HET_expression_duplication.r
Analyse of factor that could be implicated into the HM and HET maintenance or deletion.
Particularly, we focused on duplication mechanisms and expression.
It returns a summary table of HM / HET repartition among duplicates: TableS1

9.Main_Figures.R
Script to generate main figures from 6.HM_compiled_data.R and 8.HM_HET_expression_duplication.r scripts output

10.Sup_figures.R
Script to generate supplementary figures from 6.HM_compiled_data.R and 8.HM_HET_expression_duplication.r scripts output

#########################################################################
#	Scripts_for_interface_conservation folder			#
#########################################################################

001_gather_PDB_info.ipynb: Given an alignment of protein sequences to PDB chains, pairs of paralogs, and downloaded files with the asymmetrical unit, this script extracts information on which chains from which complexes from the PDB map to the given sequences. It returns a table that matches proteins to PDB structures and their paralogs. This is a Jupyter notebook. 001_gather_PDB_info.py is a Python-formatted version of the same code.

002_format_structural_data.R: This script does some additional formatting of the tables obtained from the previous step. The tables from this script are designed to be integrated into table S1. The table with the best structures selects the best crystal structures available for each of the identified complexes to use them in the interface conservation analysis.

003_interface_conservation_analysis.ipynb: This is the main script that looks for sequence conservation in interfaces. It uses the 001_generate_bio_assembly.py script from the scripts_for_simulations folder and code from the 004.1_call_interfaces.py script. It performs all the alignments and matches proteins to their corresponding PhylomeDB phylogenies. This is a Jupyter notebook. 003_interface_conservation_analysis.py is a Python-formatted version of the same code.

004_pdb2fasta_interfaces.sh: This is a modified version of Pierre Poulain's pdb2fasta.sh script available at https://bitbucket.org/pierrepo/pdb2fasta/. It saved PDB files as FASTA formatted files with interface residues in lowercase and the rest of the sequence in uppercase.

005_interface_counter.ipynb: This is a Jupyter notebook that takes the FASTA formatted interface files and outputs per chain counts of total residues and interface residues. This output was later integrated to table S11. 005_interface_counter.py is a Python-formatted version of the same code.

006_figure_S6.R: This script produces figure S6 from the processed files in the Data folder.

#########################################################################
#	Scripts_for_simulations folder					#
#########################################################################

001_generate_bio_assembly.py: Given a file with the asymmetric unit in a PDB file, it does the calculations necessary to obtain a given biological assembly from REMARK 350. Output files are PDB formatted and chains are renamed starting with chain A in the output file.

002_insertion_residues_alternative_coordinates.py: This script looks for insertion residues and alternative coordinates in biological assembly files. Insertion residues are just included in the numbering and residues with alternative coordinates are assigned those with the higher occupancy. Output files are PDB formatted.

003_foldx_repair_slurm.sh: This script takes a biological assembly and applies the FoldX Repair function for energy minimization. It is written to submit a job to a SLURM manager.

004.1_call_interfaces.py: This script is used to call interface residues from a biological assembly. Interfaces are called according to two methods: Levy's based on solvent accessibility changes and Tsai's based on distance to the other chain. Only interfaces obtained by the distance definition were used for further analyses because of the general overlap of residues. Output files are PDB formatted with the b-factor replaced by the region identified with the following code: 
	- 0.00 non-interfaces (distance)/ surface (solvent accessibility) 
	- 0.25 not used with distances / interior (solvent accessibility)
	- 0.50 not used with distances / support (solvent accessibility)
	- 0.75 nearby (distance)/ rim (solvent accessibility)
	- 1.00 contacting (distance)/ core (solvent accessibility)

This script depends on the helper functions from:
	- 004.2_call_interfaces_helper.py: helper functions for parsing PDB coordinates.

005.1_simulations_admin.sh: This is the admin script that performs the simulations with FoldX. It is designed to work with SLURM and parallelize replicates, with the results organized in folders numbered by replicates and the successive substitutions. It prepares a script to submit the job to SLURM with each of the replicates as arguments. It depends on the following scripts:
	- 005.2_foldx_coevol_config.sh: This is a helper script that is used to load parameters set by the user.
	- 005.3_simulations_main.sh: This is the main loop of the simulations. It manages the output FoldX files by organizing them in folders and calls the other scripts.
	- 005.4_substitution_generator­.py: This is the script that generates random substitutions based on a transition matrix for randomly chosen residues.
	- 005.5_apply_selection.R: This is the script that applies the selection function to the resulting mutants. 

006_figures_4_5_S9_S10_S11_S12.R: This is the script that generates figures 4, 5, S9, S10, S11, and S12 from the processed files in the Data folder.




