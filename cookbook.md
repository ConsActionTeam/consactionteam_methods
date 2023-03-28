ConsActionTeam Cookbook
================

This document exists to share institutional knowledge for obstacles that
are commonly run into by Pinsky Lab Members. The struggle is real\!

# Github

## Archiving a Git repo with Zenodo for a publication
This will likely evolve, but our current approach is to

1. Mint a release in github with the latest state (instructions on making release [here](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository))
2. Clean out all the extraneous stuff
3. Make sure the Readme is very, very clear about which files were used to create which figures, tables, results, etc., and where to get any data not in the repo.
4. Have someone not involved with the project look at the repo and Readme. Is everything very clear?
5. Mint a publication-ready release and link this to Zenodo (instructions [here](https://guides.github.com/activities/citable-code/))

Some of our examples: 
- https://github.com/pinskylab/hotWater
- https://github.com/pinskylab/ClimateAndMSP
- https://github.com/pinskylab/Clownfish_persistence

## How to add undergrads to the Pinsky Lab github.

1.  Make sure the undergrad has signed up for github and note whatever
    email they used to sign up.
2.  Create the repository on github, this should be the project that
    they are working on.
3.  Invite the undergrad as a collaborator.

## How to get RStudio and github to talk to each other

1.  Create a repository on github, copy the URL.
2.  Create a new project in RStudio and paste in the URL from github

## Setting up SSH access from RStudio to Github
see switch to ssh access: https://help.github.com/en/github/using-git/changing-a-remotes-url
1. git remote set-url origin git@github.com:pinskylab/NAMEOFREPO.git
1. git remote -v # to check
1. You may also need to set up keys. See internet instructions for how to do this in RStudio and on your computer or on the server.

## How to move an existing project on github into RStudio

1.  Copy the URL from github for your repository.
2.  On your computer, add “\_old” to the end of the name of your folder.
3.  Create a new project in Rstudio and paste in the URL from github
    with the normal folder name.
4.  Open the “old” folder, copy all, open the new folder, paste
all.

## How to transfer ownership of a repository from personal to consactionteam in github

1.  Click on the repository in github
2.  Click on “settings” in the upper middle right.
3.  Scroll down to click on “transfer ownership”.


## If RStudio is not recognizing git on your machine

[Try this helpful set of
steps](https://happygitwithr.com/rstudio-see-git.html)

## How to use the command line to connect your current folder to an existing github repo.

1.  Open command line and navigate to the folder you want to connect to
    your github repo.
2.  On the command line, type:

<!-- end list -->

``` bash
# set the new remote
git remote add origin url_of_github_repo

# verify that it worked
git remote -v 

# pull from the remote
git pull origin master

# put to the remote
git push origin master
```

## If you made a commit that won’t push to Github (too large files or something else)…

  - Make a copy of your new files/versions and put the copy in a
    different folder, like your Desktop.

  - on the command line, type `git reset --soft origin/master` or `git
    reset --mixed` or `git reset --hard` depending on how nuclear you
    need to go with your reset, “hard” will delete the most information.
    
      - replace master with whatever branch you are working on if it is
        not the master.
      - This command restores your local version of the repository to be
        identical with the web version.
      - You still need to figure out what to do with the new versions
        that won’t push to github.  
      - Good advice [about the 3 trees of
        git](https://www.atlassian.com/git/tutorials/undoing-changes/git-reset).

## Call data from a GitHub repo directly into R

The url was copied by right clicking on “View Raw” for the data file in
question and selecting “Copy Link”

``` r
# download the file
download.file(url = "https://github.com/pinskylab/genomics/blob/master/data/fish-obs.RData?raw=true", destfile = "fish-obs.RData")

# read in the data
fish_obs <- readRDS("fish-obs.RData")

# delete the file
file.remove("fish-obs.RData")
```

## Allow others to view html versions of your code.

If you want to share plots with collaborators quickly, one way is using
an Rmarkdown (Rmd) document and html output. 1. Create a branch called
gh-pages.  
2\. Checkout that branch in RStudio.  
3\. Knit your Rmd to html using output: html\_document or
html\_notebook. 4. Push your html document to github. 5. Wait for github
to process the new web page (this could take 5 minutes). 6. Go to
your-github-username.github.io/your-repo-name/html-file-name.html.

# R

## How to install an older version of a package than the one currently installed.

Based on this
[article](https://support.rstudio.com/hc/en-us/articles/219949047-Installing-older-versions-of-packages)

``` r
devtools::install_version("ggplot2", version = "0.9.1", repos = "http://cran.us.r-project.org")

packageurl <- "https://github.com/thomasp85/gganimate/archive/v0.1.1.tar.gz"
install.packages(packageurl, repos=NULL, type="source")

packageloc <- "~/Downloads/gganimate-0.1.1/"
install.packages(packageloc, repos = NULL, type="source")
```

## Convert a table from long format to wide format or vice versa

Tutorial slides are
[here](https://speakerdeck.com/yutannihilation/a-graphical-introduction-to-tidyrs-pivot-star).

More info
[here](http://www.storybench.org/pivoting-data-from-columns-to-rows-and-back-in-the-tidyverse/)

Make sure you are running at least tidyr 1.0.0 to use this version,
which is an update from spread and gather. This is great if you want to
convert row values into column headers or column headers into row
values. MRS is testing using it to add absence data in presence absence
data sets.

## Working with strings

How to find and replace a digit/character combination using stringr (in
the tidyverse library). In this example, we have used parentheses to
create 2 groups and we are replacing the whole with only the second
group

``` r
library(stringr)
str_view(fruit, "(..)\\1", match = T)

#test <- "0_5" - should return 5
test <- "10_15" - should return 15

str_replace(test, "(\\d+\\_)(\\d+)", "\\2")

# Can use str_view(test, "(\\d+\\_)(\\d+)") to see what stringr is seeing.
```

# R Spatial

## How to calculate the distance between two sets of points

``` r
# create pairwise distance matrix between two sets of points (list1, list2) in meters using the function distVincentyEllipsoid
# the WGS84 ellipsoid is used by default, as it's the best available global ellipsoid
# can change the a, b, and f parameters for a desired ellipsoid
# see ?distVincentyEllipsoid for details
library(geosphere)
mat <- distm(list1[,c('longitude','latitude')], list2[,c('longitude','latitude')], fun=distVincentyEllipsoid)

# Create a distance column in meters from a data.frame that has both points 
loc.df$dist <- distGeo(loc.df[,c('lon1', 'lat1')], loc.df[,c('lon2', 'lat2')])
```

See
[script](https://github.com/pinskylab/Phils_GIS_R/blob/master/scripts/calc_distance_matrix.R)

# Plots

## How to change the angle of your x-axis labels in ggplot

MRS found the answer
[here](https://www.datanovia.com/en/blog/ggplot-axis-ticks-set-and-rotate-text-labels/)
where p is the plot you’ve been working on

``` r
p + theme(axis.text.x = element_text(angle = 90))
```

## How to save an R plot as a pdf (when using ggplot) - 3 different ways

  - Save the last plot created into working directory.

<!-- end list -->

``` r
ggplot2::ggsave("filename.pdf")
```

  - Save the object called plot into the working directory.

<!-- end list -->

``` r
ggplot2::ggsave(plot, "filename.pdf")
```

  - Save the object called plot into the plots folder that is closest to
    your working directory.

<!-- end list -->

``` r
ggplot2::ggsave(plot, here::here("plots", "filename.pdf"))
```

