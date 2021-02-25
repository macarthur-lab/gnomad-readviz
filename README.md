###  Readviz Pipeline

#### step1: select samples
For each variant, select the ~10 sample ids to use for readviz tracks for each genotype (hom, het, hemi).
Output a hail table that lists these sample ids for each variant.  
  
**Inputs**:  
- `gs://gnomad/metadata/genomes_v3.1/gnomad_v3.1_sample_qc_metadata.ht` - sample metadata used for filtering for samples that are release and have crams, and for sample sex. 
- `gs://gnomad/raw/genomes/3.1/gnomad_v3.1_sparse_unsplit.repartitioned.mt` - matrix table with genotypes

**Outputs**:
- `gs://gnomad-readviz/v3_and_v3.1/gnomad_v3_1_readviz_crams.ht` has the schema:
  ```
    ----------------------------------------
    Row fields:
        'locus': locus<GRCh38>
        'alleles': array<str>
        'samples_w_het_var': array<struct {
            S: str,
            GQ: int32
        }>
        'samples_w_hom_var': array<struct {
            S: str,
            GQ: int32
        }>
        'samples_w_hemi_var': array<struct {
            S: str,
            GQ: int32
        }>
    ----------------------------------------
    Key: ['locus', 'alleles']
    ```

----
#### step2: rekey

**Inputs**:
- `gs://gnomad/readviz/genomes_v3/gnomad_v3_readviz_crams.ht` - generated by step 1
**Outputs**:
- `gs://gnomad-readviz/v3_and_v3.1/gnomad_v3_1_readviz_crams_exploded_keyed_by_position_bin.ht/`
- `gs://gnomad-readviz/v3_and_v3.1/gnomad_v3_1_readviz_crams_exploded_with_key.ht/`
- `gs://gnomad-readviz/v3_and_v3.1/gnomad_v3_1_readviz_crams_exploded_keyed_by_sample.ht/` 

#### step3: export per-sample tsvs
Takes the keyed-by-sample hail table output in step 2, and writes out a tsv for each sample, containing the list of 
variants that will be used for readviz from that sample. 
Split across multiple hail clusters (see `run_step3__export_per_sample_tsvs.sh`)

#### step4: generate tsv of cram paths

#### step5:  run haplotype caller per sample

#### step6: subset tsv to successful bamouts

#### step7  deidentify bamouts

#### step8  combine deidentified bamouts
   
