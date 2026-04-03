# VGP Genome Assembly — *Saccharomyces cerevisiae* S288C

A complete diploid genome assembly of *Saccharomyces cerevisiae* S288C using the **Vertebrate Genome Project (VGP) pipeline** on Galaxy, combining PacBio HiFi and Illumina Hi-C sequencing data to produce two fully phased, chromosome-level haplotype assemblies.

---

## Overview

This project follows the [VGP Galaxy tutorial](https://training.galaxyproject.org/training-material/topics/assembly/tutorials/vgp_genome_assembly/tutorial.html) to assemble a high-quality, haplotype-phased diploid genome. Using synthetic HiFi reads and real Hi-C data, the pipeline produces two phased haplotype assemblies (Hap1 and Hap2) evaluated with multiple quality control tools.

---

## Organism

| Property | Value |
|---|---|
| Species | *Saccharomyces cerevisiae* |
| Strain | S288C |
| Genome size (estimated) | ~11.7 Mb |
| Ploidy | Diploid |

---

## Input Data

| Dataset | Format | Source |
|---|---|---|
| Synthetic PacBio HiFi reads (3 files, 50x coverage) | FASTA | [Zenodo 6098306](https://zenodo.org/record/6098306) |
| Illumina Hi-C reads (forward) | FASTQ.GZ | [Zenodo 5550653](https://zenodo.org/record/5550653) |
| Illumina Hi-C reads (reverse) | FASTQ.GZ | [Zenodo 5550653](https://zenodo.org/record/5550653) |

---

## Pipeline & Tools

All steps were run on [Galaxy](https://usegalaxy.org) using the following tools:

| Step | Tool |
|---|---|
| HiFi adapter trimming | Cutadapt |
| k-mer counting | Meryl |
| Genome profiling | GenomeScope2 |
| Contig assembly | Hifiasm (Hi-C phased mode) |
| GFA → FASTA conversion | gfastats |
| Assembly statistics | gfastats |
| Completeness assessment | BUSCO |
| k-mer based QC | Merqury |
| Bionano scaffolding | Bionano Solve |
| Hi-C scaffolding | YaHS |
| Decontamination | VGP Workflow 9 |

---

## Pipeline Steps

```
HiFi reads (x3 FASTA)
    │
    ▼
1. Adapter trimming (Cutadapt)
    │
    ▼
2. k-mer profiling (Meryl → GenomeScope2)
    │   └── Estimated genome size: ~11.7 Mb
    │   └── Heterozygosity: ~0.58%
    │
    ▼
3. Hi-C phased assembly (Hifiasm)
    │   └── Input: HiFi reads + Hi-C R1/R2
    │   └── Output: Hap1 GFA + Hap2 GFA
    │
    ▼
4. GFA → FASTA conversion (gfastats)
    │
    ▼
5. Quality Control
    │   ├── Assembly statistics (gfastats)
    │   ├── Gene completeness (BUSCO — saccharomycetes_odb10)
    │   └── k-mer accuracy (Merqury)
    │
    ▼
6. Scaffolding
    │   ├── Bionano optical map scaffolding
    │   └── Hi-C scaffolding (YaHS)
    │
    ▼
7. Decontamination
    │
    ▼
Final: Hap1 & Hap2 chromosome-level assemblies
```

---

## Key Results

### Assembly Statistics (gfastats)

| Metric | Hap1 | Hap2 |
|---|---|---|
| Number of contigs | ~16 | ~17 |
| Total length | ~11.3 Mb | ~12.2 Mb |
| Largest contig | ~1.53 Mb | ~1.53 Mb |
| Contig N50 | ~922 kb | ~923 kb |

### BUSCO Results (saccharomycetes_odb10)

| Metric | Hap1 | Hap2 |
|---|---|---|
| Complete (single-copy) | ~95.8% | ~95%+ |
| Duplicated | Low | Low |
| Missing | ~29 genes | ~29 genes |

### GenomeScope2 Estimates

| Property | Value |
|---|---|
| Haploid genome size | ~11.7 Mb |
| Heterozygosity | ~0.58% |
| Diploid coverage | ~50x |
| Model fit | >93% |

---

## References

- Rhie et al. (2021). Towards complete and error-free genome assemblies of all vertebrate species. *Nature*.
- Cheng et al. (2021). Haplotype-resolved de novo assembly using phased assembly graphs with hifiasm. *Nature Methods*.
- Simão et al. (2015). BUSCO: assessing genome assembly completeness with single-copy orthologs. *Bioinformatics*.
- Rhie et al. (2020). Merqury: reference-free quality, completeness, and phasing assessment for genome assemblies. *Genome Biology*.
- [VGP Galaxy Tutorial](https://training.galaxyproject.org/training-material/topics/assembly/tutorials/vgp_genome_assembly/tutorial.html)

---

## 👤 Author

> Washma Sajjad

---

*Assembly performed using the VGP pipeline on Galaxy as part of a genome bioinformatics tutorial.*
