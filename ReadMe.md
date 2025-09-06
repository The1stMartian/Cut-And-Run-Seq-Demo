# Example data visualizations from my professional coding projects
<i>As much of my coding is proprietary, I made this page to at least demonstrate the outputs</i>
<i>Presented here: a flow cytometry analysis pipeline</i><br>
<i>Additional Proprietary Pipeline Demo: [Flow Cytometry Analysis](https://github.com/The1stMartian/Pipeline-Examples)</i>

# Cut-And-Run Seq
Technical Overview: 
- An NGS analysis script in R and utilizing various command-line bioinformatics tools
- Purpose: analyze NGS cut-and-run sequencing data
- Scope: Fastq file QC, mapping, peak detection, and differential binding analysis
- Outputs: multiple tables including differential binding analysis, and visualization files 

## Steps:
- Ingest metadata with filepaths, and sample groups
- Determine if reads are SE or PE
- Run QC on fastq files - FastQC
- Trim fastq files on quality and adapters - Timmomatic
- Post-trim QC - FastQC
- Map to the human genome - Bowtie2
- Align spike-in control - Bowtie2
- Sort .bam files and filter unmapped reads - SamTools
- Shift reads to account for MN'ase activity in Cut-And-Run - DeepTools ATAC Shift
- Remove PCR and optical duplicates - Picard
- Quantify mapped reads - SamTools  
- Peak Calling (narrow/broad) - MACS3
- Compare peaks between replicates - Irreproducible Discovery Rate
- Annotate Peaks - ChipSeekR
- Write non-normalized BigWig files for visualization - 
- Write spike-in control-normalized BigWig files - DeepTools bamCoverage
- Write input and spike-in control-normalized BigWig files - DeepTools bamCoverage
- Output readcounts for differential binding
- Downstream: Use readcounts as input for diffBind to quantify differential binding
- Quantification:
	- Pearson correlation test between ChIP and control samples
	- Pearson correlation test between rerun and original samples 
	- False discovery rate calculation 
	- Comparison of differentially bound regions between original and rerun

## Rationale:
- The Cut-and-Run pipeline published by the original authors did not use a docker container, making executiton problematic. The pipeline was re-built with equivalent processing steps using reproducible methods (i.e. containerization of command line tools in a docker container)

## Visual QC:
- As quantitative data (peak location and abundance values) were not made available by the authors, qualitative measures were required to establish reproducibility.

### Figure 5D
- The abundance of histone proteins at peak region 1 in the presence/absence of the BMI1 protein shown to affect histone binding in the manuscript.
- My repeat data are shown below and indicate a similar binding pattern to the published data. 
- Red stars indicate two disparities between the re-run and originally published results. Given the other quality control checks and overall similarity between the original and rerun data, I concluded that these point may represent errors in the authors' analysis.
<br>
!["Figure 5D"](./media/figure5D.jpg)
<br>

### Figure 5E
- Binding of H3K27me3 and H3K27Ac at region 2 shows differential binding in cells at three different ages (fetal, newborn, and adult) in both the original and rerun.<br>
- My re-analysis (lower two panels) confirms the age-dependent effect at genes IGF2BP1 and LIN28B seen in the original manuscript (upper).
- (Minor differences in the signal of H3K27me are due to minor changes in magnification in the manuscript)
!["Figure 5E"](./media/figure5E.jpg)

### Figure 6E
- Binding pattern of multiple polycomb repressive complex proteins to the IGF2BP1 target gene
- Peak binding is consistent between the original and rerun for ach protein<br>
!["Figure 6E"](./media/figure6E.jpg)

## Conclusions
- While it would be preferable to run a quantitative comparison between the original and re-run data using a Pearson correlation calculation, qualitative analyses were the only option available.
- Reproducibility between sample replicates was clearly defined using IDR
- Qualitatively, peak location and general profiles appeared to match the original data quite consistently.