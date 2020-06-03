# Analysis of MeRIP (m6A-RIP) data



## Remove rRNA reads using SortMeRNA
### Use GitHub version (version > v4.2.0)
SortMeRNA link:
https://github.com/biocore/sortmerna

### Running
* The only required options are --ref and --reads
* Options (any) can be specified usig a single dash e.g. -ref and -reads
* Both plain fasta/fastq and archived fasta.gz/fastq.gz files are accepted
* Relative paths are accepted

Examples:
<b> # single reference and single reads file </b>
sortmerna --ref REF_PATH --reads READS_PATH

<b> # for multiple references use multiple '--ref' </b>
sortmerna --ref REF_PATH_1 --ref REF_PATH_2 --ref REF_PATH_3 --reads READS_PATH

<b> # for paired reads use '--reads' twice </b>
sortmerna --ref REF_PATH_1 --ref REF_PATH_2 --ref REF_PATH_3 --reads READS_PATH_1 --reads READS_PATH_2

### Other parameters



<b> My real codes </b>
``` bash
sortmernaFolder=$ngsFolder/3.sortmerna
mkdir -p $sortmernaFolder
rrnaFasta=/media/meshif/sda/Genome/Worm/WS260/c_elegans.PRJNA13758.WS260.rDNA.fa

echo Start SortMeRNA
awk -F',' 'NR!=1{print $1}' $sampleTableFile | parallel -j 5 "echo SortMeRNA fastq {} && \
    sortmerna --ref $rrnaFasta --reads $trimFolder/{}_R1_val_1.fq.gz --reads $trimFolder/{}_R2_val_2.fq.gz \
    --aligned $sortmernaFolder/{}_with_rrna --other $sortmernaFolder/{}_non_rrna  --workdir $sortmernaFolder \
    --idx $sortmernaFolder/idx --kvdb $sortmernaFolder/{}_kvdb \
    --fastx -v --paired_in --out2 --threads 10 1> $sortmernaFolder/{}_sortmerna.log"
echo Finish SortMeRNA


```
