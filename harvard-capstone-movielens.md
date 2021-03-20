---
title: "Movielens  \n Movie Recommendation System  \n A Harvard Capstone Project"
author: "Manoj Bijoor"
email: manoj.bijoor@gmail.com
date: "March 19, 2021"
output: 
  pdf_document: 
    latex_engine: pdflatex
    number_sections: yes
    keep_tex: yes
    keep_md: yes
    df_print: kable
    highlight: pygments
    extra_dependencies: "subfig"
  md_document:
    variant: markdown_github 
    # check https://bookdown.org/yihui/rmarkdown/markdown-document.html#markdown-variants
  github_document:
    toc: true
    toc_depth: 5
    pandoc_args: --webtex
    # pandoc_args: ['--lua-filter', 'math-github.lua']
  html_document: 
    keep_md: true
    code_folding: hide
# urlcolor: blue
# linkcolor: blue
#citecolor: blue
#geometry: margin=1in
links-as-notes: true
header-includes:
  \usepackage[utf8]{inputenc}
  \usepackage[english]{babel}
  \usepackage{bookmark}
  \usepackage[]{hyperref}
  \hypersetup{
    backref,
    pdftitle={"Movielens Harvard Capstone"},
    bookmarks=true,
    bookmarksnumbered=true,
    bookmarksopen=true,
    bookmarksopenlevel=3,
    pdfpagemode=FullScreen,
    pdfstartpage=1,
    hyperindex=true,
    pageanchor=true,
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan
    }
  \usepackage{amsmath}
  \usepackage{pdflscape}
  \usepackage[titles]{tocloft}
  \usepackage{tocloft}
  \usepackage{titlesec}
  \usepackage{longtable}
  \usepackage{xpatch}
  \usepackage[T1]{fontenc}
  \usepackage{imakeidx}
  \makeindex[columns=3, title=Alphabetical Index, intoc]

  # \usepackage{amssymb}
  # \usepackage{mathtools}
  # \usepackage{unicode-math}
  # \usepackage{fontspec}
  # \usepackage{letltxmacro}%
  # \usepackage{float}
  # \usepackage{flafter}
  # \usepackage[titles]{tocloft}
---



<!-- #```{r, time_it = TRUE, eval=FALSE, echo=FALSE} -->
<!-- # Sys.sleep(2) -->
<!-- #``` -->



<!-- ------------------------------ -->

\bookmark[dest=TitlePage]{Title Page}

\pagenumbering{roman}     <!-- first page with Roman numbering -->

\newpage                  <!-- new page -->

<!-- ------------------------------ -->

\newpage 

\begin{center}

\hypertarget{Abstract}{}
\large{Abstract}
\bookmark[dest=Abstract]{Abstract}

\end{center}

\bigskip

...this is the abstract text...

<!-- ------------------------------ -->

\newpage 
\clearpage
\phantomsection
\setcounter{secnumdepth}{5}
\setcounter{tocdepth}{5}

\cleardoublepage <!-- ensure that the hypertarget is on the same page as the TOC heading -->
\hypertarget{toc}{} <!-- set the hypertarget -->
\bookmark[dest=toc,level=chapter]{\contentsname}
\tableofcontents

\clearpage

<!-- ------------------------------ -->
<!-- \renewcommand{\theHsection}{\thepart.section.\thesection} -->

\newpage
\clearpage
\phantomsection
# List of tables{-}
\renewcommand{\listtablename}{} <!-- removes default section name -->

\listoftables
\clearpage

\newpage
\clearpage
\phantomsection
# List of figures{-}
\renewcommand{\listfigurename}{}

\listoffigures
\clearpage

