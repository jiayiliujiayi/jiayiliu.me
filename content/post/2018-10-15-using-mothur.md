---
title: 'Using mothur: batch commands for processing sequencing data from Miseq'
author: ''
date: '2018-10-18'
slug: using-mothur
categories:
  - code
  - R
tags:
  - R
  - microbiota
---
### Inspired by & based on [Mothur Miseq SOP](https://www.mothur.org/wiki/MiSeq_SOP)

1. Please try interactive mode before batch mode (to know what happens within each command)  
2. Please check numeric parameters within batch file before running it. 

### start from here  

#### change the name of the file from stability.files to whatever suits your study
make.contigs(file=stability.files, processors=4)
summary.seqs(fasta=stability.trim.contigs.fasta)
screen.seqs(fasta=stability.trim.contigs.fasta, group=stability.contigs.groups, summary=stability.trim.contigs.summary, maxambig=0, maxlength=275)

summary.seqs(fasta=stability.trim.contigs.good.fasta)
unique.seqs(fasta=stability.trim.contigs.good.fasta)
count.seqs(name=stability.trim.contigs.good.names, group=stability.contigs.good.groups)

summary.seqs(count=stability.trim.contigs.good.count_table)


###pcr.seqs(fasta=silva.bacteria.fasta, start=11894, end=25319, keepdots=F, processors=8)
rename.file(input=silva.bacteria.pcr.fasta, new=silva.v4.fasta)###

summary.seqs(fasta=silva.v4.fasta)


align.seqs(fasta=stability.trim.contigs.good.unique.fasta, reference=silva.v4.fasta)
summary.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table)

screen.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table, summary=stability.trim.contigs.good.unique.summary, start=1968, end=11550, maxhomop=8)

summary.seqs(fasta=current, count=current)

filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=.)
unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)
pre.cluster(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table, diffs=2)

chimera.vsearch(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t)
remove.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, accnos=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.accnos)
summary.seqs(fasta=current, count=current)

classify.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table, reference=trainset10_082014.pds.fasta, taxonomy=trainset10_082014.pds.tax, cutoff=80)

remove.lineage(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.taxonomy, taxon=Chloroplast-Mitochondria-unknown-Archaea-Eukaryota)

remove.groups(count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.count_table, fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.taxonomy, groups=Mock)

dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.fasta, cutoff=0.01)
cluster(column=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.dist, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table)
cluster.split(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.taxonomy, splitmethod=classify, taxlevel=4, cutoff=0.01)
make.shared(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.opti_mcc.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table, label=0.01)
classify.otu(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.opti_mcc.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.taxonomy, label=0.01)

phylotype(taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.taxonomy)
make.shared(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.tx.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table, label=1)
classify.otu(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.tx.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.vsearch.pick.pick.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.pick.pick.taxonomy, label=1)
dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.fasta, output=lt, processors=8)
clearcut(phylip=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.phylip.dist)
rename.file(taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.opti_mcc.0.010.01.cons.taxonomy, shared=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.pick.opti_mcc.shared)
count.groups(shared=stability.opti_mcc.shared)
sub.sample(shared=stability.opti_mcc.shared, size=10000)
rarefaction.single(shared=stability.opti_mcc.shared, calc=sobs, freq=100)
summary.single(shared=stability.opti_mcc.shared, calc=nseqs-coverage-sobs-invsimpson-shannon, subsample=16000)
heatmap.bin(shared=stability.opti_mcc.0.01.subsample.shared, scale=log2, numotu=50) 
dist.shared(shared=stability.opti_mcc.shared, calc=thetayc-jclass-braycurtis, subsample=5000)
tree.shared(phylip=stability.opti_mcc.thetayc.0.01.lt.ave.dist)
pcoa(phylip=stability.opti_mcc.thetayc.0.01.lt.ave.dist)
nmds(phylip=stability.opti_mcc.thetayc.0.01.lt.ave.dist)
nmds(phylip=stability.opti_mcc.thetayc.0.01.lt.ave.dist, mindim=3, maxdim=3)
corr.axes(axes=stability.opti_mcc.thetayc.0.01.lt.ave.pcoa.axes, shared=stability.opti_mcc.0.01.subsample.shared, method=spearman, numaxes=3)
get.communitytype(shared=stability.opti_mcc.0.01.subsample.shared)
unifrac.unweighted(tree=current, count=current, distance=lt, processors=2, random=F, subsample=5000)
unifrac.weighted(tree=stability.tre, count=stability.count_table, distance=lt, processors=2, random=F, subsample=5000)



