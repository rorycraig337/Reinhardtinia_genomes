Run RepeatModeler

```
BuildDatabase -name Chlamydomonas_schloesseri.V1 -engine ncbi ../../genome_assemblies/c_schloesseri/final_assembly/Chlamydomonas_schloesseri.V1.fa
RepeatModeler -pa 32 -database Chlamydomonas_schloesseri.V1
perl ../c_incerta/renameRMDLconsensi.pl Chlamydomonas_schloesseri.V1-families.fa Chlamydomonas_schloesseri Chlamydomonas_schloesseri_repmod.fa
```

BLAST results against all C. reinhardtii and V. carteri, filtering unknown hits with e < 0.001

```
blastn -query Chlamydomonas_schloesseri_repmod.fa -db ../c_incerta/Cr_Vc_transcripts.fa -outfmt "6 qseqid sseqid pident slen qlen length mismatch gapopen evalue" -out repmod-v-transcripts.out
perl ../c_incerta/filter_repeat_modeler_hits.pl --blast repmod-v-transcripts.out --cutoff 0.001 --fasta Chlamydomonas_schloesseri_repmod.fa --out Chlamydomonas_schloesseri_repmod_filtered.fa
```

Make custom lib by combining Volvocales curated repeats with RepeatModeler output

```
cat ../c_incerta/volvocales_repeat_lib_w_nonTE.fa Chlamydomonas_schloesseri_repmod_filtered.fa > c_sch_custom_lib.fa
```

Run RepeatMasker without masking low complexity repeats

```
RepeatMasker -nolow -a -xsmall -gccalc -lib c_sch_custom_lib.fa ../../genome_assemblies/c_schloesseri/final_assembly/Chlamydomonas_schloesseri.V1.fa
```
