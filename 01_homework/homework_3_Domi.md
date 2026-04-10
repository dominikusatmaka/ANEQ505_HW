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
--p-min-depth 10 \
--p-max-depth 33000 \
--o-visualization alpha_rarefaction_curves_16S_1.qzv \
```

Aslinya aku pakai 10 - 10000
kalau lihat di tabel nya, rentangnya antara 10 sampai 33700 an. Jadi bisa dibuat jadi 10 di min dan 33000 di max nya

### Run Core Metrics ~={red}(1 point)=~

```
cd ../

qiime diversity core-metrics-phylogenetic \
--i-table dada2/cow_table_dada2_filtered300.qza \
--i-phylogeny tree/tree_gg2.qza \
--m-metadata-file metadata/cow_metadata.txt \
--p-sampling-depth 5000 \
--output-dir core_metrics_results_5k
```

Aslinya aku pakai 1500
kalo lihat datanya bisa pakai 4500 sebenernya karena ketika angka itu datanya mulai plateau. Jadi datanya akan mulai stabil. --> coba rarefy pakai 4500 nanti --> ganti pakai core_metrics_results_5k
tapi biasanya semakin tinggi angka rarefy nya maka kita akan kehilangan lebih banyak sampel. Nah di 4500 ini kita akan kehilangan 40% data. Biasanya tergantung berapa banyak yang mau kita preserve dari data yang kita punya. Kaau kita mau preserve lebih banyak data maka rarefy nya bisa pakai angka yang rendah.

### Visualize alpha diversity plots
- generate a plot to visualize the observed features ~={red}(1 point)=~
```
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results_5k/observed_features_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results_5k/observed_features_statistics.qzv
```

- generate a plot to visualize faith's PD ~={red}(2 points)=~
```
## insert the entire code chunk for generating this visualization 
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results_5k/shannon_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results_5k/shannon_statistics.qzv  
  
qiime diversity alpha-group-significance \
--i-alpha-diversity core_metrics_results_5k/faith_pd_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results_5k/faiths_pd_statistics.qzv

qiime diversity alpha-correlation \
--i-alpha-diversity core_metrics_results_5k/faith_pd_vector.qza \
--m-metadata-file metadata/cow_metadata.txt \
--o-visualization core_metrics_results_5k/faith_pd_correlation_statistics.qzv
```



## Homework questions ~={red}(10 points)=~

1. what is the name of the file you needed to use to figure out what min and max depths to use to generate the alpha rarefaction plot? (Hint: which file contains the sequencing depths for each sample)
	The file is cow_table_dada2_filtered300.qzv. This file contains the sequencing depth (frequency) for each sample, which is used to determine the minimum and maximum depths.
2. what did you choose for the rarefaction depth (the input for core metrics -p-sampling-depth flag)? why? 
	I chose a rarefaction depth that retains most of the samples while removing samples with very low sequencing depth. This ensures fair comparison across samples while keeping as much sequencing data as possible. I use 1500 reads sequences per sample because most samples had sequencing depths higher than 1000 reads, but there are also sample that had sequencing lower than 2000 reads (very low sequencing depth). Also from the refraction curves we can see the graph start to plateau at around 1200-1500 sequencing depth. So I think to reduce the sample that may leads to bias because of the very low reads, I put 1500 reads sequences per sample  (midpoint between 1000 and 2000) as my rarefaction depth to make sure no sample below that read are included.
3. Which cow body location had more observed features? Which has the lowest?
	Based on the body location, we can see that the fecal sample had the highest observed featured and nasal sample had the lowest observed featured. Control sample also have low observed featured but this is not a "real" cow sample so the control can be excluded.
4. What is the main difference between Faiths PD and Shannons alpha diversity metrics?  
	Faith’s PD (Phylogenetic Diversity) measures diversity based on phylogenetic relationships between organisms. Shannon diversity measures diversity based on species richness and evenness, but does not use phylogenetic relationships.
5. Which diversity metrics produced by the core-metrics pipeline require phylogenetic information?
	The metrics that require phylogenetic information are: Faith’s Phylogenetic Diversity (Faith’s PD) for alpha diversity, Unweighted UniFrac, and Weighted UniFrac for beta diversity. These metrics use a phylogenetic tree of the taxa.
6. Which two body sites have the highest Faiths PD alpha diversity?  Are the groups significantly different?
	The two body sites with the highest Faith’s PD are fecal and skin sample compare to the nasal and oral sample. 
	For the second question, yes, the groups appear significantly different based on the statistical test shown in the visualization. As we can see from the pairwise results, the fecal and oral has p-value < 0.001.
7. Does it seem like there are any groupings in the beta diversity? What are the groupings? 
	Yes, there are groupings in the beta diversity plots based on the unweighted and weighted unifrac data. Samples tend to cluster based on cow body location. The fecal sample are group tightly together and forming a cluster separate from other body site. The skin and udder are more close one to another, while nasal and oral are also close together even the cluster is not that tight and more spread out.
8. Why do you think these samples are grouping together? 
	These samples group together because different body sites host distinct microbial communities due to differences in environment, nutrients, oxygen levels, and biological function. Like example the nasal and oral are grouping together because they were included in the respiratory system, so they share the same microbes. Also skin and udder are grouping together because they were included in body surfaces. Meanwhile fecal sample are cluster together because these sample are in spesific different environment than other sample from other body site. 
9. What test can you run to determine if the groups are significantly different?
	The test commonly used is a PERMANOVA test (Permutational Multivariate Analysis of Variance).
10. What command would you use to run that test?

```
#insert command for running the test you suggest from question 7

