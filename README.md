# GSoC-2024-Test-Results
Converting Sweave to Rmarkdown using texor package

# Easy

![A sample image](LaTeX article to HTML.png)

Q:Convert LaTex article article to HTML

Below code snippet show how that works and specifically converting them for web
use and managing bibliographies
```{r setup, include=FALSE }

library('texor')
library(rebib)

## install.packages("remotes")
remotes::install_github("Abhi-1U/rebib")

remotes::install_github("Abhi-1U/texor")

# To check the version of pandoc
rmarkdown::pandoc_version()
```


```{r }
article_dir <- ("D:\\GSoC Research\\Mentor test - Converting Sweave to Rmarkdown 
using texor package\\Test\\git_repo\\supplement\\math-env")
#texor::latex_to_web(article_dir)


```




# Medium

Q: Write an R function that takes an example Markdown file with multiple figures 
and converts it to HTML

## Sample Images That Used


![A sample image](Medium/penguins.png)![Another sample image](Medium/Rlogo-5.png)

The function is to provide a perfect way to convert Markdown documents to HTML 
format using R, with the additional ability to apply custom transformations via 
Lua filters. This can be  useful for batch processing of documents, applying 
consistent formatting rules, or integrate custom processing steps that are not
directly supported by Markdown or HTML.
After completing conversion it confirm with text message.


```{r results='asis'}
#' @param markdown_file Path to the Markdown file to be converted.
#' @param lua_filter_path Path to the Lua filter file for image numbering (optional).
#' @param output_file Path where the HTML output should be saved.
#' @return Invisible NULL. The function prints a message indicating whether 
#' the conversion was successful or if it failed.
#' @examples
#' @export

convert_md_to_html_with_lua_filter <- 
  function(markdown_file, lua_filter_path = NULL, output_file = NULL) {
  if (is.null(output_file)) {
    output_file <- gsub("\\.md$", ".html", markdown_file)
  }
  
  pandoc_args <- c("--from","markdown", "--to","html5", "--output",output_file)
  
  if (!is.null(lua_filter_path)) {
    pandoc_args <- c(pandoc_args, "--lua-filter", lua_filter_path)
  }
  
  # === Attempt the conversion and catch any errors ===
  tryCatch({
    rmarkdown::pandoc_convert(markdown_file, options = pandoc_args)
    cat("Conversion completed successfully: ", output_file, "\n")
  }, error = function(e) {
    cat("Conversion failed with error: ", e$message, "\n")
  })
  
 
  invisible(NULL)
}

convert_md_to_html_with_lua_filter("example.md", "image_numbering_filter.lua", 
                                   "filtered-example.html")

```




# Hard

Q: Write a Custom Pandoc Reader in Lua to just extract code chunks from Sweave files

This allows for custom processing of documents where code blocks and text follow
a specific notation that's not natively recognized by Pandoc.


## Example 1

```{r echo=TRUE, warning=FALSE}

library(tools) 

sweave_file <- "Hard/030-landscape.Rnw"

lua_reader <- "sweave_reader.lua"

output_format <- "native"

# === Pandoc command for results ===
pandoc_command <- paste("pandoc", sweave_file, "-f", lua_reader, "-t", output_format)

output <- system(pandoc_command, intern = TRUE)

cat(output, sep = "\n")

```

## Example 2

```{r echo=FALSE, warning=FALSE}
library(tools)  



sweave_file <- "Hard/031-noindent.Rnw"


lua_reader <- "sweave_reader.lua"

output_format <- "native"

pandoc_command <- paste("pandoc", sweave_file, "-f", lua_reader, "-t", output_format)

output <- system(pandoc_command, intern = TRUE)

# Print the result
cat(output, sep = "\n")

```

## Example 3

```{r echo=FALSE, warning=FALSE}
library(tools)  

sweave_file <- "Hard/032-persp.Rnw"

lua_reader <- "sweave_reader.lua"

output_format <- "native"

# === Pandoc command for results ===
pandoc_command <- paste("pandoc", sweave_file, "-f", lua_reader, "-t", output_format)

output <- system(pandoc_command, intern = TRUE)

cat(output, sep = "\n")

```
