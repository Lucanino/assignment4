source("https://bioconductor.org/biocLite.R")
#source lädt die Daten von der URL als package in R
if (!requireNamespace("BiocManager", quietly = TRUE))
  install.packages("BiocManager")
#das Paket BiocManager wird als installator heruntergeladen
BiocManager::install("rGADEM")
#über Paket BiocManager wird das Paket rGADEM installiert
library("rGADEM")

pwd <- ""
path<- system.file("extdata/VL.fasta", package="rGADEM")
#path wird definiert als der Dateipfad von der fasta-Datei im Ordner von Paket rGADEM (mit Test_100.fasta anstatt VL.fasta können die Testdaten genutzt werden)
FastaFile <- paste(pwd, path, sep="")
#FastaFile wird definiert als der Dateipfad von der fasta-Datei
sequences <- readDNAStringSet(FastaFile, "fasta")
#mit read.DNAStringSet wurde die Sequenz aus den Daten von Test_100.fasta für GADEM() aufgearbeitet (Struktur (Sequenzen mit Namen) ändert sich prinzipiell nicht)
gadem <- GADEM(sequences, verbose=1)
#gadem() sucht nun häufigste Consensussequenzen von 4/5 meren
consensus(gadem)
#consensus filtert die Consensussequenzen heraus
pwm <- gadem@motifList[[1]]@pwm
#eine position weight matrix wird von Motiv 1 (in eckigen Klammern) erstellt. siehe https://bioconductor.org/packages/release/bioc/vignettes/rGADEM/inst/doc/rGADEM.pdf
seqLogo(pwm)
#seqLogo stellt ein Sequenzlogo (Säulendiagramm abhängig von den Häufigkeiten der einzelnen Basen auf verschiedenen Motiven) abhängig von den Häufigkeiten von pwm dar