#### View on [github.io](https://mtandon09.github.io/MAFDash/)
# MAFDashRPackage
*Cuz once you've called the variants, it's a MAF dash to the finish line*

##Installation

**Required libraries**
Here's some code that will try to install required libraries that are not already installed (from [my Gist](https://gist.github.com/mtandon09/4a870bf4addbe46e784059bce0e5d8d6) about this)

```
all_pkgs<-c("rmarkdown", 
            "knitr",
            "flexdashboard",
            "htmltools",
            "DT",
            "bsplus",
            "crosstalk",
            "plotly",
            "canvasXpress",
            "maftools",
            "dplyr",
            "ComplexHeatmap",
            "circlize",
            "RColorBrewer",
            "ggbeeswarm"
            "data.table",
            "ensurer",
            "ggplot2")
            
### Figure out which ones are available in Bioconductor and install any new that are not already present
bioc_universe <- BiocManager::available()
bioc_packages <- intersect(bioc_universe, pkglist)
print(paste0(length(bioc_packages), " of ", length(pkglist), " packages found in Bioconductor."))
bioc_packages <- bioc_packages[!(bioc_packages %in% installed.packages()[,"Package"])]
print(paste0("Installing ",length(bioc_packages), " new packages from Bioconductor..."))
if(length(bioc_packages)) BiocManager::install(bioc_packages)

### Figure out which ones are available in CRAN and install any new that are not already present
cran_universe <- available.packages(repos="https://cloud.r-project.org")[,"Package"]
cran_packages <- intersect(cran_universe, pkglist)
print(paste0(length(cran_packages), " of ", length(pkglist), " packages found in CRAN."))
cran_packages <- cran_packages[!(cran_packages %in% installed.packages()[,"Package"])]
print(paste0("Installing ",length(cran_packages), " new packages from CRAN..."))
if(length(cran_packages)) install.packages(cran_packages, repos="https://cloud.r-project.org")
```

**Installation from Github**
```
install.packages(devtools)
library(devtools)
devtools::install_github("ashishjain1988/MAFDashRPackage")
```
<!--
* Install Dependencies
* `install.packages(c("dplyr","ensurer","ggplot2","tidyr","DT","rmarkdown","knitr","flexdashboard","htmltools","data.table","ggbeeswarm","RColorBrewer","plotly","circlize","canvasXpress","crosstalk","bsplus"))`
* `install.packages("BiocManager","maftools","ComplexHeatmap")`
* `BiocManager::install(c("TCGAbiolinks",))`
* Now install the `devtools` package
* `install.packages(devtools)`
* `library(devtools)`
* Run command `install_github("ashishjain1988/MAFDashRPackage")`-->

## Example
Here are some example dashboards created using TCGA data:
- [TCGA-UVM](https://mtandon09.github.io/MAFDash/output/TCGA-UVM.MAFDash.html)
- [TCGA-BRCA](https://mtandon09.github.io/MAFDash/output/TCGA-BRCA.MAFDash.html)

## Scope
[Mutation Annotation Format (MAF)](https://docs.gdc.cancer.gov/Encyclopedia/pages/Mutation_Annotation_Format/) is a tabular data format used for storing genetic mutation data. For example, [The Cancer Genome Atlas (TCGA)](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga) project has made MAF files from each project publicly available.

This repo -- **MAFDash** -- contains a set of R tools to easily create an HTML dashboard to summarize and visualize data from MAF file.

The resulting HTML file serves as a self-contained report that can be used to explore the result.  Currently, MAFDash produces mostly static plots powered by [maftools](https://bioconductor.org/packages/release/bioc/vignettes/maftools/inst/doc/maftools.html),  [ComplexHeatmap](https://github.com/jokergoo/ComplexHeatmap) and [circlize](https://github.com/jokergoo/circlize), as well as interactive visualizations using [canvasXpress](https://cran.r-project.org/web/packages/canvasXpress/vignettes/getting_started.html) and [plotly](https://plotly.com/r/).  The report is generated with a parameterized [R Markdown](https://rmarkdown.rstudio.com/) script that uses [flexdashboard](https://rmarkdown.rstudio.com/flexdashboard/) to arrange all the information. I hope to add more interactivity 

This repo is a companion to a Shiny app I made, [MAFWiz](https://github.com/mtandon09/mafwiz).  Instead of relying on a Shiny server, this dashboard is an attempt to try some of those things using client-side javascript functionality.

## Making a MAF Dashboard
The `MAFDash.Rmd` file can be rendered in R using the `rmarkdown` library like this:

```
# MAF data
maf_file="/path/to/maf/file"

library("MAFDashRPackage")
getMAFDashboard(maf_file,outputFilePath = ".",outputFileName="output" ,outputFileTitle="MAF Dashboard")

```
### Details
- `maf_file` can be anything that's accepted by maftools's [`read.maf`](https://rdrr.io/bioc/maftools/man/read.maf.html) function (path to a file, or a `MAF` , `data.frame`, or `data.table` object)
- The `getMAFdataTCGA` function can be used to download MAF data from TCGA website.

```
#Download MAF data from TCGA
CancerCode <- "ACC"
outputFolderPath <- "data"
maf <- getMAFdataTCGA(tcga_dataset = CancerCode,save_folder = outputFolderPath)
```
