```
mv pempek_metadata_3_26.txt metadata.txt
```

```
qiime metadata tabulate \
--m-input-file metadata.txt \
--o-visualization metadata.qzv
```

```
qiime metadata tabulate \
--m-input-file metadata.txt \
--o-visualization metadata.qzv
```

```
mv demux_sr49_pempek_nonasal.qza demux_pempek.qza
```

cd demux

```
qiime demux summarize \
--i-data demux_pempek.qza \
--o-visualization demux_pempek.qzv
```

cd ../dada2

``` 
qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_pempek.qza \  
--p-trunc-len-f 150 \  
--p-trunc-len-r 150 \  
--p-n-threads 8 \  
--o-table table_run2.qza \  
--o-representative-sequences seqs_run2.qza \  
--o-denoising-stats dada2_stats_run2.qza
```

```
qiime dada2 denoise-paired \
--i-demultiplexed-seqs ../demux/demux_pempek.qza \
--p-trim-left-f 0 \
--p-trim-left-r 0 \
--p-trunc-len-f 250 \
--p-trunc-len-r 250 \
--p-n-threads 6 \
--o-representative-sequences pempek_seqs_dada2.qza \
--o-denoising-stats pempek_dada2_stats.qza \
--o-table pempek_table_dada2.qza
```

```
#Visualize the denoising results:
qiime metadata tabulate \
--m-input-file pempek_dada2_stats.qza \
--o-visualization dada2_stats.qzv

qiime feature-table summarize \
--i-table pempek_table_dada2.qza \
--m-sample-metadata-file ../metadata/metadata.txt \
--o-visualization dada2_table.qzv

qiime feature-table tabulate-seqs \
--i-data pempek_seqs_dada2.qza \
--o-visualization dada2_seqs.qzv
```

```
qiime feature-table filter-seqs \
--i-data pempek_seqs_dada2.qza \
--m-metadata-file pempek_seqs_dada2.qza \
--p-where 'length(sequence) < 300' \
--o-filtered-data pempek_seqs_dada2_filtered300.qza

qiime feature-table tabulate-seqs \
--i-data pempek_seqs_dada2_filtered300.qza \
--o-visualization pempek_seqs_dada2_filtered300.qzv

qiime feature-table filter-features \
--i-table pempek_table_dada2.qza \
--m-metadata-file pempek_seqs_dada2_filtered300.qza \
--o-filtered-table pempek_table_dada2_filtered300.qza
  
qiime feature-table summarize \
--i-table pempek_table_dada2_filtered300.qza \
--m-sample-metadata-file ../metadata/metadata.txt \
--o-visualization pempek_table_dada2_filtered300.qzv
    
```

```
qiime feature-classifier classify-sklearn \
--i-reads ../dada2/pempek_seqs_dada2_filtered300.qza \)|
--i-classifier 2024.09.backbone.v4.nb.qza \
--o-classification taxonomy_gg2_filtered.qza
```

