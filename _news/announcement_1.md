---
layout: post
date: 2024-05-31 16:11:00-0400
inline: true
related_posts: false
---

Pangaea got published on Nature Communication! 

---

<!-- [Full text](https://www.nature.com/articles/s41467-024-49060-z#Abs1) -->

#### Abstract
Although long-read sequencing enables the generation of complete genomes for unculturable microbes, its high cost limits the widespread adoption of long-read sequencing in large-scale metagenomic studies. An alternative method is to assemble short-reads with long-range connectivity, which can be a cost-effective way to generate high-quality microbial genomes. Here, we develop Pangaea, a bioinformatic approach designed to enhance metagenome assembly using short-reads with long-range connectivity. Pangaea leverages connectivity derived from physical barcodes of linked-reads or virtual barcodes by aligning short-reads to long-reads. Pangaea utilizes a deep learning-based read binning algorithm to assemble co-barcoded reads exhibiting similar sequence contexts and abundances, thereby improving the assembly of high- and medium-abundance microbial genomes. Pangaea also leverages a multi-thresholding algorithm strategy to refine assembly for low-abundance microbes. We benchmark Pangaea on linked-reads and a combination of short- and long-reads from simulation data, mock communities and human gut metagenomes. Pangaea achieves significantly higher contig continuity as well as more near-complete metagenome-assembled genomes (NCMAGs) than the existing assemblers. Pangaea also generates three complete and circular NCMAGs on the human gut microbiomes.

#### Code 
<!-- [https://github.com/ericcombiolab/Pangaea](https://github.com/ericcombiolab/Pangaea) -->