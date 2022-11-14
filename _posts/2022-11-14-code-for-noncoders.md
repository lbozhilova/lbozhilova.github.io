---
title:  "Writing code for a wet-lab audience"
categories:
  - Blog
tags:
  - conferences
  - R
  - software
---

_Last month I gave a talk at an open science workshop organised by Phillip Crout at the [MRC Biostatistics Unit](https://www.mrc-bsu.cam.ac.uk/). This blog post elaborates on my talk. You can find the talk slides [here](/assets/pdfs/bsu_LyubaBozhilova_171022.pdf)._

I turned twenty-nine this year, which means it has been twenty years since I wrote my first lines of code [moving a turtle around](https://en.wikipedia.org/wiki/Logo_(programming_language)), and ten years since I started working in R. Before you get any wrong impressions, my code does not look like twenty years worth of experience have gone into it. However, it is undeniable that I am not your average R user, and neither are any of the other people who write about R or give talks about it. As Hadley Wickham [points out](https://adv-r.hadley.nz/introduction.html), _most R users are not programmers_.

Instead, they are much closer in experience to my wet-lab colleagues and collaborators of the past year: people who have a basic understanding of R and can follow a tutorial or edit a script, but who do not confidently write code from scratch themselves. These days I write anything from [libraries](https://github.com/lbozhilova/kimura) to short data analysis [scripts](https://github.com/lbozhilova/bcl2l13-mtdna-selection) and I have become increasingly aware that the people most likely to read and run my code are precisely such "non-coders".

Should I care? Perhaps not. There is no universal definition of what constitutes good code, but we generally agree that it should do what it is supposed to, do it well, and be written in such a way that it could be adapted and extended. There is little room here for the sensitivities of the less experienced. Insofar as we write code with an audience in mind, that audience tends to have a background comparable to our own.

Nevertheless, one of the reasons, albeit rarely the main reason, we share academic code is so our colleagues are able to understand and make use of it. If this is important to us, then our code serves two distinct purposes: to contribute to the research, and furthermore to communicate it. One of the lessons from the past year for me has been that effective communication of code depends hugely on the experience level of our audience, and this is something we can choose to act on.

# Nothing is obvious

One clear difference between experienced and inexperienced coders lays in their familiarity with convention. For example, in R almost every script will start with loading of packages such as
```
library("tidyverse")
library("MASS")
```
When well-known packages are used, and especially when these are available on [CRAN](https://cran.r-project.org/), as opposed to [Bioconductor](https://www.bioconductor.org/) or GitHub, it is rare to see installation instructions accompanying data analysis scripts. Attempting to load a package which has not been previously installed throws an error, like so:
```
Error in library("tidyverse") :
  there is no package called ‘tidyverse’
```
Using `require()` instead of `library()` will throw a longer and equally cryptic warning. How is this in any way cryptic, I hear you ask. _Obviously_ the package is missing, and so _obviously_ it needs to be installed. Except this is only obvious if you know that packages are separate pieces of software distributed independently of base R.

Let us instead put ourselves in the shoes of a less experienced coder. First he sees a glaring red error message, indicating that something is broken. Not encouraging, especially as these are the very first lines of code in the script! And what does it mean there is no package called `tidyverse`? There _obviously_ must be one, otherwise why would anybody be trying to load it? If our imaginary non-coder has encountered this particular error before, he might already know the solution is to run `install.packages("tidyverse")`. If he does not, he is left to the mercy of search engines and help forums.

In addition to installing and loading packages, there are a number of similar issues that might feel trivial to your average bioinformatician but can nevertheless be frustrating for a non-coder:
  - OS incompatibility (often encountered when trying to use `parallel`),
  - out-of-date software,
  - running code in the wrong directory,
  - failing to read in data.

Is it our job to teach people how to avoid such problems every time we share code? Of course not. However, we can help by quickly addressing them in a README ([example](https://github.com/lbozhilova/bcl2l13-mtdna-selection/blob/main/README.md)) or even with a small set-up script ([example](https://github.com/lbozhilova/COGENT/blob/master/tutorial/installTutorialPackages.R)). It is true that we cannot update someone's R remotely, or set their working directory for them. Still, listing dependencies is always a good idea, as is providing appropriately formatted toy raw data where full datasets cannot be readily shared alongside code.

# Errors are scary

These initial struggles can be the most frustrating for a few different reasons. That they happen in the first few lines of code, where nothing much is meant to happen, can be deeply discouraging. I think emotionally coding is much like mathematics: the joy of it often comes from a sense of accomplishment, but otherwise it can feel impenetrable and exclusive. People often end up feeling like code is too complicated and not something they can do, and this discourages them from working through error messages and reading documentation. When I get an error message I do not understand, I quickly look it up. When my colleagues do, their first reaction tends to be "it does not work".

To compound this problem, formal documentation is also often seen as inaccessible and meant for "experts" only. If you think coding is a complex activity meant for other people, and that your role is to only make small tweaks like changing file paths and sample sizes, it is natural to also think that detailed documentation is confusing rather than helpful. After all, you are not looking to understand how a piece of code works. Instead, you want to find a working piece of code you can quickly adapt.

Altogether, this makes writing code a fraught affair. Non-coders are more likely to make errors, more likely to not understand or engage with those errors, and less likely to have access to reading material.

When it comes to writing errors, warnings and documentation, perhaps we need to take the approach that you can take a horse to water but you cannot make it drink. However, I think this is a gentle reminder that fixing "trivial" errors, e.g. needing to change filepaths when restructuring a repo to make public on GitHub, in our code can be immensely helpful! And when it comes to testing code, it is an important reminder that we need to test what happens when users do not, for whatever reason, interact with our documentation. Sometimes the best person to test your code is not someone with a keen eye and systematic approach, but rather someone who does not know all that much about code in the first place, and is likely to ignore any additional reading you have provided.

# In defence of plain scripts

This brings me to an important point on documentation and literate programming, i.e. the practice of writing documentation alongside code, for example in interactive notebooks. Donald E. Knuth [defines](https://www-cs-faculty.stanford.edu/~knuth/lp.html) it like so:

> Literate programming is a methodology that combines a programming language with a documentation language, thereby making programs more robust, more portable, more easily maintained, and arguably more fun to write than programs that are written only in a high-level language. The main idea is to treat a program as a piece of literature, addressed to human beings rather than to a computer.

While I suspect Knuth did not have my particular use-case in mind, I think his point on treating code as literature is universally relevant. Often we think of literate programming simply as using Jupyter notebooks. However, in writing we recognise that form matters. If I want to write about new research which my colleagues would read, I aim to publish in a scientific journal. If I am instead writing a short story, I would not be looking for journal LaTeX templates.

I think the same applies to code. Notebooks are _fantastic_. They can make code easier to read and can help build important narrative around what it is our code does and how it achieves this... for the right audience. And non-coders are often not that audience, which blows my mind a little.

In my experience there are two types of notebooks out there: tutorials, and casual everyday Jupyter notebooks for people who work in Python. No offence to my Python colleagues but the text in the latter is most often either missing or rather sparse. Tutorials can suffer from the opposite problem. Despite the significant effort which goes into writing them, most of the time only a small proportion of tutorial users read the text. They tend to skip it altogether.

What is worse, and especially with `.Rmd` notebooks, you cannot expect people to download the interactive notebook, or even to necessarily copy and paste code along. They might not know how to, for one, as often only a static HTML or PDF version is directly shared. Moreover, people freak out at unfamiliar file extensions, and insitinctively pick ones they are familiar with. A lot of the time, they retype the code, sometimes directly in the console and not in a file to be saved for later. This process is usually frustrating, inefficient, and provides little long-term understanding.

I originally thought I might be a bit of a grumpy outlier about this, but speaking to a bunch of people who code in various fields, we were all in agreement: if you want anything read, your best strategy is to leave it as an inline comment in a plain script.

# The art of inline comments

The worst kind of script is one entirely void of comments. The second worst is when you cannot tell where one part of code ends and another begins. Are comments clarifying specific different steps? Are they separating unrelated sections of code? Who knows!

There are many ways to document code well, and I make no claims that mine are the best. However, they work for me, and I have been told they work for others, so I might as well share them. Here is the data preprocessing [script](https://github.com/lbozhilova/bcl2l13-mtdna-selection/blob/main/01-data-preprocessing.R) from our recent paper ([Laura Kremer _et al._, 2022](https://www.biorxiv.org/content/10.1101/2022.09.02.506367v1)). Like most of my scripts, it starts with a title and description. I also like to add a last-edited line, but that is more for my personal reference rather than something I expect others to care about.

```
##############################
##### Data preprocessing #####
##############################

# Last edited: 20/07/22 by LVB

# Description: Data preprocessing and summary statistics.
```

Below this header, my scripts are split in sections. Usually these start with loading packages, calling other code, and loading data. Note that the section headers are delineated less strongly than the overall title, but more strongly than regular comments.

```
#----- Packages
require("tidyverse")

#----- Code
source("00-plot-setup.R")

#----- Load data
mother_df <- read_table("data/raw/tRNAAla_mothers.txt")[, -7] # remove empty column
...
```

Within each section, I might use inline comments to explain bits of code (e.g. above `# remove empty column` refers to the otherwise cryptic `[, -7]` subset of the data frame). I might also add notes to the reader, where I expect issues might arise.

```
# Note: If using different data, make sure to check:
# - are all heteroplasmy fractions recorded
# - are they between 0 and 100
# - are there any homoplasmies that need perturbing for the log-odds calculation.
df$Frequency <- .01 * df$Frequency
df$Het_mothers <- .01 * df$Het_mothers
df$Shift <- qlogis(df$Frequency) - qlogis(df$Het_mothers)
```

Finally, I try to make effective use of empty spaces to define "paragraphs" within sections. Below, the first three "paragraphs" read in three different data frames. Afterwards, these are merged, before finally unnecessary variables are cleared at the end of the section.

```
#----- Load data
mother_df <- read_table("data/raw/tRNAAla_mothers.txt")[, -7] # remove empty column

offspring_df <- read_table("data/raw/tRNAAla_offspring.txt")
offspring_df <- filter(offspring_df, !is.na(Linear)) # remove empty rows

pyroseq_df <- read_table("data/raw/tRNAAla_pyro.txt")[, -3] # remove empty column

df <- merge(offspring_df, mother_df, by = "Dam")
colnames(df)[c(5, 13)] <- c("DoB", "DoB_mother")
df <- merge(df, pyroseq_df, by = "Linear")

rm(mother_df, offspring_df, pyroseq_df); gc()
```
The whole script is 71 lines, of which barely 34 are code. Out of curiosity I have just had a look at my code more generally and it is quite common for my scripts to be about half code. One thing I sometimes also do, although there is no good example in this particular script, is to also print expected output and put that in the comments. Sometimes this is can be the few lines produced by `table()` or `summary()` or the full output of a hypothesis test.

R allows us to write code in sections naturally, as it is not a compiled language and has a decade-long history of being run interactively, unlike Python. Another thing that helps is making sure the code itself is modular.

# Modular code

We use R for data analysis and in my head, the easiest way to think about the modularity of code is to think about different stages of the data. In this script, there are three main sections that "do stuff":
- `#----- Load data` starts with the three `.csv` files of raw data and ends with a single merged data frame `df`, containing all relevant information,
- `#----- Process data` takes `df` and enriches it with additional computed columns, and
- `#----- Summary statistics` uses the enriched `df` to calculate summary statistics which later appear in the manuscript.

People often talk about writing modular code by writing shorter functions as part of a larger pipeline. Here in this single project which has custom data formatting, it makes little sense to do so: no one will be able to use those exact functions for any other purpose. Nevertheless, each section has its own data input and (data) output.

This modular approach is something that is also true across scripts. The data pre-processing script is one of six in the repository, the longest of which is barely 113 lines of code. I suspect that the overall number of lines of R code in the repository is no more than 500, of which no more than 250 would be executable. These could have fit in a single script, and not even a very long one. To split them is a choice, and one I stand by.

# A matter of personal style

As you may have spotted if you clicked on the GitHub link above, the script I have been using as an example was called `01-data-preprocessing.R`. Right at the top it calls `00-plot-setup.R`. In fact, all scripts in the repository are numbered: there are also `02-maximal-tolerated.R`, `03-shift-analysis.R`, and so on. I do this for all of my projects.

The idea to both number scripts and give them descriptive titles is one I took from the [`tidyverse` style guide](https://style.tidyverse.org/). Style guides are collections of somewhat arbitrary conventions for writing readable code. I like the `tidyverse` one, but I do not follow it religiously. I think the script-numbering trick is particularly neat though.

It serves two purposes. The first one is to define a workflow while keeping distinct code modules in separate scripts - as a rule, I do not independently execute `00-` scripts, and run everything else in order. If you were being fancy (and perhaps you should not be), you can instead define workflows with [Snakemake](https://snakemake.readthedocs.io/en/stable/).

The second benefit of numbered scripts is numbered output. This is something I started doing for my own benefit, before I knew others would find it helpful. What I do is I make sure that every time I save something to file, the file name shares a prefix with the generating script. For example, the code for generating `figures/05-fig2.jpg` can be found in `05-figures.R`. There is nothing more frustrating than having to troll through a large repository to figure out exactly how a particular bit of analysis is carried out, and this indexing can help a lot.

Other style guide-adjacent things I tend to do include:
- using underscores instead of camel case (`day_one`, not `dayOne`),
- systematic indentation and generous spacing (`(a + b) * c`, not `(a+ b)*c`),
- adding data types to variable names (`oocytes_df` for a data frame, and `pgc_df_lt` for a list of data frames), and
- using verbs in function names (`calculate_shift()`, not `shift()`).

However, I should stress the key thing here is not following one particular set of rules and conventions, but rather having a set of rules to follow. More than anything, it is consistency that helps with clarity.

# In conclusion

This ended up being a bit of a ramble, so thank you for sticking with me so far. Perhaps my writing could use a style guide...

I hope this post helped remind you that code, like maths, can be intimidating for those who do not use it regularly. I also hope that it convinced you that it is on us to be aware of this when programming, especially when our audience is significantly less experienced than us. There are some ways in which inexperienced coders differ from experienced ones, e.g. in how they approach documentation. We can choose to act on these differences and make their lives easier. There are also ways in which good code is universally good for everyone. Writing modularly, and following a consistent style are good examples of this. And, you know, making sure our code does what it says on the tin...

Before I sign off, special thanks to Laura Kremer, Tom Barton-Owen, Fei Gao, Malwina Prater, and Shafiqur Rahman for helpful discussions on this topic. And to everyone at the workshop who nodded in agreement. I love a bit of validation.
