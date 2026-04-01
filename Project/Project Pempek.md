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