## permanova test
qiime diversity beta-group-significance \
--i-distance-matrix core_metrics_results_5k/unweighted_unifrac_distance_matrix.qza \
--m-metadata-file metadata/cow_metadata.txt \
--m-metadata-column body_site \
--p-method permanova \
--o-visualization core_metrics_results_5k/unweighted_unifrac_body_site_metric.qzv

Biasanya akan ditambahkan  -p pairwise untuk melakukan tes pairwise

## bray curtis  
qiime diversity beta-group-significance \
--i-distance-matrix core_metrics_results_5k/bray_curtis_distance_matrix.qza \
--m-metadata-file metadata/cow_metadata.txt \
--m-metadata-column body_site \
--p-method permanova \
--o-visualization core_metrics_results_5k/bray_curtis_body_site_metric.qzv

```












```


cd dada2

qiime feature-table transpose \
--i-table table_nomitochloro_gg2_filtered300.qza \
--o-transposed-feature-table table_nomitochloro_transposed.qza

qiime metadata tabulate \
--m-input-file table_nomitochloro_transposed.qza \
--m-input-file cow_seqs_dada2.qza \
--m-input-file ../taxonomy/taxonomy_gg2_filtered.qza \
--o-visualization tabulated_results.qzv
```

```
qiime feature-table filter-samples \
  --i-table table_nomitochloro_gg2_filtered300.qza \
  --m-metadata-file cow_metadata_nocontrols.txt \
  --o-filtered-table table_nomitochlorocontrols_gg2_filtered300.qza
```

```
qiime feature-table filter-samples \
--i-table ../dada2/table_nomitochlorocontrols_gg2_filtered300.qza \
--p-min-frequency 5000 \
--o-filtered-table table_5k.qza
```


```
qiime feature-table filter-samples \
--i-table ../dada2/table_nomitochlorocontrols_gg2_filtered300.qza \
--p-min-frequency 4500 \
--o-filtered-table table_4k.qza
```

**Filter out low abundance and low prevalence ASVs ~={red}(1 point)=~**

```
qiime feature-table filter-features \
--i-table table_4k.qza \
--p-min-frequency 50 \
--p-min-samples 20 \
--o-filtered-table table_4k_abund.qza
```

**Collapse features to genus level ~={red}(1 point)=~**
- We will collapse to the genus level to make it easier to interpret the results. (Hint: We used 7 for species, so think about which number you would use for genus.)

```
qiime taxa collapse \
--i-table table_4k_abund.qza \
--i-taxonomy ../taxonomy/taxonomy_gg2_filtered.qza \
--p-level 7 \
--o-collapsed-table table_4k_abund_L7.qza
```


**Run ANCOM-BC2 ~={red}(1 point)=~**

```
qiime composition ancombc2 \
--i-table table_4k_abund_L7.qza \
--m-metadata-file cow_metadata_nocontrols.txt \
--p-fixed-effects-formula body_site \
--o-ancombc2-output ancombc2_results_bodysite_genus_4k.qza
```


**Visualize the ANCOM-BC2 results ~={red}(1 point)=~**
- Generate a barplot to visualize the differentially abundant features. 
```
qiime composition tabulate \
--i-data ancombc2_results_bodysite_genus_4k.qza \
--o-visualization ancombc2_bodysite_genus_4k.qzv
  
qiime composition ancombc2-visualizer \
  --i-data ancombc2_results_bodysite_genus_4k.qza \
  --o-visualization ancombc2_barplot_bodysite_genus_4k.qzv


```qiime diversity alpha-rarefaction \
--i-table ../dada2/table_nomitochlorocontrols_gg2_filtered300.qza \
--m-metadata-file ../metadata/cow_metadata.txt \
--p-min-depth 10 \
--p-max-depth 33000 \
--o-visualization alpha_rarefaction_curves_16S_2.qzv \
```