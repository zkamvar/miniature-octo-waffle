# r-novice-inflammation

This repository is a way for me to test out the new features in [{sandpaper}] on
a real repository. It's derived from [Programming With
R](https://swcarpentry.github.io/r-novice-inflammation) and hopefully, I can
get it to a point where it will be identical without using Jekyll.

To render this lesson, first clone it and then use [{sandpaper}] to render it

```r
remotes::install_github("zkamvar/sandpaper", dep = TRUE)
usethis::create_from_github("zkamvar/miniature-octo-waffle", "~/Desktop")
## you will be moved to that directory

sandpaper::build_lesson()
``` 

I converted this site by using the [{dovetail}] package:

```r
remotes::install_github("zkamvar/sandpaper", dep = TRUE)
remotes::install_github("carpentries/dovetail") # contains script to convert lessons
remotes::install_github("carpentries/pegboard") # for converting 

# Download the repo from github
usethis::create_from_github("swcarpentry/r-novice-inflammation", "~/Desktop")

# convert the materials
source(system.file("convert", "convert.R", package = "dovetail"))

# Create sandpaper repo
sandpaper::create_lesson("~/Desktop/new-rni")

system("cp -R ~/Desktop/rni/_episodes_rmd/* ~/Desktop/new-rni/episodes")

## change ../fig to fig
## change latex link to latex equation
```

After that was done, I had to make a couple of changes like making sure the
hard-coded figures were no longer sourcing above their current directory, 
by changing all instances of `../fig` to `fig` and changing the reference to the
latex equation in episode 8 to actual latex code.

I could then build the lesson inside of the new-rni directory

```r
sandpaper::build_lesson()
```


[{sandpaper}]: https://github.com/zkamvar/sandpaper
[{dovetail}]: https://github.com/carpentries/dovetail
    
