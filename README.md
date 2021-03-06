# rmd_pdf_examples
Examples of use of rmarkdown for creating pdf-files

# Introduction
This document describes some examples of the use of [**R Markdown**](http://rmarkdown.rstudio.com/) to produce PDF files.  
The examples differ in the measure that additional LaTeX functionality has been used. 

*NB. handling of bibliographies in the manner done here is suddenly changed in package rmarkdown version 0.9 . This was corrected in 0.9.1 so please update the package to this version or later when you need this functionality.*  

# Software used
This document and the ones described are created by editing the input files (rmd-, tex- and bib-files) in the [**RStudio**](https://www.rstudio.com/products/RStudio/) environment and pressing the **Knit** button.  
This button-click executes the following workflow:   
* the R package [**knitr**](http://yihui.name/knitr/) is used to analyze the rmd-source and to execute the knitr chuncks. It also uses the information in the yaml header. The result is an md-file.
* the md-file is converted by [**Pandoc**](http://pandoc.org/) to a tex-file
* the tex-file is converted by LaTeX to a pdf-file

The following software is necessary :   
* a distribution of the typesetting system LaTeX. Working on Windows I used **MiKTeX**, but currently I am using **TinyTeX**
* a distribution of R with the **rmarkdown** and **knitr** packages
* the **Pandoc** software. The **RStudio** environment contains a copy of this software
* **RStudio** is not necessary but makes this workflow very easy: recommended!
* and the R packages you want to use: in the examples they are **xtable**, **ggplot2** and **ggthemes**

# The examples
## iris_data_set_vm1.rmd
In this basic example no additional LaTeX is used. The main advantage is that is trivial to convert the rmd-file to a html- or a docx-format. The main disadvantage is that internal references to figures and tables are not available. However internal linking is (shown) possible.

## iris_data_set_vm2.rmd
Here we use the basic LaTeXcommands **\label**, **\ref** and **\pageref** to get the internal references that were missing in the first example.
And also we show the use of a *child chunk*: to handle the references we made a change to the *setup chunk* and saved it in a separate rmd-file that we will include from now on.

## iris_data_set_vm3.rmd
In this example we use additional LaTeX packages to   
* set the default font to be sans-serif (via include of **header.tex**)
* load the package **subfig** so that in a chunk two figures can be placed side by side (via include of **header.tex**)
* (re) define some text macros (via include of **header.tex**)
* redefine some of the colors that are used for highlighting the R-code and its background (via include of **header.tex**).
With one of these settings the background color was made darker so that it would be just visible when the document is printed.
* load and set some attributes of the package *fancyhdr* that enables the use of headers and footers (via include of **extra1.tex**)
* set additional attributes for *fancyhdr* (via the chunk **setheader** in iris_data_set_bib1.rmd)

We have structured these LaTeX commands in three separate groups:   
* in the external permanent file **header.tex** the commands that go in the LaTeX preamble part and don't change for each document
* in the internal file **extra1.tex** the commands that go in the LaTeX preamble part and are specific for this document
* in the internal file **extra2.tex** the commands that go in front of the LaTeX body part and are specific for this document.
In our examples no contents: a candidate for inclusion would be the **\chead** command but because the header is not constant (see next paragraph) we need to use an engine_R chunk and not a engine_cat chunk.

Apart from these LaTeX changes we also show what can be done in the yaml-header:   
* define your own parameters by using the *params* keyword:
    + we use the *doc_version* parameter to include a version number in the header of each page
    + we use the *altplot* parameter to change the program flow by changing some text and including or omitting a figure.
    We do this by setting some text variables dependent on the parameter and using the parameter to decide about executing and echoing chunks.
* specify e.g. the page orientation by using the *geometry* keyword
* specify that a table of contents is to be included
* specify the *knit* command that will be executed. Here we use it to explicitly specify the name of the pdf-file and to ensure that the intermediate md-file is not removed after processing. By specifying *keep\_tex: yes* the intermediate tex-file will also be kept. This can be useful for debugging when the output is not as expected.

We also show here how to use an internal bibliography. This is a list of references that we include at the end of the document.
You can also use (and reuse) a bibliography that is stored in an external file. Handling of that is shown in the next example.

## iris_data_set_vm4.rmd
This example is the nearly the same as the previous example. The difference is that the parameter *altplot* is now set to *F* and  that the bibiography is now in an external file. The latter is convenient when you often reference the same items. The bibliography has to be specified in the yaml-header and is fully handled by Pandoc and not by the LaTeX processor. Therefore the Pandoc way of referring has to be used and not the **\cite** method.

## iris_data_set_vm5.rmd
In the last examples we used a parameter to distinguish the two cases: only one plot or with an additional plot. We used the parameter as a boolean flag for the *eval* and *echo* parameters in the *r1b* chunk. 
However each parameter in a chunk can be an R expression. We use this in the current example: we include child documents where the file name is an R expression dependent on the parameter. The example shows a table where depending on a parameter *sortorder* the observations with the highest or lowest values of the variable *Sepal.Length* are displayed.

## iris_data_set_vm6.rmd
In the previous examples we used only a limited amount of LaTeX in our code. In this example we show how to display two or more tables side by side. This is convenient (saves space and avoids turning over pages) when dealing with many small tables. The LaTeX code needed for this was contributed to [**Stack Overflow**](http://stackoverflow.com/questions/23926671/side-by-side-xtables-in-rmarkdown ) by Marcin Kosiński. 
In this example we place the two possible tables of the previous example side by side.

## iris_data_set_vm7.rmd
References to tables and figures are different for html and pdf documents. In this example we show how we can create two sets of functions (one for html and one for pdf document) that can be used for referencing figures and tables. Depending on the type of document one of these sets is used. For the tables we use the package **pander** because it allows the use of captions. For html documents the package **captioner** is used. 

## flexknit.Rmd and myknit.R
In this example we show a function that is executed when the `knit` button is clicked. This happens by specifying the **knit** option in the yaml metadata block. With this function (that has been named *myknit*) and some additional yaml options the user can specify a different name (and folder) for the output with optional version number. Also a copy of the input file with that version number can be created. In this way an output folder with various versions of the inputs and outputs can be created. The pdf can be found in the `output` folder. In June 2018 the *myknit* function was extended to allow R expression in the additional yaml options. The file *flexknit.Rmd* uses this now for the *hoqc_version* option. The new output pdf is also included in the `output` folder besides the old one that was created with `hoqc_version: _v1`. The function *myknit* is now included in package [HOQCutil](https://github.com/HanOostdijk/HOQCutil)

## include_latex.Rmd 
In this example we show the various possibilities to include LaTeX statements in the `tex` file.
The include files are dynamically created in the Rmd document. We show the yaml that we use with the *myknit* function of the previous example. An adapted version of the *pandoc* template is included. The pdf can be found in the `output` folder.   

## bookdown_HOQC.Rmd 
In this example we show how the [bookdown](https://github.com/rstudio/bookdown) package can be used to produce both `html` and `pdf` documents. Compare this with `iris_data_set_vm7.rmd`. 
The inputs are placed in the `input_bookdown` folder and the outputs in `output_bookdown`.

