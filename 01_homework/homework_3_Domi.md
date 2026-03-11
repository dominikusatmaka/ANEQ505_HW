~={red}(1point)=~ for Alpha Rarefaction Plot
Run Core Metrics ~={red}(1 point; .25pts per line)=~
Make alpha diversity plots ~={red}(3points)=~
~={red}10 points=~ for the questions 

~={red}15 points total=~
------------------------------------------------------------------

Due: 

**For complete credit for this assignment, you must answer all questions and include all commands in your obsidian upload.**

------------------------------------------------------------------
**Learning Objectives**
1. Practice recording commands and editing code to match your analysis.
2. Perform alpha rarefaction and determine an appropriate sequencing depth.
3. Run core metrics, generate plots for alpha and beta diversity
--------------------------------------------------

**Cow Site Data Workflow**, part 3

Load qiime2 in a terminal session after you go into the **cow** folder

```
# Insert the two commands to activate qiime2
module purge  
  
module load qiime2/2024.10_amplicon


```

### Alpha Rarefaction Plot ~={red}(1 point)=~
- Chose the input sequencings depths (min and max) for generating the alpha rarefaction plot: 

```
#go to the cow directory

qiime diversity alpha-rarefaction \
--i-table ../dada2/cow_table_dada2_filtered300.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-max-depth 10000 \
--o-visualization alpha_rarefaction_curves_16S.qzv \
```


### Run Core Metrics ~={red}(1 point)=~

```
cd ../

qiime diversity core-metrics-phylogenetic \
--i-table dada2/cow_table_dada2_filtered300.qza \
--i-phylogeny tree/tree_gg2.qza \
--m-metadata-file metadata/cow_metadata.txt \
--p-sampling-depth 1500 \
--output-dir core_metrics_results
```

### Visualize alpha diversity plots
- generate a plot to visualize the observed features ~={red}(1 point)=~
```
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/observed_features_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/observed_features_statistics.qzv
```

- generate a plot to visualize faith's PD ~={red}(2 points)=~
```
## insert the entire code chunk for generating this visualization 
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/shannon_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/shannon_statistics.qzv  
  
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results/faith_pd_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results/faiths_pd_statistics.qzv

```



## Homework questions ~={red}(10 points)=~

1. what is the name of the file you needed to use to figure out what min and max depths to use to generate the alpha rarefaction plot? (Hint: which file contains the sequencing depths for each sample)
	The file is cow_table_dada2_filtered300.qza. 
2. what did you choose for the rarefaction depth (the input for core metrics -p-sampling-depth flag)? why? 
	I chose a rarefaction depth that retains most of the samples while removing samples with very low sequencing depth. This ensures fair comparison across samples while keeping as much sequencing data as possible. I use 1500 reads sequences per sample because most samples had sequencing depths higher than this value.
3. Which cow body location had more observed features? Which has the lowest?
	The rumen had the highest number of observed features, while milk had the lowest observed features.
4. What is the main difference between Faiths PD and Shannons alpha diversity metrics?  
	Faith’s PD (Phylogenetic Diversity) measures diversity based on phylogenetic relationships between organisms. Shannon diversity measures diversity based on species richness and evenness, but does not use phylogenetic relationships.
5. Which diversity metrics produced by the core-metrics pipeline require phylogenetic information?
	The metrics that require phylogenetic information are: Faith’s Phylogenetic Diversity (Faith’s PD), Unweighted UniFrac, and Weighted UniFrac. These metrics use a phylogenetic tree of the taxa.
6. Which two body sites have the highest Faiths PD alpha diversity?  Are the groups significantly different?
	The two body sites with the highest Faith’s PD are rumen and feces. Yes, the groups appear significantly different based on the statistical test shown in the visualization.
7. Does it seem like there are any groupings in the beta diversity? What are the groupings? 
	Yes, there are groupings in the beta diversity plots.  
Samples tend to cluster based on cow body location (for example: rumen, feces, milk, and skin).
8. Why do you think these samples are grouping together? 
	These samples group together because different body sites host distinct microbial communities due to differences in environment, nutrients, oxygen levels, and biological function.
9. What test can you run to determine if the groups are significantly different?
	The test commonly used is PERMANOVA (Permutational Multivariate Analysis of Variance).
10. What command would you use to run that test?

```
#insert command for running the test you suggest from question 7

qiime diversity beta-group-significance \  
--i-distance-matrix weighted_unifrac_distance_matrix.qza \  
--m-metadata-file sample-metadata.tsv \  
--m-metadata-column body-site \  
--p-method permanova \  
--o-visualization permanova-body-site.qzv

```