\newpage
\clearpage
\phantomsection
\newcommand{\listequationsname}{List of Equations}
\newlistof{equations}{equ}{\listequationsname}
\newcommand{\equations}[1]{%
\refstepcounter{equations}
\addcontentsline{equ}{equations}{ \protect\numberline{\theequations}#1}\par}
\xpretocmd{\listofequations}{\addcontentsline{toc}{section}{\listequationsname}}{}{}

\renewcommand{\listequationsname}{}

\listofequations
\clearpage

<!-- ------------------------------ -->

\newpage

\pagenumbering{arabic} 

<!-- ------------------------------ -->


# Project Overview: MovieLens - A Harvard Capstone Project  
A movie recommendation system using the MovieLens dataset.  

For this project, I will be creating a movie recommendation system using the MovieLens dataset, provided by [GroupLens Research](https://grouplens.org/), a research lab in the Department of Computer Science and Engineering at the University of Minnesota, Twin Cities specializing in recommender systems, online communities, mobile and ubiquitous technologies, digital libraries, and local geographic information systems.

[GroupLens Research](https://grouplens.org/datasets/movielens/) has collected and made available rating data sets from the [MovieLens web site](https://movielens.org). The data sets were collected over various periods of time, depending on the size of the set.

I will use the [10M version of the MovieLens dataset](https://grouplens.org/datasets/movielens/10m/) to make the computation a little easier.

**First**, I will download the MovieLens data and run code provided to generate my datasets.

**Second**, I will train a machine learning algorithm using the inputs in one subset to predict movie ratings in the validation set.  


## Create Train and Final Hold-out Test Sets  
I will develop my algorithm using the edx set. For a final test of my final algorithm, I predict movie ratings in the validation set (the final hold-out test set) as if they were unknown. [RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation) will be used to evaluate how close my predictions are to the true values in the validation set (the final hold-out test set). My target is RMSE < 0.86490. 


### Important: Data sets usage  
The validation data (the final hold-out test set) will NOT be used for training, developing, or selecting my algorithm and it will ONLY be used for evaluating the RMSE of my final algorithm. The final hold-out test set will only be used at the end of my project with my final model. It will not be used to test the RMSE of multiple models during model development. I will split the edx data into separate training and test sets to design and test my algorithm.   


## Final Product  
### My submission for this project is three files:  
1. My report in Rmd format
2. My report in PDF format (knit from my Rmd file)
3. A script in R format that generates my predicted movie ratings and RMSE score (contains all code and comments for my project)  
  
The report documents the analysis and presents the findings, along with supporting statistics and figures. The report assumes that the reader is not familiar with the project or the data. The report includes the RMSE generated and the following sections:

1. an introduction/overview/executive summary section that describes the dataset and summarizes the goal of the project and key steps that were performed

2. a methods/analysis section that explains the process and techniques used, including data cleaning, data exploration and visualization, insights gained, and my modeling approach

3. a results section that presents the modeling results and discusses the model performance

4. a conclusion section that gives a brief summary of the report, its limitations and future work  



\newpage
# Exploratory Data Analysis

## Data Wrangling

[Data wrangling](https://en.wikipedia.org/wiki/Data_wrangling), sometimes referred to as data munging, is the process of transforming and mapping data from one "raw" data form into another format with the intent of making it more appropriate and valuable for a variety of downstream purposes such as analytics.

The main steps could be described as follows:  
1. Discovering  
2. Structuring  
3. Cleaning  
4. Enriching  
5. Validating  
6. Publishing

Let's perform the steps or combinations thereof starting with Initial data Exploration & Visualization in the next few subsections.

### Initial data Exploration & Visualization

The first ten rows out of 9000055 rows of the Movielens data can be found in Table \ref{tbl:movielens_data}.

\begin{table}[H]

\caption{\label{tab:eda_1}Movielens data\label{tbl:movielens_data}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{rrrrll}
\toprule
userId & movieId & rating & timestamp & title & genres\\
\midrule
1 & 122 & 5 & 838985046 & Boomerang (1992) & Comedy|Romance\\
1 & 185 & 5 & 838983525 & Net, The (1995) & Action|Crime|Thriller\\
1 & 292 & 5 & 838983421 & Outbreak (1995) & Action|Drama|Sci-Fi|Thriller\\
1 & 316 & 5 & 838983392 & Stargate (1994) & Action|Adventure|Sci-Fi\\
1 & 329 & 5 & 838983392 & Star Trek: Generations (1994) & Action|Adventure|Drama|Sci-Fi\\
1 & 355 & 5 & 838984474 & Flintstones, The (1994) & Children|Comedy|Fantasy\\
1 & 356 & 5 & 838983653 & Forrest Gump (1994) & Comedy|Drama|Romance|War\\
1 & 362 & 5 & 838984885 & Jungle Book, The (1994) & Adventure|Children|Romance\\
1 & 364 & 5 & 838983707 & Lion King, The (1994) & Adventure|Animation|Children|Drama|Musical\\
1 & 370 & 5 & 838984596 & Naked Gun 33 1/3: The Final Insult (1994) & Action|Comedy\\
\bottomrule
\end{tabular}
\end{table}

Each row represents a rating given by one user to one movie.

We can see the number of unique users that provided ratings and how many unique movies were rated. The unique genres here is based on a column called "genres" that includes every genre that applies to the movie. Some movies fall under several genres. Defining a category as whatever combination appears in this column, we are going to refer to this category as "unique genres" or simply "genres" from now on.  

The number of unique users, movies and genres can be found in Table \ref{tbl:uniq_users_movies_genres}.

\begin{table}[H]

\caption{\label{tab:eda_2}Unique Users, Movies and Genres\label{tbl:uniq_users_movies_genres}}
\centering
\begin{tabular}[t]{rrr}
\toprule
unique\_users & unique\_movies & unique\_genres\\
\midrule
69878 & 10677 & 797\\
\bottomrule
\end{tabular}
\end{table}

If we multiply the number of unique users by number of unique movies, we get a very large number, actually 746087406, yet our data table has 9000055 rows. This implies that not every user rated every movie. So we can think of these data as a very large matrix, with users on the rows and movies on the columns, with many empty cells. The 'gather' function permits us to convert it to this format, but if we try it for the entire matrix, it will crash R.

Let's show the matrix of seven users ie: userId's 13-20 and four movies in Table \ref{tbl:matrix_seven_users_four_movies}.

\begin{table}[H]

\caption{\label{tab:eda_3}Matrix of seven users and four movies\label{tbl:matrix_seven_users_four_movies}}
\centering
\fontsize{8}{10}\selectfont
\begin{tabular}[t]{rrrrr}
\toprule
userId & Forrest Gump (1994) & Jurassic Park (1993) & Pulp Fiction (1994) & Silence of the Lambs, The (1991)\\
\midrule
13 & NA & NA & 4 & NA\\
16 & NA & 3 & NA & NA\\
17 & NA & NA & NA & 5\\
18 & NA & 3 & 5 & 5\\
19 & 4 & 1 & NA & NA\\
\bottomrule
\end{tabular}
\end{table}

\newpage
#### A Very Sparse Matrix

You can think of the task of a recommendation system as filling in the 'NAs' in the table above. To see how sparse the matrix is, here is the matrix in Figure \ref{fig:matr} for a random sample of 100 movies and 100 users with yellow indicating a user/movie combination for which we have a rating.

![A Very Sparse Matrix\label{fig:matr}](figures/eda_4-1.pdf) 

This machine learning challenge is quite complicated, because each outcome $Y$ has a different set of predictors. To see this, note that if we are predicting the rating for movie *i* by user *u*, in principle, all other ratings related to movie *i* and by user *u* may be used as predictors, but different users rate different movies and a different number of movies. Furthermore, we may be able to use information from other movies that we have determined are similar to movie *i* or from users determined to be similar to user *u*. In essence, the entire matrix can be used as predictors for each cell.

\newpage
#### General Properties of the data

Let's look at some of the **_general properties_** of the data to better understand the challenges.

The **_first thing_** we notice is that some movies get rated more than others. Figure \ref{fig:movies_getting_rated} shows the Movies getting rated distribution. This should not surprise us given that there are blockbuster movies watched by millions and artsy, independent movies watched by just a few:

![Movies getting rated distribution\label{fig:movies_getting_rated}](figures/eda_5-1.pdf) 

\newpage
Our **_second observation_** is that some users are more active than others at rating movies. Figure \ref{fig:users_rating_movies} shows Users rating movies distribution:

![Users rating movies distribution\label{fig:users_rating_movies}](figures/eda_6-1.pdf) 

\newpage  

### Further data Exploration, Visualization & Modification  

#### Modify edx {#modify_edx}

**Convert timestamp** in Movielens edx data Table \ref{tbl:movielens_data} into date-time, a more readable and useful format named _rating_date_ in Table \ref{tbl:movielens_edx_data_rating_date} below.


\begin{table}[H]

\caption{\label{tab:dw_1}Movielens edx data with rating date-time\label{tbl:movielens_edx_data_rating_date}}
\centering
\fontsize{8}{10}\selectfont
\begin{tabular}[t]{rrrlll}
\toprule
userId & movieId & rating & title & genres & rating\_date\\
\midrule
1 & 122 & 5 & Boomerang (1992) & Comedy|Romance & 1996-08-02 11:24:06\\
1 & 185 & 5 & Net, The (1995) & Action|Crime|Thriller & 1996-08-02 10:58:45\\
1 & 292 & 5 & Outbreak (1995) & Action|Drama|Sci-Fi|Thriller & 1996-08-02 10:57:01\\
1 & 316 & 5 & Stargate (1994) & Action|Adventure|Sci-Fi & 1996-08-02 10:56:32\\
1 & 329 & 5 & Star Trek: Generations (1994) & Action|Adventure|Drama|Sci-Fi & 1996-08-02 10:56:32\\
\bottomrule
\end{tabular}
\end{table}

**Split title** in Movielens edx data Table \ref{tbl:movielens_data} into title and year movie released, a more useful format named _movie_dt_ in Table \ref{tbl:movielens_edx_movie_release_date} below.  

\begin{table}[H]

\caption{\label{tab:dw_2}Movielens edx data with movie release date\label{tbl:movielens_edx_movie_release_date}}
\centering
\fontsize{8}{10}\selectfont
\begin{tabular}[t]{rrrlllr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt\\
\midrule
1 & 122 & 5 & Boomerang & Comedy|Romance & 1996-08-02 11:24:06 & 1992\\
1 & 185 & 5 & Net, The & Action|Crime|Thriller & 1996-08-02 10:58:45 & 1995\\
1 & 292 & 5 & Outbreak & Action|Drama|Sci-Fi|Thriller & 1996-08-02 10:57:01 & 1995\\
1 & 316 & 5 & Stargate & Action|Adventure|Sci-Fi & 1996-08-02 10:56:32 & 1994\\
1 & 329 & 5 & Star Trek: Generations & Action|Adventure|Drama|Sci-Fi & 1996-08-02 10:56:32 & 1994\\
\bottomrule
\end{tabular}
\end{table}

\newpage  

#### Modify validation, repeat above steps

**Convert timestamp** in Movielens validation data Table \ref{tbl:movielens_data} into date-time, a more readable and useful format named _rating_date_ in Table \ref{tbl:movielens_val_data_rating_date} below.  

\begin{table}[H]

\caption{\label{tab:dw_3}Movielens validation data with rating date-time\label{tbl:movielens_val_data_rating_date}}
\centering
\fontsize{8}{10}\selectfont
\begin{tabular}[t]{rrrlll}
\toprule
userId & movieId & rating & title & genres & rating\_date\\
\midrule
1 & 231 & 5 & Dumb \& Dumber (1994) & Comedy & 1996-08-02 10:56:32\\
1 & 480 & 5 & Jurassic Park (1993) & Action|Adventure|Sci-Fi|Thriller & 1996-08-02 11:00:53\\
1 & 586 & 5 & Home Alone (1990) & Children|Comedy & 1996-08-02 11:07:48\\
2 & 151 & 3 & Rob Roy (1995) & Action|Drama|Romance|War & 1997-07-07 03:34:10\\
2 & 858 & 2 & Godfather, The (1972) & Crime|Drama & 1997-07-07 03:20:45\\
\bottomrule
\end{tabular}
\end{table}

**Split title** in Movielens validation data Table \ref{tbl:movielens_data} into title and year movie released, a more useful format named _movie_dt_ in Table \ref{tbl:movielens_val_movie_release_date} below.  

\begin{table}[H]

\caption{\label{tab:dw_4}Movielens validation data with movie release date\label{tbl:movielens_val_movie_release_date}}
\centering
\fontsize{8}{10}\selectfont
\begin{tabular}[t]{rrrlllr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt\\
\midrule
1 & 231 & 5 & Dumb \& Dumber & Comedy & 1996-08-02 10:56:32 & 1994\\
1 & 480 & 5 & Jurassic Park & Action|Adventure|Sci-Fi|Thriller & 1996-08-02 11:00:53 & 1993\\
1 & 586 & 5 & Home Alone & Children|Comedy & 1996-08-02 11:07:48 & 1990\\
2 & 151 & 3 & Rob Roy & Action|Drama|Romance|War & 1997-07-07 03:34:10 & 1995\\
2 & 858 & 2 & Godfather, The & Crime|Drama & 1997-07-07 03:20:45 & 1972\\
\bottomrule
\end{tabular}
\end{table}

\newpage
#### Genres combinations per movie - a closer look   

The movielens data Table \ref{tbl:movielens_edx_avg_rating_time_effect} also has a genres column. This column includes every genre that applies to the movie. Some movies fall under several genres. We define a category of genres as whatever combination of genres appears in this column, and refer to it as simply "genres". 

Here we keep only categories with more than 1,000 ratings. Then compute the average and standard error for each category, and plot these as error bar plots. See Figure \ref{fig:genres_error_bar_plots} : 

![Movies genres error bar plots\label{fig:genres_error_bar_plots}](figures/gnr-1.pdf) 

The plot shows strong evidence of a genre effect.


\newpage
#### Movie Rating Date-Time - a closer look  

The Movielens edx data Table \ref{tbl:movielens_data}  also includes a time stamp. This variable represents the time and date in which the rating was provided. The units are seconds since January 1, 1970. We create a new column date with the date named _rating_date_ in subsection [Modify edx](#modify_edx) to get Table \ref{tbl:movielens_edx_movie_release_date} . 

We compute the average rating for each week and plot this average against day. See Figure \ref{fig:movies_average_ratings_for_each_week_versus_day} : 


```
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![Movies average ratings for each week versus day\label{fig:movies_average_ratings_for_each_week_versus_day}](figures/rd_1-1.pdf) 

The plot shows some evidence of a time effect. If we define $d_{u,i}$ as the day for user's *u* rating of movie *i*, then the following model given by Equation \ref{eq:EqTimeEffect} is most appropriate:

\equations{Movie Rating Date-Time effect Equation \ref{eq:EqTimeEffect}}
\label{eq:EqTimeEffect}
\begin{equation}
Y_{u,i} = \mu + b_{i} + b_{u}  + f(d_{u,i})+ \epsilon_{u,i}\text{, with f a smooth function of }d_{u,i}
\end{equation}

**_Modify edx_**
Let's update the **_edx_** table with a new column for the average rating for each week and another column for the day rounded to the nearest value of the week to get Table \ref{tbl:movielens_edx_avg_rating_time_effect} below:

\begin{table}[H]

\caption{\label{tab:rd_2}Movielens edx data with average rating due to rating time effect\label{tbl:movielens_edx_avg_rating_time_effect}}
\centering
\fontsize{6}{8}\selectfont
\begin{tabular}[t]{rrrlllrlr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt & date & avg\_rating\\
\midrule
1 & 122 & 5 & Boomerang & Comedy|Romance & 1996-08-02 11:24:06 & 1992 & 1996-08-04 & 3.538801\\
1 & 185 & 5 & Net, The & Action|Crime|Thriller & 1996-08-02 10:58:45 & 1995 & 1996-08-04 & 3.538801\\
1 & 292 & 5 & Outbreak & Action|Drama|Sci-Fi|Thriller & 1996-08-02 10:57:01 & 1995 & 1996-08-04 & 3.538801\\
1 & 316 & 5 & Stargate & Action|Adventure|Sci-Fi & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.538801\\
1 & 329 & 5 & Star Trek: Generations & Action|Adventure|Drama|Sci-Fi & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.538801\\
\bottomrule
\end{tabular}
\end{table}

**TODO: Repeat above for validation data as well and somehow add this to the modelling section**

**_Modify validation_**
We need to do the above **_avg_rating_time_effect_** update for the validation data as well.
Let's update the **_validation_** table to get Table \ref{tbl:movielens_validation_avg_rating_time_effect} below:

\begin{table}[H]

\caption{\label{tab:rd_3}Movielens validation data with average rating due to rating time effect\label{tbl:movielens_validation_avg_rating_time_effect}}
\centering
\fontsize{6}{8}\selectfont
\begin{tabular}[t]{rrrlllrlr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt & date & avg\_rating\\
\midrule
1 & 231 & 5 & Dumb \& Dumber & Comedy & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.555820\\
1 & 480 & 5 & Jurassic Park & Action|Adventure|Sci-Fi|Thriller & 1996-08-02 11:00:53 & 1993 & 1996-08-04 & 3.555820\\
1 & 586 & 5 & Home Alone & Children|Comedy & 1996-08-02 11:07:48 & 1990 & 1996-08-04 & 3.555820\\
2 & 151 & 3 & Rob Roy & Action|Drama|Romance|War & 1997-07-07 03:34:10 & 1995 & 1997-07-06 & 3.606571\\
2 & 858 & 2 & Godfather, The & Crime|Drama & 1997-07-07 03:20:45 & 1972 & 1997-07-06 & 3.606571\\
\bottomrule
\end{tabular}
\end{table}

\newpage

#### Movie Release Date - a closer look

Computing the number of ratings for each movie and then plotting it against the year the movie came out, that is the release date and using the square root transformation on the counts using Table \ref{tbl:movielens_edx_movie_release_date} , we get see Figure \ref{fig:ratings_movie_release_date_all_dates} :

**TODO: Align images**
<!-- # ```{r, md_1, echo=FALSE, eval=TRUE, comment="", fig.pos="h!", fig.show="hold", out.width="50%", fig.cap="Ratings Movie Release Date - All dates\\label{fig:ratings_movie_release_date_all_dates}"} -->

\begin{figure}[h!]

{\centering \subfloat[All data points only\label{fig:md_1-1}]{\includegraphics[width=0.7\linewidth]{figures/md_1-1} }\newline\subfloat[Smooth line through all data points\label{fig:md_1-2}]{\includegraphics[width=0.7\linewidth]{figures/md_1-2} }

}

\caption{Ratings Movie Release Date - All dates\label{fig:ratings_movie_release_date_all_dates}}\label{fig:md_1}
\end{figure}

we see that, on average, movies that came out after 1993 get more ratings. We also see that with newer movies, starting in 1993, the number of ratings decreases with year: the more recent a movie is, the less time users have had to rate it.

\newpage
Among movies that came out in 1993 or later, we select the top 25 movies with the highest average number of ratings per year (n/year) and calculate the average rating of each of them. To calculate number of ratings per year, use 2018 as the end year. See Figure \ref{fig:25_movies_avg_and_most_ratings_per_year_post_1993} :


```
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![25 Movies with the most ratings per year and their average rating post 1993\label{fig:25_movies_avg_and_most_ratings_per_year_post_1993}](figures/md_2-1.pdf) 

\newpage
We see that the most rated movies tend to have above average ratings. This is not surprising: more people watch popular movies. To confirm this, we stratify the post 1993 movies by ratings per year and compute their average ratings. Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993} is a plot of average ratings versus ratings per year showing an estimate of the trend.

We see that the more a movie is rated, the higher the rating.

**Post-1993 movies**   

```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year post  1993\label{fig:movies_average_ratings_versus_ratings_per_year_post_1993}](figures/md_3-1.pdf) 

\newpage
**Pre-1993 movies**  
Compare Pre-1993 movies trend shown here in Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_pre_1993} Versus Post-1993 movies trend in Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993} above.


```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year pre  1993\label{fig:movies_average_ratings_versus_ratings_per_year_pre_1993}](figures/md_4-1.pdf) 


\newpage
**_Modify edx data for Release Date Effect_**
We stratify the movies by ratings per year and compute their average ratings based on what we learnt above, where we confirmed our intuition that more people watch popular movies. Finally edx data table looks as shown in Table \ref{tbl:movielens_edx_avg_release_date_effect} below. Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_for_all_years_for_edx} is a plot of average ratings versus ratings per year showing an estimate of the trend.

**_All Years_**

```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year for all years for edx\label{fig:movies_average_ratings_versus_ratings_per_year_for_all_years_for_edx}](figures/md_5-1.pdf) 


\begin{table}[H]

\caption{\label{tab:md_6}Movielens edx data with average rating due to release date effect\label{tbl:movielens_edx_avg_release_date_effect}}
\centering
\fontsize{4}{6}\selectfont
\begin{tabular}[t]{rrrlllrlrrrrr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt & date & avg\_rating & avg\_rating\_rel & n & years & n\_year\\
\midrule
1 & 122 & 5 & Boomerang & Comedy|Romance & 1996-08-02 11:24:06 & 1992 & 1996-08-04 & 3.538801 & 2.858586 & 2178 & 26 & 84\\
1 & 185 & 5 & Net, The & Action|Crime|Thriller & 1996-08-02 10:58:45 & 1995 & 1996-08-04 & 3.538801 & 3.129334 & 13469 & 23 & 586\\
1 & 292 & 5 & Outbreak & Action|Drama|Sci-Fi|Thriller & 1996-08-02 10:57:01 & 1995 & 1996-08-04 & 3.538801 & 3.418011 & 14447 & 23 & 628\\
1 & 316 & 5 & Stargate & Action|Adventure|Sci-Fi & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.538801 & 3.349677 & 17030 & 24 & 710\\
1 & 329 & 5 & Star Trek: Generations & Action|Adventure|Drama|Sci-Fi & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.538801 & 3.337457 & 14550 & 24 & 606\\
\bottomrule
\end{tabular}
\end{table}

```
tibble [9,000,055 x 13] (S3: tbl_df/tbl/data.frame)
 $ userId        : int [1:9000055] 1 1 1 1 1 1 1 1 1 1 ...
 $ movieId       : num [1:9000055] 122 185 292 316 329 355 356 362 364 370 ...
 $ rating        : num [1:9000055] 5 5 5 5 5 5 5 5 5 5 ...
 $ title         : chr [1:9000055] "Boomerang " "Net, The " "Outbreak " "Stargate " ...
 $ genres        : chr [1:9000055] "Comedy|Romance" "Action|Crime|Thriller" "Action|Drama|Sci-Fi|Thriller" "Action|Adventure|Sci-Fi" ...
 $ rating_date   : POSIXct[1:9000055], format: "1996-08-02 11:24:06" "1996-08-02 10:58:45" ...
 $ movie_dt      : num [1:9000055] 1992 1995 1995 1994 1994 ...
 $ date          : POSIXct[1:9000055], format: "1996-08-04" "1996-08-04" ...
 $ avg_rating    : num [1:9000055] 3.54 3.54 3.54 3.54 3.54 ...
 $ avg_rating_rel: num [1:9000055] 2.86 3.13 3.42 3.35 3.34 ...
 $ n             : int [1:9000055] 2178 13469 14447 17030 14550 4831 31079 3612 18921 7331 ...
 $ years         : num [1:9000055] 26 23 23 24 24 24 24 24 24 24 ...
 $ n_year        : num [1:9000055] 84 586 628 710 606 ...
```


\newpage
**_Modify validation data for Release Date Effect_**
We need to do the above **_avg_rating_rel_effect_** update for the validation data as well.
Let's update the **_validation_** table to get Table \ref{tbl:movielens_validation_avg_release_date_effect} below:

**_All Years_**
\begin{table}[H]

\caption{\label{tab:md_7}Movielens validation data with average rating due to release date effect\label{tbl:movielens_validation_avg_release_date_effect}}
\centering
\fontsize{4}{6}\selectfont
\begin{tabular}[t]{rrrlllrlrrrrr}
\toprule
userId & movieId & rating & title & genres & rating\_date & movie\_dt & date & avg\_rating & avg\_rating\_rel & n & years & n\_year\\
\midrule
1 & 231 & 5 & Dumb \& Dumber & Comedy & 1996-08-02 10:56:32 & 1994 & 1996-08-04 & 3.555820 & 2.953281 & 1798 & 24 & 75\\
1 & 480 & 5 & Jurassic Park & Action|Adventure|Sci-Fi|Thriller & 1996-08-02 11:00:53 & 1993 & 1996-08-04 & 3.555820 & 3.643993 & 3271 & 25 & 131\\
1 & 586 & 5 & Home Alone & Children|Comedy & 1996-08-02 11:07:48 & 1990 & 1996-08-04 & 3.555820 & 3.074550 & 1556 & 28 & 56\\
2 & 151 & 3 & Rob Roy & Action|Drama|Romance|War & 1997-07-07 03:34:10 & 1995 & 1997-07-06 & 3.606571 & 3.571984 & 771 & 23 & 34\\
2 & 858 & 2 & Godfather, The & Crime|Drama & 1997-07-07 03:20:45 & 1972 & 1997-07-06 & 3.606571 & 4.412675 & 2067 & 46 & 45\\
\bottomrule
\end{tabular}
\end{table}

```
tibble [999,999 x 13] (S3: tbl_df/tbl/data.frame)
 $ userId        : int [1:999999] 1 1 1 2 2 2 3 3 4 4 ...
 $ movieId       : num [1:999999] 231 480 586 151 858 ...
 $ rating        : num [1:999999] 5 5 5 3 2 3 3.5 4.5 5 3 ...
 $ title         : chr [1:999999] "Dumb & Dumber " "Jurassic Park " "Home Alone " "Rob Roy " ...
 $ genres        : chr [1:999999] "Comedy" "Action|Adventure|Sci-Fi|Thriller" "Children|Comedy" "Action|Drama|Romance|War" ...
 $ rating_date   : POSIXct[1:999999], format: "1996-08-02 10:56:32" "1996-08-02 11:00:53" ...
 $ movie_dt      : num [1:999999] 1994 1993 1990 1995 1972 ...
 $ date          : POSIXct[1:999999], format: "1996-08-04" "1996-08-04" ...
 $ avg_rating    : num [1:999999] 3.56 3.56 3.56 3.61 3.61 ...
 $ avg_rating_rel: num [1:999999] 2.95 3.64 3.07 3.57 4.41 ...
 $ n             : int [1:999999] 1798 3271 1556 771 2067 862 2545 947 1869 776 ...
 $ years         : num [1:999999] 24 25 28 23 46 21 28 17 23 24 ...
 $ n_year        : num [1:999999] 75 131 56 34 45 41 91 56 81 32 ...
```


\newpage
# Analysis - Model Building and Evaluation  

## Split the edx data into separate training and test sets
We will develop our algorithm using the edx set only.   
We will split the edx data into separate training and test sets to design and test our algorithm, namely train_set and test_set.



### Loss function
For a final test of our algorithm, we predict movie ratings in the test set as if they were unknown. [RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation) (residual mean squared error/root mean square error), the typical error loss, will be used to evaluate how close our predictions are to the true values in the validation set.   
We define ${y_{u,i}}$ as the rating for movie *i* by user *u* and denote our prediction with ${\hat{y}_{u,i}}$.   

The RMSE is then defined as Equation \ref{eq:EqLossFunction}:   

\equations{Loss function RMSE Equation \ref{eq:EqLossFunction}}
\label{eq:EqLossFunction}
\begin{equation}
  RMSE=\sqrt{\frac{1}{N}\sum_{u,i}(\hat{y}_{u,i}-y_{u,i})^{2}}
\end{equation}

with N being the number of user/movie combinations and the sum occurring over all these combinations.

Remember that we can interpret the RMSE similarly to a standard deviation: it is the typical error we make when predicting a movie rating. If this number is larger than 1, it means our typical error is larger than one star, which is not good.

Let's write a function that computes the RMSE for vectors of ratings and their corresponding predictors:  


```r
RMSE <- function(true_ratings, predicted_ratings) {
    sqrt(mean((true_ratings - predicted_ratings)^2))
}
```

\newpage
## Model 1: A first naive "mean" model  

Let's start by building the simplest possible recommendation system: we predict the same rating for all movies regardless of user. A model that assumes the same rating for all movies and users with all the differences explained by random variation would look like Equation \ref{eq:EqModel1}: 

\equations{Model 1: A naive "mean" model Equation \ref{eq:EqModel1}}
\label{eq:EqModel1}
\begin{equation}
  Y_{i,i} = \mu + \epsilon_{u,i}
\end{equation}

with $\epsilon_{u,i}$ independent errors sampled from the same distribution centered at 0 and $\mu$ the "true" rating for all movies. We know that the estimate that minimizes the RMSE is the least squares estimate of $\mu$ and, in this case, is the average of all ratings:


```r
(mu_hat <- mean(train_set$rating))
[1] 3.512482
```

If we predict all unknown ratings with $\hat\mu$ we obtain the following RMSE:


```r
(model_1_rmse <- RMSE(test_set$rating, mu_hat))
[1] 1.059904
```

Keep in mind that if we plug in any other number, we get a higher RMSE. For example:


```r
predictions <- rep(2.5, nrow(test_set))
RMSE(test_set$rating, predictions)
[1] 1.465736

predictions <- rep(3, nrow(test_set))
RMSE(test_set$rating, predictions)
[1] 1.177271

predictions <- rep(4, nrow(test_set))
RMSE(test_set$rating, predictions)
[1] 1.166678
```

From looking at the distribution of ratings, we can visualize that this is the standard deviation of that distribution. We get a RMSE of about 1. Our target is RMSE < 0.86490. So we can definitely do better!

\newpage
### Results Model 1

As we go along, we will be comparing different approaches. Let's start by creating a results table with this naive approach to get Table \ref{tbl:rmse_results_model_1}:

\begin{table}[H]

\caption{\label{tab:m_1_4}RMSE Results Model 1\label{tbl:rmse_results_model_1}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.059904\\
\bottomrule
\end{tabular}
\end{table}

\newpage
## Model 2: Movie effects
We know from experience that some movies are just generally rated higher than others. This intuition, that different movies are rated differently, is confirmed by data. We can augment our previous model by adding the term $b_{i}$ to represent average ranking for movie *i* and would look like Equation \ref{eq:EqModel2-1}: 

\equations{Model 2: Movie effects linear model Equation \ref{eq:EqModel2-1}}
\label{eq:EqModel2-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + \epsilon_{u,i}
\end{equation}

Statistics textbooks refer to the *b*s as effects or *"bias"*.

We can again use least squares to estimate the $b_{i}$ in the following way to get Equation \ref{eq:EqModel2-2}: 

\equations{LSE linear function to fit Movie effects linear model Equation \ref{eq:EqModel2-2}}
\label{eq:EqModel2-2}
\begin{equation}
  fit \leftarrow lm(rating \; \sim \; as.factor(userId), \: data = train\_{}set)
\end{equation}

Because there are thousands of $b_{i}$ as each movie gets one, the lm() function 
will be very slow here. We therefore will not run the code above.

But in this particular situation, we know that the least squares estimate 
$\hat{b_{i}}$ is just the average of $Y_{u,i}-\hat{\mu}$ for each movie *i*. **_So we can compute them this way (we will drop the hat notation in the code to represent estimates going forward)_**: 

\equations{Movie specific effects Equation \ref{eq:EqModel2-3}}
\label{eq:EqModel2-3}
\begin{equation}
  \hat{b_{i}} = \overline{y_{u,i} - \hat{\mu}}
\end{equation}


```r
mu <- mean(train_set$rating)

```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_2}

![Movie effect or bias distribution\label{fig:model_2}](figures/me_2-1.pdf) 

Remember $\hat{\mu}$=3.5 so a $b_{i}$=1.5 implies a perfect five star rating.
Let's see how much our prediction improves once we use $\hat{y_{u,i}}=\hat{\mu}+\hat{b_{i}}$:


```r
predicted_ratings_model_2 <- mu + test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  .$b_i

[1] 0.9437429
```

\newpage
### Results Model 1-2

Let's add the movie effects model to our results table to get Table \ref{tbl:rmse_results_model_1-2}

\begin{table}[H]

\caption{\label{tab:me_4}RMSE Results Models 1-2\label{tbl:rmse_results_model_1-2}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
\bottomrule
\end{tabular}
\end{table}

\newpage
## Model 3: User effects
Let's compute *b_u* the average rating for user *u* for those that have rated over 100 movies, see Figure \ref{fig:average_ratings_for_users_who_have_rated_over_100_movies}


```r
train_set %>% 
  group_by(userId) %>% 
  summarize(b_u = mean(rating)) %>% 
  filter(n()>=100) %>%
  ggplot(aes(b_u)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for users who have rated over 100 movies")
```

![Average rating for users who have rated over 100 movies\label{fig:average_ratings_for_users_who_have_rated_over_100_movies}](figures/ue_1-1.pdf) 

\newpage
Let's compute *b_u* the average rating for user *u* for those that have rated any movies, see Figure \ref{fig:average_ratings_for_users_who_have_rated_any_movies}


```r
train_set %>% 
  group_by(userId) %>% 
  summarize(b_u = mean(rating)) %>% 
  ggplot(aes(b_u)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for users who have rated any movies")
```

![Average rating for users who have rated any movies\label{fig:average_ratings_for_users_who_have_rated_any_movies}](figures/ue_2-1.pdf) 

Notice that there is substantial variability across users as well: some users are very cranky and others love every movie. This implies that a further improvement to our model may be as shown in Equation \ref{eq:EqModel3-1}:

\equations{Model 3: Movie + User effects linear model Equation \ref{eq:EqModel3-1}}
\label{eq:EqModel3-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \epsilon_{u,i}
\end{equation}

where $b_{u}$ is a user-specific effect. Now if a cranky user (negative $b_{u}$) rates a great movie (positive $b_{i}$), the effects counter each other and we may be able to correctly predict that this user gave this great movie a 3 rather than a 5.

To fit this model, we could again use lm() as shown in Equation \ref{eq:EqModel3-2}:

\equations{LSE linear function to fit Movie + User effects linear model Equation \ref{eq:EqModel3-2}}
\label{eq:EqModel3-2}
\begin{equation}
  fit \leftarrow lm(rating \; \sim \; as.factor(movieId) + as.factor(userId), \: data = train\_{}set)
\end{equation}

but, for the reasons described earlier, we won't. Instead, we will compute an approximation by computing $\hat{\mu}$ and $\hat{b_{i}}$ and estimating $\hat{b_{u}}$ as the average of $y_{u,i}-\hat{\mu}-\hat{b_{i}}$:

\equations{User specific effects Equation \ref{eq:EqModel3-3}}
\label{eq:EqModel3-3}
\begin{equation}
  \hat{b_{u}} = \overline{y_{u,i} - \hat{\mu} - \hat{b_{i}}}
\end{equation}

\newpage

```r
user_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  group_by(userId) %>%
  summarize(b_u = mean(rating - mu - b_i))
```

We can see that these estimates vary substantially, see Figure \ref{fig:model_3}

![User effect or bias distribution\label{fig:model_3}](figures/ue_4-1.pdf) 

We can now construct predictors and see how much the RMSE improves:


```r
predicted_ratings_model_3 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>%
  mutate(pred = mu + b_i + b_u) %>%
  .$pred

[1] 0.865932
```

\newpage
### Results Table Model 1-3
Let's add the user effects model to our results table to get Table \ref{tbl:rmse_results_model_1-3}

\begin{table}[H]

\caption{\label{tab:ue_6}RMSE Results Models 1-3\label{tbl:rmse_results_model_1-3}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
3 & Movie + User Effects Model & 0.8659320\\
\bottomrule
\end{tabular}
\end{table}

\newpage
## Model 4: Genre effects
The movielens data also has a genres column. This column includes every genre that applies to the movie. Some movies fall under several genres. Define a category of genres as whatever combination of genres appears in this column. We will refer to this category as simply "genres".

There is strong evidence of a genre effect as we have shown earlier in Figure \ref{fig:genres_error_bar_plots}, and in this section below in Figure \ref{fig:model_4}. If we define $g_{u,i}$ as the genre for *u* user's rating of movie *i*, then the following model as shown in Equation \ref{eq:EqModel4-1} is most appropriate:   

\equations{Model 4: Movie + User + Genre effects linear model Equation \ref{eq:EqModel4-1}}
\label{eq:EqModel4-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + \epsilon_{u,i}
\end{equation}

\begin{center}
with $x_{u,i}^k=1$ if $g_{u,i}$ is genre *k*
\end{center}


```r
train_set %>% 
  group_by(genres) %>% 
  summarize(mu_g = mean(rating)) %>% 
  ggplot(aes(mu_g)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for movies of category genres")
```

![Average rating for movies of category genres\label{fig:average_ratings_for_movies_of_category_genres}](figures/ge_1-1.pdf) 



To fit this model, we could again use the lm() function as shown in Equation \ref{eq:EqModel4-2}:

\equations{LSE linear function to fit Movie + User + Genres effects linear model Equation \ref{eq:EqModel4-2}}
\label{eq:EqModel4-2}
\begin{equation}
\begin{split}
  fit \leftarrow lm(rating \; \sim \; & as.factor(movieId) + as.factor(userId) + \\ 
  & as.factor(genres), \: data = train\_{}set)
\end{split}
\end{equation}

but, for the reasons described earlier, we won't. Instead, we will compute an approximation by computing $\hat{\mu}$, $\hat{b_{i}}$, $\hat{b_{u}}$ and estimating $\hat{b_{g}}$ as the average of $y_{u,i}-\hat{\mu}-\hat{b_{i}}-\hat{b_{u}}$ : 
<!-- $$b_{g}=\sum_{k=1}^Kx_{u,i}\beta_k$$ -->

\equations{Genres specific effects Equation \ref{eq:EqModel4-3}}
\label{eq:EqModel4-3}
\begin{equation}
  \hat{b_{g}} = \overline{y_{u,i} - \hat{\mu} - \hat{b_{i}} - \hat{b_{u}}}
\end{equation}

where:  

\equations{Genres specific effects Equation \ref{eq:EqModel4-4}}
\label{eq:EqModel4-4}
\begin{equation}
  \hat{b_{g}} = \sum_{k=1}^Kx_{u,i}\beta_k
\end{equation}

\begin{center}
with $x_{u,i}^k=1$ if $g_{u,i}$ is genre *k*
\end{center}

where $\hat{b_{g}}$ is genre specific effect.


```r
genres_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>%
  group_by(genres) %>%
  summarize(b_g = mean(rating - mu - b_i - b_u))
```

<!-- \newpage -->
We can see that these estimates vary substantially, see Figure \ref{fig:model_4}

![Genres effect or bias distribution\label{fig:model_4}](figures/ge_4-1.pdf) 

We can now construct predictors and see how much the RMSE improves:


```r
predicted_ratings_model_4 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  mutate(pred = mu + b_i + b_u + b_g) %>% 
  .$pred

[1] 0.8655941
```

\newpage
### Results Table Model 1-4

Let's add the genres effects model to our results table to get Table \ref{tbl:rmse_results_model_1-4}

\begin{table}[H]

\caption{\label{tab:ge_6}RMSE Results Models 1-4\label{tbl:rmse_results_model_1-4}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
\bottomrule
\end{tabular}
\end{table}

\newpage
## Model 5: Rating Time effect
The movielens dataset also includes a time stamp. This variable represents the time and date in which the rating was provided. Earlier in the EDA/Data wrangling section we created a new column date with the time stamp.   

We computed the average rating for each week and plotted this average against day.   

The plot shows some evidence of a time effect. If we define $d_{u,i}$ as the day for user's *u* rating of movie *i*, then the following updated model as shown in Equation \ref{eq:EqModel5-1} is most appropriate:   

\equations{Model 5: Movie + User + Genre + Rating time effects linear model Equation \ref{eq:EqModel5-1}}
\label{eq:EqModel5-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + f(d_{u,i}) + \epsilon_{u,i}
\end{equation}

\begin{center}
with f a smooth function of $d_{u,i}$
\end{center}

To fit this model, we could again use lm() function as shown in Equation \ref{eq:EqModel5-2}:

\equations{LSE linear function to fit Movie + User + Genres + Rating time effects linear model Equation \ref{eq:EqModel5-2}}
\label{eq:EqModel5-2}
\begin{equation}
\begin{split}
  fit \leftarrow lm(rating \; \sim \; & as.factor(movieId) + as.factor(userId) + \\ 
  & as.factor(genres) + as.factor(date), \: data = train\_{}set)
\end{split}
\end{equation}

but, for the reasons described earlier, we won't. Instead, we will compute an approximation by computing $\hat{\mu}$, $\hat{b_{i}}$, $\hat{b_{u}}$, $\hat{b_{g}}$  and estimating  $\hat{b_{d}}$ as the average of $y_{u,i}-\hat{\mu}-\hat{b_{i}}-\hat{b_{u}}-\hat{b_{g}}$ :   
<!-- $$\hat{b_{d}}=f(d_{u,i})$$ -->

\equations{Rating time specific effects Equation \ref{eq:EqModel5-3}}
\label{eq:EqModel5-3}
\begin{equation}
  \hat{b_{d}} = \overline{y_{u,i} - \hat{\mu} - \hat{b_{i}} - \hat{b_{u}}  - \hat{b_{u}}}
\end{equation}

where:

\equations{Rating time specific effects in function form Equation \ref{eq:EqModel5-4}}
\label{eq:EqModel5-4}
\begin{equation}
  \hat{b_{d}} = f(d_{u,i})
\end{equation}

where $\hat{b_{d}}$ is rating time specific effect.


```r
time_effect_avgs <- train_set %>%
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
  group_by(date) %>% 
  summarize(b_d = mean(avg_rating - mu - b_i - b_u - b_g))
```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_4}

![Rating time effect or bias distribution\label{fig:model_5}](figures/rte_2-1.pdf) 

We can now construct predictors and see how much the RMSE improves


```r
predicted_ratings_model_5 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  left_join(time_effect_avgs, by = "date") %>%
  mutate(pred = mu + b_i + b_u + b_g + b_d) %>%
  .$pred

[1] 0.8654205
```

\newpage
### Results Table Model 1-5

Let's add the Rating Time effects model to our results table to get Table \ref{tbl:rmse_results_model_1-5}

\begin{table}[H]

\caption{\label{tab:rte_4}RMSE Results Models 1-5\label{tbl:rmse_results_model_1-5}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
5 & Movie + User + Genres + Rating Time Effects Model & 0.8654205\\
\bottomrule
\end{tabular}
\end{table}


\newpage
## Model 6: Release Date Effect
The plots in Figures \ref{fig:ratings_movie_release_date_all_dates},  \ref{fig:25_movies_avg_and_most_ratings_per_year_post_1993}, \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993}, \ref{fig:movies_average_ratings_versus_ratings_per_year_pre_1993}, \ref{fig:movies_average_ratings_versus_ratings_per_year_for_all_years_for_edx} above shows some evidence of a Release Date effect based on the when the movie was released and it's popularity given by the mean rating. If we define $arr_{r,i,y}$ as the average rating *r=mean(rating)* since release date *y=n_year* for movie *i* (in the formula for plots above), then the following updated model is most appropriate:   

\equations{Model 6: Movie + User + Genre + Rating time + Release date effects linear model Equation \ref{eq:EqModel6-1}}
\label{eq:EqModel6-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + f(d_{u,i}) + f(arr_{r,i,y}) + \epsilon_{u,i}
\end{equation}

\begin{center}
with f a smooth function of $arr_{r,i,y}$
\end{center}

To fit this model, we could again use lm() function as shown in Equation \ref{eq:EqModel6-2}:

\equations{LSE linear function to fit Movie + User + Genres + Rating time + Release date effects linear model Equation \ref{eq:EqModel6-2}}
\label{eq:EqModel6-2}
\begin{equation}
\begin{split}
  fit \leftarrow lm(rating \; \sim \; & as.factor(movieId) + as.factor(userId) + \\ 
  & as.factor(genres) + as.factor(date) + \\ 
  & as.factor(movie_dt), \: data = train\_{}set)
\end{split}
\end{equation}

but, for the reasons described earlier, we won't. Instead, we will compute an approximation by computing $\hat{\mu}$, $\hat{b_{i}}$, $\hat{b_{u}}$, $\hat{b_{g}}$, $\hat{b_{d}}$ and estimating  $\hat{b_{r}}$ as the average of $y_{u,i}-\hat{\mu}-\hat{b_{i}}-\hat{b_{u}}-\hat{b_{g}}-\hat{b_{d}}$ where:   
<!-- $$\hat{b_{r}}=f(arr_{r,i,y})$$ -->

\equations{Release date specific effects Equation \ref{eq:EqModel6-3}}
\label{eq:EqModel6-3}
\begin{equation}
  \hat{b_{r}} = \overline{y_{u,i} - \hat{\mu} - \hat{b_{i}} - \hat{b_{u}}  - \hat{b_{g}} - \hat{b_{d}}}
\end{equation}

where:

\equations{Release date specific effects in function form Equation \ref{eq:EqModel6-4}}
\label{eq:EqModel6-4}
\begin{equation}
  \hat{b_{r}} = f(arr_{r,i,y})
\end{equation}

where $\hat{b_{r}}$ is Release date specific effect.


```r
rel_effect_avgs <- train_set %>%
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
  left_join(time_effect_avgs, by = "date") %>% 
  group_by(movieId) %>% 
  summarize(b_r = mean(avg_rating_rel - mu - b_i - b_u - b_g - b_d))
```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_6}

![Release Date effect or bias distribution\label{fig:model_6}](figures/rde_2-1.pdf) 

We can now construct predictors and see how much the RMSE improves


```r
predicted_ratings_model_6 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  left_join(time_effect_avgs, by = "date") %>% 
  left_join(rel_effect_avgs, by='movieId') %>%
  mutate(pred = mu + b_i + b_u + b_g + b_d + b_r) %>%
  .$pred

(model_6_rmse <- RMSE(predicted_ratings_model_6, test_set$rating))
```

```
[1] 0.863333
```

\newpage
### Results Table Model 1-6

Let's add the Release Date effects model to our results table to get Table \ref{tbl:rmse_results_model_1-6}

\begin{table}[H]

\caption{\label{tab:rde_4}RMSE Results Models 1-6\label{tbl:rmse_results_model_1-6}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
5 & Movie + User + Genres + Rating Time Effects Model & 0.8654205\\
6 & Movie + User + Genres + Rating Time + Release date Effects Model & 0.8633330\\
\bottomrule
\end{tabular}
\end{table}


\newpage
## Regularization
### Motivation 
Despite the large movie to movie variation, our improvement in RMSE is either relatively negligible or the results from the recommendations are strange. Let's explore where we made mistakes in our second model, using only movie effects $b_{i}$.   

Here are the **10 largest mistakes**

```r
test_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  mutate(residual = rating - (mu + b_i)) %>%
  arrange(desc(abs(residual))) %>%  
  slice(1:10) %>% 
  pull(title) %>% 
    kable("latex", escape=FALSE, booktabs=TRUE, linesep="", 
          caption="Without Regularization 10 largest mistakes\\label{tbl:without_regularization_10_largest_mistakes}") %>% 
      kable_styling(latex_options=c("HOLD_position"))
```

\begin{table}[H]

\caption{\label{tab:reg_1}Without Regularization 10 largest mistakes\label{tbl:without_regularization_10_largest_mistakes}}
\centering
\begin{tabular}[t]{l}
\toprule
x\\
\midrule
From Justin to Kelly\\
Time Changer\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Shawshank Redemption, The\\
Children Underground\\
\bottomrule
\end{tabular}
\end{table}


These all seem like obscure movies, or in this case a repetition. Many of them have large predictions.   
Let's look at the **top 10 worst and best movies** based on $\hat{b_{i}}$. First, let's create a database that connects 'movieId' to movie title


```r
movie_titles <- edx %>% 
  select(movieId, title) %>%
  distinct()
```

Here are the **10 best movies** according to our estimate


```r
movie_avgs %>% left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  slice(1:10)  %>% 
  pull(title)%>% 
  kable("latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Without Regularization 10 Best movies\\label{tbl:without_regularization_10_best_movies}") %>%
    kable_styling(latex_options=c("HOLD_position"))
```

\begin{table}[H]

\caption{\label{tab:reg_3}Without Regularization 10 Best movies\label{tbl:without_regularization_10_best_movies}}
\centering
\begin{tabular}[t]{l}
\toprule
x\\
\midrule
Hellhounds on My Trail (1999)\\
Who's Singin' Over There? (a.k.a. Who Sings Over There) (Ko to tamo peva) (1980)\\
Satan's Tango (SÃ¡tÃ¡ntangÃ³) (1994)\\
Shadows of Forgotten Ancestors (1964)\\
Money (Argent, L') (1983)\\
Fighting Elegy (Kenka erejii) (1966)\\
Sun Alley (Sonnenallee) (1999)\\
Aerial, The (La Antena) (2007)\\
Blue Light, The (Das Blaue Licht) (1932)\\
More (1998)\\
\bottomrule
\end{tabular}
\end{table}

<!-- \newpage -->
And here are the **10 worst movies**


```r
movie_avgs %>% left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  slice(1:10)  %>% 
  pull(title)%>% 
  kable("latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Without Regularization 10 Worst movies\\label{tbl:without_regularization_10_worst_movies}") %>%
    kable_styling(latex_options=c("HOLD_position"))
```

\begin{table}[H]

\caption{\label{tab:reg_4}Without Regularization 10 Worst movies\label{tbl:without_regularization_10_worst_movies}}
\centering
\begin{tabular}[t]{l}
\toprule
x\\
\midrule
Besotted (2001)\\
Confessions of a Superhero (2007)\\
War of the Worlds 2: The Next Wave (2008)\\
SuperBabies: Baby Geniuses 2 (2004)\\
From Justin to Kelly (2003)\\
Legion of the Dead (2000)\\
Disaster Movie (2008)\\
Hip Hop Witch, Da (2000)\\
Criminals (1996)\\
Mountain Eagle, The (1926)\\
\bottomrule
\end{tabular}
\end{table}

They all seem to be quite obscure. Let's look at how often they are rated.   

**10 best movies**


```r
train_set %>% count(movieId) %>% 
  left_join(movie_avgs, by="movieId") %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  slice(1:10) %>% 
  pull(n)
 [1] 1 3 2 1 1 1 1 1 1 6
```

**10 worst movies**


```r
train_set %>% count(movieId) %>% 
  left_join(movie_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  slice(1:10) %>% 
  pull(n)
 [1]   1   1   2  40 168   4  28  11   1   1
```

The **_supposed "best" and "worst" movies were rated by very few users_**, in most cases just 1. These movies were mostly obscure ones. This is because with just a few users, we have more uncertainty. Therefore, larger estimates of $b_{i}$, negative or positive, are more likely.

These are noisy estimates that we should not trust, especially when it comes to prediction. Large errors can increase our RMSE, so we would rather be conservative when unsure.

**_Regularization permits us to penalize large estimates that are formed using small sample sizes_**.

\newpage
### Penalized least squares
**_The general idea behind regularization is to constrain the total variability of the effect sizes_**. Why does this help? Consider a case in which we have movie $i=1$ with 100 user ratings and 4 movies $i=2,3,4,5$ with just one user rating. 
We intend to fit the model   
$$Y_{u,i}=\mu+b_{i}+\epsilon_{u,i}$$
Suppose we know the average rating is, say, $\mu=3$. If we use least squares, the estimate for the first movie effect $b_{1}$ is the average of the 100 user ratings, $\frac{1}{100}\sum_{i=1}^{100}(Y_{i,1}-\mu)$, which we expect to be a quite precise. However, the estimate for movies 2, 3, 4, and 5 will simply be the observed deviation from the average rating $\hat{b_{i}}=Y_{u,i}-\hat{\mu}$ which is an estimate based on just one number so it won't be precise at all. Note these estimates make the error $Y_{u,i}-\mu+\hat{b_{i}}$ equal to 0 for $i=2,3,4,5$, but this is a case of over-training. In fact, ignoring the one user and guessing that movies 2,3,4, and 5 are just average movies $(b_{i}=0)$ might provide a better prediction. The general idea of penalized regression is to control the total variability of the movie effects: $\sum_{1=1}^{5}=b_{i}^2$. Specifically, instead of minimizing the least squares equation, we minimize an equation that adds a penalty:   
$$\frac{1}{N}\sum_{u,i}(y_{u,i}-\mu-b_{i})^2+\lambda\sum_{i}b_{i}^2$$
The first term is just least squares and the second is a penalty that gets larger when many $b_{i}$ are large. Using calculus we can actually show that the values of $b_{i}$ that minimize this equation are:  
$$\hat{b_{i}}(\lambda)=\frac{1}{\lambda+n_{i}}\sum_{u=1}^{n_{i}}(Y_{u,i}-\hat{\mu})$$
where $n_{i}$ is the number of ratings made for movie $i$. This approach will have our desired effect: when our sample size $n_{i}$ is very large, a case which will give us a stable estimate, then the penalty $\lambda$ is effectively ignored since $n_{i}+\lambda\approx{n_{i}}$. However, when the $n_{i}$ is small, then the estimate $\hat{b_{i}}(\lambda)$ is shrunken towards 0. The larger $\lambda$, the more we shrink.


\newpage
## Model 7 : Model 2 + Regularization: Choosing the penalty terms
Note that $\lambda$ is a tuning parameter. We will use cross-validation to choose it.   

**_Here we use only 1 fold_**


```r
lambdas <- seq(0, 10, 0.25)
mu <- mean(train_set$rating)
just_the_sum <- train_set %>% 
  group_by(movieId) %>% 
  summarize(s = sum(rating - mu), n_i = n())
rmses <- sapply(lambdas, function(l){
  predicted_ratings <- test_set %>% 
    left_join(just_the_sum, by='movieId') %>% 
    mutate(b_i = s/(n_i+l)) %>%
    mutate(pred = mu + b_i) %>%
    .$pred
  return(RMSE(predicted_ratings, test_set$rating))
})

(lambda <- lambdas[which.min(rmses)])
```

```
[1] 2.5
```

```r
qplot(lambdas, rmses)  
```

![](figures/m7_1-1.pdf)<!-- --> 

```r
mu <- mean(train_set$rating)
movie_reg_avgs <- train_set %>% 
  group_by(movieId) %>% 
  summarize(b_i = sum(rating - mu)/(n()+lambda), n_i = n()) 
```


To see how the estimates shrink, let's make a plot of the regularized estimates versus the least squares estimates.

![](figures/m7_3-1.pdf)<!-- --> 

\newpage
### Effect of use of penalized estimates
Now, let's look at the **top 10 best movies based on the penalized estimates** $\hat{b_{i}}(\lambda)$

**Top 10 best movies**


```r
train_set %>%
  dplyr::count(movieId) %>% 
  left_join(movie_reg_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  select(title, b_i, n) %>% 
  slice(1:10) %>% 
  kable("latex", escape=TRUE, booktabs=TRUE, linesep="", caption="With Regularization top 10 best movies based on the penalized estimates\\label{tbl:with_regularization_top_10_best_movies_based_on_the_penalized_estimates}") %>% kable_styling(latex_options=c("HOLD_position"))
Joining, by = "movieId"
```

\begin{table}[H]

\caption{\label{tab:m7_4}With Regularization top 10 best movies based on the penalized estimates\label{tbl:with_regularization_top_10_best_movies_based_on_the_penalized_estimates}}
\centering
\begin{tabular}[t]{lrr}
\toprule
title & b\_i & n\\
\midrule
More (1998) & 0.9911891 & 6\\
Shawshank Redemption, The (1994) & 0.9447301 & 22363\\
Godfather, The (1972) & 0.9048453 & 14107\\
Usual Suspects, The (1995) & 0.8568137 & 17315\\
Schindler's List (1993) & 0.8514631 & 18567\\
Who's Singin' Over There? (a.k.a. Who Sings Over There) (Ko to tamo peva) (1980) & 0.8113734 & 3\\
Rear Window (1954) & 0.8087002 & 6325\\
Casablanca (1942) & 0.8046208 & 9027\\
Double Indemnity (1944) & 0.7972822 & 1711\\
Third Man, The (1949) & 0.7961995 & 2420\\
\bottomrule
\end{tabular}
\end{table}

These make much more sense! These movies are watched more and have more ratings.   

\newpage
**Top 10 worst movies**


```r
train_set %>%
  dplyr::count(movieId) %>% 
  left_join(movie_reg_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  select(title, b_i, n) %>% 
  slice(1:10) %>% 
  kable("latex", escape=TRUE, booktabs=TRUE, linesep="", caption="With Regularization top 10 worst movies based on the penalized estimates\\label{tbl:with_regularization_top_10_worst_movies_based_on_the_penalized_estimates}") %>%
    kable_styling(latex_options=c("HOLD_position"))
Joining, by = "movieId"
```

\begin{table}[H]

\caption{\label{tab:m7_5}With Regularization top 10 worst movies based on the penalized estimates\label{tbl:with_regularization_top_10_worst_movies_based_on_the_penalized_estimates}}
\centering
\begin{tabular}[t]{lrr}
\toprule
title & b\_i & n\\
\midrule
From Justin to Kelly (2003) & -2.628135 & 168\\
SuperBabies: Baby Geniuses 2 (2004) & -2.588218 & 40\\
Disaster Movie (2008) & -2.421295 & 28\\
PokÃ©mon Heroes (2003) & -2.406821 & 109\\
Glitter (2001) & -2.300597 & 282\\
Barney's Great Adventure (1998) & -2.287959 & 168\\
Carnosaur 3: Primal Species (1996) & -2.282927 & 51\\
Gigli (2003) & -2.268819 & 249\\
Pokemon 4 Ever (a.k.a. PokÃ©mon 4: The Movie) (2002) & -2.249836 & 168\\
Son of the Mask (2005) & -2.226802 & 128\\
\bottomrule
\end{tabular}
\end{table}

**Improved our results**


```r
predicted_ratings_model_7 <- test_set %>% 
  left_join(movie_reg_avgs, by='movieId') %>%
  mutate(pred = mu + b_i) %>%
  .$pred

[1] 0.9436745
```

\newpage
### Results Table Model 1-7

Let's add Regularization to our Model 1-2 results table to get Table \ref{tbl:rmse_results_model_1-7}

\begin{table}[H]

\caption{\label{tab:m7_7}RMSE Results Models 1-7\label{tbl:rmse_results_model_1-7}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
5 & Movie + User + Genres + Rating Time Effects Model & 0.8654205\\
6 & Movie + User + Genres + Rating Time + Release date Effects Model & 0.8633330\\
7 & Regularized Movie Effect Model - 1 fold CV & 0.9436745\\
\bottomrule
\end{tabular}
\end{table}
The penalized estimates provide an improvement over the least squares estimates.


\newpage
## Model 8 : Model 4 + Regularization: Choosing the penalty terms
*We will be using multi-fold full cross-validation on the entire edx data set shortly*. 

But first...we use regularization for the estimate *user effects as well as the genres effects*. 
We are minimizing:   
$$\frac{1}{N}\sum_{u,i}(y_{u,i}-\mu-b_{i}-b_{u}-b_{g})^2+\lambda(\sum_{i}{b_{i}^2}+\sum_{u}{b_{u}^2}+\sum_{g_{u,i}}{b_{g}^2})$$
The estimates that minimize this can be found similarly to what we did above.   

Here **_we use 1-fold cross-validation_** to pick the optimal $\lambda$.   


```r
lambdas <- seq(0, 10, 0.25)
rmses <- sapply(lambdas, function(l){
  mu <- mean(train_set$rating)
  b_i <- train_set %>%
    group_by(movieId) %>%
    summarize(b_i = sum(rating - mu)/(n()+l))
  b_u <- train_set %>% 
    left_join(b_i, by="movieId") %>%
    group_by(userId) %>%
    summarize(b_u = sum(rating - b_i - mu)/(n()+l))
  b_g <- train_set %>% 
    left_join(b_i, by='movieId') %>% 
    left_join(b_u, by='userId') %>%
    group_by(genres) %>% 
    summarize(b_g = sum(rating - mu - b_i - b_u)/(n()+l))
  predicted_ratings <- test_set %>% 
    left_join(b_i, by = "movieId") %>%
    left_join(b_u, by = "userId") %>% 
    left_join(b_g, by = "genres") %>% 
    mutate(pred = mu + b_i + b_u + b_g) %>%
    .$pred
  return(RMSE(predicted_ratings, test_set$rating))
})

qplot(lambdas, rmses)
```

![](figures/m8_1-1.pdf)<!-- --> 

For the Movie + User + Genres model so far, the optimal $\lambda$ is


```r
(lambda <- lambdas[which.min(rmses)])
```

```
## [1] 4.75
```


\newpage
### Results Table Model 1-8

Let's add Regularization to our Model 1-4 results table to get Table \ref{tbl:rmse_results_model_1-8} and arrange in descending order of RMSE's

\begin{table}[H]

\caption{\label{tab:m8_4}RMSE Results Models 1-8\label{tbl:rmse_results_model_1-8}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
7 & Regularized Movie Effect Model - 1 fold CV & 0.9436745\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
5 & Movie + User + Genres + Rating Time Effects Model & 0.8654205\\
8 & Regularized Movie + User + Genre Effects Model - 1 fold CV & 0.8649406\\
6 & Movie + User + Genres + Rating Time + Release date Effects Model & 0.8633330\\
\bottomrule
\end{tabular}
\end{table}


\newpage
## Model 9 : Model 6 + Regularization: Choosing the penalty terms
Let's add the penalty term to the model 6 **_using 1-fold CV_**


```r
lambdas <- seq(0, 10, 0.25)
rmses <- sapply(lambdas, function(l){
  mu <- mean(train_set$rating)
  b_i <- train_set %>%
    group_by(movieId) %>%
    summarize(b_i = sum(rating - mu)/(n()+l), n_i = n())
  b_u <- train_set %>% 
    left_join(b_i, by="movieId") %>%
    group_by(userId) %>%
    summarize(b_u = sum(rating - b_i - mu)/(n()+l), n_i = n())
  b_g <- train_set %>% 
    left_join(b_i, by='movieId') %>% 
    left_join(b_u, by='userId') %>%
    group_by(genres) %>% 
    summarize(b_g = sum(rating - mu - b_i - b_u)/(n()+l), n_i = n())
  b_d <- train_set %>%
    left_join(b_i, by='movieId') %>%
    left_join(b_u, by='userId') %>% 
    left_join(b_g, by = "genres") %>% 
    group_by(date) %>% 
    summarize(b_d = sum(avg_rating - mu - b_i - b_u - b_g)/(n()+l), n_i = n())
  b_r <- train_set %>%
    left_join(b_i, by='movieId') %>%
    left_join(b_u, by='userId') %>% 
    left_join(b_g, by = "genres") %>% 
    left_join(b_d, by = "date") %>% 
    group_by(movieId) %>% 
    summarize(b_r = sum(avg_rating_rel - mu - b_i - b_u - b_g - b_d)/(n()+l), n_i = n())

  predicted_ratings <- test_set %>%
    left_join(b_i, by = "movieId") %>%
    left_join(b_u, by = "userId") %>%
    left_join(b_g, by = "genres") %>% 
    left_join(b_d, by = "date") %>% 
    left_join(b_r, by='movieId') %>%
    mutate(pred = mu + b_i + b_u + b_g + b_d + b_r) %>% 
    .$pred
  
  return(RMSE(predicted_ratings, test_set$rating))
})

(lambda <- lambdas[which.min(rmses)])
```

```
[1] 4.5
```

```r
qplot(lambdas, rmses)  
```

![](figures/m9_1-1.pdf)<!-- --> 

\newpage
To see how the estimates shrink, let's make a plot of the regularized estimates versus the least squares estimates.
**_Here we use only 1 fold_**

![](figures/m9_2-1.pdf)<!-- --> 




\newpage
### Results Table  Model 1-9

Let's add Regularization to our Model 1-6 results table to get Table \ref{tbl:rmse_results_model_1-9} and arrange in descending order of RMSE's

\begin{table}[H]

\caption{\label{tab:m9_4}RMSE Results Models 1-9\label{tbl:rmse_results_model_1-9}}
\centering
\fontsize{7}{9}\selectfont
\begin{tabular}[t]{llr}
\toprule
Index & Method & RMSE\\
\midrule
1 & Just the average & 1.0599043\\
2 & Movie Effect Model & 0.9437429\\
7 & Regularized Movie Effect Model - 1 fold CV & 0.9436745\\
3 & Movie + User Effects Model & 0.8659320\\
4 & Movie + User + Genres Effects Model & 0.8655941\\
5 & Movie + User + Genres + Rating Time Effects Model & 0.8654205\\
8 & Regularized Movie + User + Genre Effects Model - 1 fold CV & 0.8649406\\
6 & Movie + User + Genres + Rating Time + Release date Effects Model & 0.8633330\\
9 & Regularized Movie + User + Genre + Rating Time + Release date Effect Model - 1 fold CV & 0.8629094\\
\bottomrule
\end{tabular}
\end{table}

\newpage
# Results

---  

\newpage
# Conclusion

---  

\newpage
# Appendix: All code for this report


```r
knitr::knit_hooks$set(time_it = local({
  now <- NULL
  function(before, options) {
    if (before) {
      # record the current time before each chunk
      now <<- Sys.time()
    } else {
      # calculate the time difference after a chunk
      res <- difftime(Sys.time(), now)
      # return a character string to show the time
      # paste("Time for this code chunk to run:", res)
      paste("Time for the chunk", options$label, "to run:", res)
    }
  }
}))

# knitr::opts_chunk$set(fig.pos = "!H", out.extra = "")
knitr::opts_chunk$set(echo = TRUE,
                      fig.path = "figures/")

# Beware, using the "time_it" hook messes up fig.cap, \label, \ref
# knitr::opts_chunk$set(time_it = TRUE)
#knitr::opts_chunk$set(eval = FALSE)
  library(ggplot2)
  library(kableExtra)

################################
# Create edx set, validation set
################################

# Note: this process could take a couple of minutes

if(!require(tidyverse)) install.packages("tidyverse", repos = "http://cran.us.r-project.org")
if(!require(caret)) install.packages("caret", repos = "http://cran.us.r-project.org")
if(!require(data.table)) install.packages("data.table", repos = "http://cran.us.r-project.org")
if(!require(lubridate)) install.packages("caret", repos = "http://cran.us.r-project.org")
if(!require(groupdata2)) install.packages("data.table", repos = "http://cran.us.r-project.org")
if(!require(gridExtra)) install.packages("gridExtra", repos = "http://cran.us.r-project.org")

# https://www.tidyverse.org/blog/2020/05/dplyr-1-0-0-last-minute-additions/
options(dplyr.summarise.inform = FALSE)

# MovieLens 10M dataset:
# https://grouplens.org/datasets/movielens/10m/
# http://files.grouplens.org/datasets/movielens/ml-10m-README.html
# http://files.grouplens.org/datasets/movielens/ml-10m.zip

dl <- tempfile()
download.file("http://files.grouplens.org/datasets/movielens/ml-10m.zip", dl)

ratings <- fread(text = gsub("::", "\t", readLines(unzip(dl, "ml-10M100K/ratings.dat"))),
                 col.names = c("userId", "movieId", "rating", "timestamp"))

movies <- str_split_fixed(readLines(unzip(dl, "ml-10M100K/movies.dat")), "\\::", 3)
colnames(movies) <- c("movieId", "title", "genres")
movies <- as.data.frame(movies) %>% mutate(movieId = as.numeric(movieId),
                                           title = as.character(title),
                                           genres = as.character(genres))

movielens <- left_join(ratings, movies, by = "movieId")

# Validation set will be 10% of MovieLens data
set.seed(1, sample.kind="Rounding")
# if using R 3.5 or earlier, use `set.seed(1)` instead
test_index <- createDataPartition(y = movielens$rating, times = 1, p = 0.1, list = FALSE)
edx <- movielens[-test_index,]
temp <- movielens[test_index,]

# Make sure userId and movieId in validation set are also in edx set
validation <- temp %>%
  semi_join(edx, by = "movieId") %>%
  semi_join(edx, by = "userId")

# Add rows removed from validation set back into edx set
removed <- anti_join(temp, validation)
edx <- rbind(edx, removed)

save(ratings, file = "rdas/ratings.rda")
save(movies, file = "rdas/movies.rda")
rm(dl, ratings, movies, test_index, temp, movielens, removed)

save(edx, file = "rdas/edx.rda")
save(validation, file = "rdas/validation.rda")
  kable(edx[1:10,], "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Movielens data\\label{tbl:movielens_data}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
unique_u_m_g <- edx %>% 
  summarize(unique_users = n_distinct(userId),
            unique_movies = n_distinct(movieId), 
            unique_genres = n_distinct(genres))

kable(unique_u_m_g, "latex", 
      booktabs=TRUE, linesep="",
      caption="Unique Users, Movies and Genres\\label{tbl:uniq_users_movies_genres}") %>%  kable_styling(latex_options=c("HOLD_position"))
keep <- edx %>%
  dplyr::count(movieId) %>%
  top_n(4) %>%
  pull(movieId)
tab <- edx %>%
  filter(userId %in% c(13:20)) %>% 
  filter(movieId %in% keep) %>% 
  select(userId, title, rating) %>% 
  spread(title, rating)
  
kable(tab, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Matrix of seven users and four movies\\label{tbl:matrix_seven_users_four_movies}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=8)
users <- sample(unique(edx$userId), 100)
rafalib::mypar()
edx %>% filter(userId %in% users) %>% 
  select(userId, movieId, rating) %>%
  mutate(rating = 1) %>%
  spread(movieId, rating) %>% select(sample(ncol(.), 100)) %>% 
  as.matrix() %>% t(.) %>%
  image(1:100, 1:100,. , xlab="Movies", ylab="Users")
abline(h=0:100+0.5, v=0:100+0.5, col = "grey")
edx %>% 
  dplyr::count(movieId) %>% 
  ggplot(aes(n)) + 
  geom_histogram(bins = 30, color = "black") + 
  scale_x_log10() +  # try with and without this line
  ggtitle("Movies")
edx %>%
  dplyr::count(userId) %>% 
  ggplot(aes(n)) + 
  geom_histogram(bins = 30, color = "black") + 
  scale_x_log10() + # try with and without this line
  ggtitle("Users")

rm(tab,keep,unique_u_m_g,users)
edx_dt <- edx %>% 
  mutate(rating_date = as_datetime(timestamp)) %>% 
  select(-timestamp)
rm(edx)

kable(edx_dt[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens edx data with rating date-time\\label{tbl:movielens_edx_data_rating_date}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=8)
td <- edx_dt$title
movie_dt <- str_extract(td, "\\([0-9]{4}\\)$") %>%
  str_extract("[0-9]{4}")
title_o <- str_replace(td, "\\([0-9]{4}\\)$", "")
edx_md <- edx_dt %>% mutate(title=title_o, movie_dt=as.numeric(movie_dt))
rm(td,movie_dt,title_o,edx_dt)
save(edx_md, file = "rdas/edx_md.rda")

kable(edx_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens edx data with movie release date\\label{tbl:movielens_edx_movie_release_date}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=8)
validation_dt <- validation %>% 
  mutate(rating_date = as_datetime(timestamp)) %>% 
  select(-timestamp)
rm(validation)

kable(validation_dt[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens validation data with rating date-time\\label{tbl:movielens_val_data_rating_date}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=8)
td <- validation_dt$title
movie_dt <- str_extract(td, "\\([0-9]{4}\\)$") %>% 
  str_extract("[0-9]{4}")
title_o <- str_replace(td, "\\([0-9]{4}\\)$", "")
validation_md <- validation_dt %>% mutate(title=title_o, movie_dt=as.numeric(movie_dt))
rm(td,movie_dt,title_o,validation_dt)
save(validation_md, file = "rdas/validation_md.rda")

kable(validation_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens validation data with movie release date\\label{tbl:movielens_val_movie_release_date}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=8)

# rm(validation_md)
edx_md %>% group_by(genres) %>%
  summarize(n = n(), avg = mean(rating), se = sd(rating)/sqrt(n())) %>%
  filter(n >= 1000) %>% 
  mutate(genres = reorder(genres, avg)) %>%
  ggplot(aes(x = genres, y = avg, ymin = avg - 2*se, ymax = avg + 2*se)) + 
  geom_point() +
  geom_errorbar() + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
edx_md %>% mutate(date = round_date(rating_date, unit = "week")) %>%
  group_by(date) %>%
  summarize(avg_rating = mean(rating)) %>%
  ggplot(aes(date, avg_rating)) +
  geom_point() +
  geom_smooth()
edx_md <- edx_md %>% mutate(date = round_date(rating_date, unit = "week")) %>%
  group_by(date) %>%
  mutate(avg_rating = mean(rating)) %>% ungroup()
save(edx_md, file = "rdas/edx_md.rda")

kable(edx_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens edx data with average rating due to rating time effect\\label{tbl:movielens_edx_avg_rating_time_effect}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=6)
validation_md <- validation_md %>% mutate(date = round_date(rating_date, unit = "week")) %>%
  group_by(date) %>%
  mutate(avg_rating = mean(rating)) %>% ungroup()
save(validation_md, file = "rdas/validation_md.rda")

kable(validation_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens validation data with average rating due to rating time effect\\label{tbl:movielens_validation_avg_rating_time_effect}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=6)

# rm(validation_md)
ratings_m_y <- edx_md %>%
  group_by(movieId) %>%
  summarize(n = n_distinct(userId), year = as.character(first(movie_dt))) %>% 
  mutate(sqrt_n=sqrt(n)) %>% 
  group_by(year) %>% 
  mutate(r_median=median(sqrt_n)) %>% select(year,r_median) %>% 
  unique() %>% arrange(desc(r_median))

# min(edx_md$movie_dt)
# # [1] 1915
# max(edx_md$movie_dt)
# # [1] 2008

# View(ratings_m_y)
ggplot(data=ratings_m_y, aes(x=year, y=r_median) ) + 
  geom_point( size=1, colour="#000080" ) + 
  theme_light(base_size=8) + 
  # labs(title="", x="\nyear", y="r_median\n") + 
  scale_x_discrete(breaks=seq(min(edx_md$movie_dt),max(edx_md$movie_dt),by=2)) + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

spline_int <- as.data.frame(spline(ratings_m_y$year, ratings_m_y$r_median))

ggplot(data=ratings_m_y, aes(x=year, y=r_median) ) +
  geom_point(data = spline_int, aes(x = x, y = y)) +
  geom_line(data = spline_int, aes(x = x, y = y)) +
  theme_light(base_size=8) +
  # scale_x_discrete(breaks=seq(min(edx_md$movie_dt),max(edx_md$movie_dt),by=2)) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

rm(ratings_m_y)

res2_1993_plus <- edx_md %>%
  filter(movie_dt >= 1993) %>%
  group_by(movieId) %>%
  # summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - min(movie_dt)) %>% 
  summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - first(movie_dt)) %>% 
  mutate(n_year = n / years) %>%
  top_n(25, n_year) %>%
  arrange(desc(n_year))
# qplot(res2_1993_plus$years,res2_1993_plus$n)
ggplot(res2_1993_plus,aes(years,n)) + 
  geom_line() + 
  geom_smooth() + 
  geom_vline(xintercept = 25) # 1993

rm(res2_1993_plus)
res3 <- edx_md %>%
  filter(movie_dt >= 1993) %>%
  group_by(movieId) %>%
  summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - min(movie_dt)) %>%
  mutate(n_year = n / years)
res3 %>% 
  group_by(round(n_year)) %>% 
  ggplot(aes(n_year,avg_rating)) + geom_point() + geom_smooth()

rm(res3)
res3_b <- edx_md %>%
  filter(movie_dt < 1993) %>%
  group_by(movieId) %>%
  summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - min(movie_dt)) %>%
  mutate(n_year = n / years)
res3_b %>% 
  group_by(round(n_year)) %>% 
  ggplot(aes(n_year,avg_rating)) + geom_point() + geom_smooth()

rm(res3_b)
res3_all_edx <- edx_md %>%
  group_by(movieId) %>%
  summarize(avg_rating_rel = mean(rating), n = n(), title=title[1], years=2018 - first(movie_dt)) %>%
  mutate(n_year = round(n / years))
res3_all_edx %>% 
  group_by(n_year) %>% 
  ggplot(aes(n_year,avg_rating_rel)) + geom_point() + geom_smooth()
save(res3_all_edx, file = "rdas/res3_all_edx.rda")
res3_all_edx_2 <- res3_all_edx %>% select(-title)
edx_md <- edx_md %>% left_join(res3_all_edx_2, by='movieId')
save(edx_md, file = "rdas/edx_md.rda")

kable(edx_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens edx data with average rating due to release date effect\\label{tbl:movielens_edx_avg_release_date_effect}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=4)
str(edx_md)
rm(res3_all_edx,res3_all_edx_2)
res3_all_val <- validation_md %>%
  group_by(movieId) %>%
  summarize(avg_rating_rel = mean(rating), n = n(), title=title[1], years=2018 - first(movie_dt)) %>%
  mutate(n_year = round(n / years))
# res3_all_val %>% 
#   group_by(n_year) %>% 
#   ggplot(aes(n_year,avg_rating_rel)) + geom_point() + geom_smooth()
# save(res3_all_val, file = "rdas/res3_all_val.rda")

res3_all_val_2 <- res3_all_val %>% select(-title)
validation_md <- validation_md %>% left_join(res3_all_val_2, by='movieId')
save(validation_md, file = "rdas/validation_md.rda")

kable(validation_md[1:5,], "latex", escape=TRUE, booktabs=TRUE, linesep="", caption="Movielens validation data with average rating due to release date effect\\label{tbl:movielens_validation_avg_release_date_effect}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=4)
str(validation_md)
rm(res3_all_val,res3_all_val_2,validation_md)
# Sys.time()
set.seed(1, sample.kind="Rounding")
test_index <- createDataPartition(y = edx_md$rating, times = 1,
                                  p = 0.2, list = FALSE)
train_set <- edx_md[-test_index,]
test_set <- edx_md[test_index,]

# To make sure we don't include users and movies in the test set that do not 
# appear in the training set, we remove these entries using the semi_join 
# function:

test_set <- test_set %>% 
  semi_join(train_set, by = "movieId") %>%
  semi_join(train_set, by = "userId")
RMSE <- function(true_ratings, predicted_ratings){
  sqrt(mean((true_ratings - predicted_ratings)^2))
}

save(test_index, file = "rdas/test_index.rda")
save(train_set, file = "rdas/train_set.rda")
save(test_set, file = "rdas/test_set.rda")

rm(edx_md, test_index)
(mu_hat <- mean(train_set$rating))
(model_1_rmse <- RMSE(test_set$rating, mu_hat))
predictions <- rep(2.5, nrow(test_set))
RMSE(test_set$rating, predictions)

predictions <- rep(3, nrow(test_set))
RMSE(test_set$rating, predictions)

predictions <- rep(4, nrow(test_set))
RMSE(test_set$rating, predictions)

rm(predictions)
rmse_results <- tibble(Index = "1", Method = "Just the average", RMSE = model_1_rmse)

save(rmse_results, file = "rdas/rmse_results.rda")
save(model_1_rmse, file = "rdas/model_1_rmse.rda")

rm(mu_hat,model_1_rmse)

# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Model 1\\label{tbl:rmse_results_model_1}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
mu <- mean(train_set$rating)

movie_avgs <- train_set %>% 
  group_by(movieId) %>% 
  summarize(b_i = mean(rating - mu))

save(mu, file = "rdas/mu.rda")
save(movie_avgs, file = "rdas/movie_avgs.rda")
movie_avgs %>%  
  ggplot(aes(b_i)) + 
  geom_histogram(bins = 10, color = "black") + 
  # scale_x_log10() +  # try with and without this line
  ggtitle("Movie effect or bias distribution")
predicted_ratings_model_2 <- mu + test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  .$b_i

(model_2_rmse <- RMSE(predicted_ratings_model_2, test_set$rating))

save(predicted_ratings_model_2, file = "rdas/predicted_ratings_model_2.rda")
save(model_2_rmse, file = "rdas/model_2_rmse.rda")

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "2", Method="Movie Effect Model",
                                 RMSE = model_2_rmse))

save(rmse_results, file = "rdas/rmse_results.rda")
rm(model_2_rmse,predicted_ratings_model_2)
# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-2\\label{tbl:rmse_results_model_1-2}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
train_set %>% 
  group_by(userId) %>% 
  summarize(b_u = mean(rating)) %>% 
  filter(n()>=100) %>%
  ggplot(aes(b_u)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for users who have rated over 100 movies")
train_set %>% 
  group_by(userId) %>% 
  summarize(b_u = mean(rating)) %>% 
  ggplot(aes(b_u)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for users who have rated any movies")
user_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  group_by(userId) %>%
  summarize(b_u = mean(rating - mu - b_i))

save(user_avgs, file = "rdas/user_avgs.rda")
user_avgs %>%  
  ggplot(aes(b_u)) + 
  geom_histogram(bins = 10, color = "black") + 
  # scale_x_log10() +  # try with and without this line
  ggtitle("User effect or bias distribution")
predicted_ratings_model_3 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>%
  mutate(pred = mu + b_i + b_u) %>%
  .$pred

(model_3_rmse <- RMSE(predicted_ratings_model_3, test_set$rating))

save(predicted_ratings_model_3, file = "rdas/predicted_ratings_model_3.rda")
save(model_3_rmse, file = "rdas/model_3_rmse.rda")
rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "3", Method="Movie + User Effects Model",  
                                 RMSE = model_3_rmse))
save(rmse_results, file = "rdas/rmse_results.rda")
rm(model_3_rmse, predicted_ratings_model_3)
# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-3\\label{tbl:rmse_results_model_1-3}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
train_set %>% 
  group_by(genres) %>% 
  summarize(mu_g = mean(rating)) %>% 
  ggplot(aes(mu_g)) + 
  geom_histogram(bins = 30, color = "black") + 
  ggtitle("Average rating for movies of category genres")
q8_1 <- train_set %>% 
  group_by(movieId) %>%
  summarize(n = n(),genres=genres[1])

q8_2 <- q8_1 %>% group_by(genres) %>% 
  summarise(nr=sum(n)) %>% 
  filter(nr>1000) 

q8 <- train_set %>% 
  group_by(genres) %>% 
  summarize(n = n(),avg = mean(rating), se = sd(rating)/sqrt(n())) %>%
  filter(n >= 1000) 

q8 %>% 
  mutate(genres = reorder(genres, avg)) %>% 
  ggplot(aes(x = genres, y = avg, ymin = avg - 2*se, ymax = avg + 2*se)) + 
  geom_point() +
  geom_errorbar() + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

mean(q8$avg)

median(q8$avg)

rm(q8_1,q8_2,q8)
genres_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>%
  group_by(genres) %>%
  summarize(b_g = mean(rating - mu - b_i - b_u))
genres_avgs %>%  
  ggplot(aes(b_g)) + 
  geom_histogram(bins = 10, color = "black") + 
  # scale_x_log10() +  # try with and without this line
  ggtitle("Genres effect or bias distribution")
predicted_ratings_model_4 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  mutate(pred = mu + b_i + b_u + b_g) %>% 
  .$pred

(model_4_rmse <- RMSE(predicted_ratings_model_4, test_set$rating))

save(genres_avgs, file = "rdas/genres_avgs.rda")
save(predicted_ratings_model_4, file = "rdas/predicted_ratings_model_4.rda")
save(model_4_rmse, file = "rdas/model_4_rmse.rda")

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "4", Method="Movie + User + Genres Effects Model",  
                                 RMSE = model_4_rmse))

save(rmse_results, file = "rdas/rmse_results.rda")

rm(model_4_rmse, predicted_ratings_model_4)
# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-4\\label{tbl:rmse_results_model_1-4}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
time_effect_avgs <- train_set %>%
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
  group_by(date) %>% 
  summarize(b_d = mean(avg_rating - mu - b_i - b_u - b_g))
time_effect_avgs %>%  
  ggplot(aes(b_d)) + 
  geom_histogram(bins = 10, color = "black") + 
  # scale_x_log10() +  # try with and without this line
  ggtitle("Rating time effect or bias distribution")
predicted_ratings_model_5 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  left_join(time_effect_avgs, by = "date") %>%
  mutate(pred = mu + b_i + b_u + b_g + b_d) %>%
  .$pred

(model_5_rmse <- RMSE(predicted_ratings_model_5, test_set$rating))

save(time_effect_avgs, file = "rdas/time_effect_avgs.rda")
save(predicted_ratings_model_5, file = "rdas/predicted_ratings_model_5.rda")
save(model_5_rmse, file = "rdas/model_5_rmse.rda")

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "5", Method="Movie + User + Genres + Rating Time Effects Model",  
                                 RMSE = model_5_rmse))

save(rmse_results, file = "rdas/rmse_results.rda")

rm(model_5_rmse, predicted_ratings_model_5)
# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-5\\label{tbl:rmse_results_model_1-5}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
rel_effect_avgs <- train_set %>%
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
  left_join(time_effect_avgs, by = "date") %>% 
  group_by(movieId) %>% 
  summarize(b_r = mean(avg_rating_rel - mu - b_i - b_u - b_g - b_d))

save(rel_effect_avgs, file = "rdas/rel_effect_avgs.rda")
rel_effect_avgs %>%  
  ggplot(aes(b_r)) + 
  geom_histogram(bins = 10, color = "black") + 
  # scale_x_log10() +  # try with and without this line
  ggtitle("Release Date effect or bias distribution")
predicted_ratings_model_6 <- test_set %>% 
  left_join(movie_avgs, by='movieId') %>% 
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by='genres') %>% 
  left_join(time_effect_avgs, by = "date") %>% 
  left_join(rel_effect_avgs, by='movieId') %>%
  mutate(pred = mu + b_i + b_u + b_g + b_d + b_r) %>%
  .$pred

(model_6_rmse <- RMSE(predicted_ratings_model_6, test_set$rating))

save(predicted_ratings_model_6, file = "rdas/predicted_ratings_model_6.rda")
save(model_6_rmse, file = "rdas/model_6_rmse.rda")

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "6", Method="Movie + User + Genres + Rating Time + Release date Effects Model",  
                                 RMSE = model_6_rmse))

rmse_results <- rmse_results %>% arrange(desc(RMSE))

save(rmse_results, file = "rdas/rmse_results.rda")
# rmse_results %>% knitr::kable()
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-6\\label{tbl:rmse_results_model_1-6}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
test_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  mutate(residual = rating - (mu + b_i)) %>%
  arrange(desc(abs(residual))) %>%  
  slice(1:10) %>% 
  pull(title) %>% 
    kable("latex", escape=FALSE, booktabs=TRUE, linesep="", 
          caption="Without Regularization 10 largest mistakes\\label{tbl:without_regularization_10_largest_mistakes}") %>% 
      kable_styling(latex_options=c("HOLD_position"))
load("rdas/edx.rda")
movie_titles <- edx %>% 
  select(movieId, title) %>%
  distinct()
save(movie_titles, file = "rdas/movie_titles.rda")
rm(edx)
movie_avgs %>% left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  slice(1:10)  %>% 
  pull(title)%>% 
  kable("latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Without Regularization 10 Best movies\\label{tbl:without_regularization_10_best_movies}") %>%
    kable_styling(latex_options=c("HOLD_position"))
movie_avgs %>% left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  slice(1:10)  %>% 
  pull(title)%>% 
  kable("latex", escape=FALSE, booktabs=TRUE, linesep="", caption="Without Regularization 10 Worst movies\\label{tbl:without_regularization_10_worst_movies}") %>%
    kable_styling(latex_options=c("HOLD_position"))
train_set %>% count(movieId) %>% 
  left_join(movie_avgs, by="movieId") %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  slice(1:10) %>% 
  pull(n)
train_set %>% count(movieId) %>% 
  left_join(movie_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  slice(1:10) %>% 
  pull(n)
lambdas <- seq(0, 10, 0.25)
mu <- mean(train_set$rating)
just_the_sum <- train_set %>% 
  group_by(movieId) %>% 
  summarize(s = sum(rating - mu), n_i = n())
rmses <- sapply(lambdas, function(l){
  predicted_ratings <- test_set %>% 
    left_join(just_the_sum, by='movieId') %>% 
    mutate(b_i = s/(n_i+l)) %>%
    mutate(pred = mu + b_i) %>%
    .$pred
  return(RMSE(predicted_ratings, test_set$rating))
})

(lambda <- lambdas[which.min(rmses)])
qplot(lambdas, rmses)  

mu <- mean(train_set$rating)
movie_reg_avgs <- train_set %>% 
  group_by(movieId) %>% 
  summarize(b_i = sum(rating - mu)/(n()+lambda), n_i = n()) 
rm(just_the_sum)
tibble(original = movie_avgs$b_i, 
       regularlized = movie_reg_avgs$b_i, 
       n = movie_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) + 
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("Movie Effect Reg")
train_set %>%
  dplyr::count(movieId) %>% 
  left_join(movie_reg_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(desc(b_i)) %>% 
  select(title, b_i, n) %>% 
  slice(1:10) %>% 
  kable("latex", escape=TRUE, booktabs=TRUE, linesep="", caption="With Regularization top 10 best movies based on the penalized estimates\\label{tbl:with_regularization_top_10_best_movies_based_on_the_penalized_estimates}") %>% kable_styling(latex_options=c("HOLD_position"))
train_set %>%
  dplyr::count(movieId) %>% 
  left_join(movie_reg_avgs) %>%
  left_join(movie_titles, by="movieId") %>%
  arrange(b_i) %>% 
  select(title, b_i, n) %>% 
  slice(1:10) %>% 
  kable("latex", escape=TRUE, booktabs=TRUE, linesep="", caption="With Regularization top 10 worst movies based on the penalized estimates\\label{tbl:with_regularization_top_10_worst_movies_based_on_the_penalized_estimates}") %>%
    kable_styling(latex_options=c("HOLD_position"))
predicted_ratings_model_7 <- test_set %>% 
  left_join(movie_reg_avgs, by='movieId') %>%
  mutate(pred = mu + b_i) %>%
  .$pred

(model_7_rmse <- RMSE(predicted_ratings_model_7, test_set$rating))
save(model_7_rmse, file = "rdas/model_7_rmse.rda")
# 
# ```
# ```{r, e_u_pls_4, echo=FALSE, eval=TRUE}

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "7", Method="Regularized Movie Effect Model - 1 fold CV",  
                                 RMSE = model_7_rmse))

save(rmse_results, file = "rdas/rmse_results.rda")

rm(model_7_rmse, predicted_ratings_model_7, movie_reg_avgs, lambda)
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-7\\label{tbl:rmse_results_model_1-7}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
lambdas <- seq(0, 10, 0.25)
rmses <- sapply(lambdas, function(l){
  mu <- mean(train_set$rating)
  b_i <- train_set %>%
    group_by(movieId) %>%
    summarize(b_i = sum(rating - mu)/(n()+l))
  b_u <- train_set %>% 
    left_join(b_i, by="movieId") %>%
    group_by(userId) %>%
    summarize(b_u = sum(rating - b_i - mu)/(n()+l))
  b_g <- train_set %>% 
    left_join(b_i, by='movieId') %>% 
    left_join(b_u, by='userId') %>%
    group_by(genres) %>% 
    summarize(b_g = sum(rating - mu - b_i - b_u)/(n()+l))
  predicted_ratings <- test_set %>% 
    left_join(b_i, by = "movieId") %>%
    left_join(b_u, by = "userId") %>% 
    left_join(b_g, by = "genres") %>% 
    mutate(pred = mu + b_i + b_u + b_g) %>%
    .$pred
  return(RMSE(predicted_ratings, test_set$rating))
})

qplot(lambdas, rmses)
(lambda <- lambdas[which.min(rmses)])
save(rmses, file = "rdas/rmses8.rda")
model_8_rmse <- min(rmses)
save(model_8_rmse, file = "rdas/model_8_rmse.rda")

rmse_results <- bind_rows(rmse_results,
                          tibble(Index = "8", Method="Regularized Movie + User + Genre Effects Model - 1 fold CV",  
                                 RMSE = model_8_rmse))

rmse_results <- rmse_results %>% arrange(desc(RMSE))

save(rmse_results, file = "rdas/rmse_results.rda")

rm(lambda, lambdas, rmses)
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-8\\label{tbl:rmse_results_model_1-8}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
lambdas <- seq(0, 10, 0.25)
rmses <- sapply(lambdas, function(l){
  mu <- mean(train_set$rating)
  b_i <- train_set %>%
    group_by(movieId) %>%
    summarize(b_i = sum(rating - mu)/(n()+l), n_i = n())
  b_u <- train_set %>% 
    left_join(b_i, by="movieId") %>%
    group_by(userId) %>%
    summarize(b_u = sum(rating - b_i - mu)/(n()+l), n_i = n())
  b_g <- train_set %>% 
    left_join(b_i, by='movieId') %>% 
    left_join(b_u, by='userId') %>%
    group_by(genres) %>% 
    summarize(b_g = sum(rating - mu - b_i - b_u)/(n()+l), n_i = n())
  b_d <- train_set %>%
    left_join(b_i, by='movieId') %>%
    left_join(b_u, by='userId') %>% 
    left_join(b_g, by = "genres") %>% 
    group_by(date) %>% 
    summarize(b_d = sum(avg_rating - mu - b_i - b_u - b_g)/(n()+l), n_i = n())
  b_r <- train_set %>%
    left_join(b_i, by='movieId') %>%
    left_join(b_u, by='userId') %>% 
    left_join(b_g, by = "genres") %>% 
    left_join(b_d, by = "date") %>% 
    group_by(movieId) %>% 
    summarize(b_r = sum(avg_rating_rel - mu - b_i - b_u - b_g - b_d)/(n()+l), n_i = n())

  predicted_ratings <- test_set %>%
    left_join(b_i, by = "movieId") %>%
    left_join(b_u, by = "userId") %>%
    left_join(b_g, by = "genres") %>% 
    left_join(b_d, by = "date") %>% 
    left_join(b_r, by='movieId') %>%
    mutate(pred = mu + b_i + b_u + b_g + b_d + b_r) %>% 
    .$pred
  
  return(RMSE(predicted_ratings, test_set$rating))
})

(lambda <- lambdas[which.min(rmses)])
qplot(lambdas, rmses)  

mu <- mean(train_set$rating)
movie_reg_avgs <- train_set %>%
    group_by(movieId) %>%
    summarize(b_i = sum(rating - mu)/(n()+lambda), n_i = n())
user_reg_avgs <- train_set %>% 
    left_join(movie_avgs, by="movieId") %>%
    group_by(userId) %>%
    summarize(b_u = sum(rating - b_i - mu)/(n()+lambda), n_i = n())
genres_reg_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
    group_by(genres) %>% 
    summarize(b_g = sum(rating - mu - b_i - b_u)/(n()+lambda), n_i = n())
time_effect_reg_avgs <- train_set %>% 
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
    group_by(date) %>% 
    summarize(b_d = sum(avg_rating - mu - b_i - b_u - b_g)/(n()+lambda), n_i = n())
rel_effect_reg_avgs <- train_set %>%
  left_join(movie_avgs, by='movieId') %>%
  left_join(user_avgs, by='userId') %>% 
  left_join(genres_avgs, by = "genres") %>% 
  left_join(time_effect_avgs, by = "date") %>% 
  group_by(movieId) %>% 
  summarize(b_r = sum(avg_rating_rel - mu - b_i - b_u - b_g - b_d)/(n()+lambda), n_i = n())

mp <- tibble(original = movie_avgs$b_i,
       regularlized = movie_reg_avgs$b_i,
       n = movie_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) +
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("Movie Effect Reg")
up <- tibble(original = user_avgs$b_u,
       regularlized = user_reg_avgs$b_u,
       n = user_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) +
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("User Effect Reg")
gp <- tibble(original = genres_avgs$b_g,
       regularlized = genres_reg_avgs$b_g,
       n = genres_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) +
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("Genres Effect Reg")
tp <- tibble(original = time_effect_avgs$b_d,
       regularlized = time_effect_reg_avgs$b_d,
       n = time_effect_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) +
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("Rating time Effect Reg")
rp <- tibble(original = rel_effect_avgs$b_r,
       regularlized = rel_effect_reg_avgs$b_r,
       n = rel_effect_reg_avgs$n_i) %>%
  ggplot(aes(original, regularlized, size=sqrt(n))) +
  geom_point(shape=1, alpha=0.5) + 
  ggtitle("Release date Effect Reg")

# require(gridExtra)
grid.arrange(mp, up, gp, tp, rp, ncol=2)
# save(rmses, file = "rmses.rda")
save(rmses, file = "rdas/rmses9.rda")
model_9_rmse <- min(rmses)
save(model_9_rmse, file = "rdas/model_9_rmse.rda")

rmse_results <- bind_rows(rmse_results, 
                          tibble(Index = "9", 
                                 Method="Regularized Movie + User + Genre + Rating Time + Release date Effect Model - 1 fold CV", 
                                 RMSE = model_9_rmse))
rmse_results <- rmse_results %>% arrange(desc(RMSE))
save(rmse_results, file = "rdas/rmse_results.rda")

rm(train_set,test_set,movie_avgs,user_avgs,genres_avgs,time_effect_avgs,rel_effect_avgs)
rm(lambda,lambdas, model_9_rmse,mu,rmses)
  kable(rmse_results, "latex", escape=FALSE, booktabs=TRUE, linesep="", caption="RMSE Results Models 1-9\\label{tbl:rmse_results_model_1-9}") %>%
    kable_styling(latex_options=c("HOLD_position"), font_size=7)
	knitr::knit_exit()
options(tinytex.verbose = TRUE)
fit <- lm(mpg ~ cyl + disp, mtcars)
# show the theoretical model
equatiomatic::extract_eq(fit)
options(tinytex.verbose = FALSE)
```

---  

\newpage

Terms like generate\index{generate} and some\index{others} will also show up.

\printindex



```r
	knitr::knit_exit()
```







