---
title: "Movielens  \n Movie Recommendation System  \n A Harvard Capstone Project"
author: "Manoj Bijoor"
email: manoj.bijoor@gmail.com
date: "March 18, 2021"
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

<!-- $Y_{u,i}=\mu+b_{i}+b_{u}+f(d_{u,i})+\epsilon_{u,i}$, with f a smooth function of $d_{u,i}$ -->

  <!-- Y_{u,i} = \mu + b_{i} + b_{u}  + f(d_{u,i})+ \epsilon_{u,i}\text{, with f a smooth function of }d_{u,i} -->

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

![25 Movies with the most ratings per year and their average rating post 1993\label{fig:25_movies_avg_and_most_ratings_per_year_post_1993}](figures/md_3-1.pdf) 

\newpage
We see that the most rated movies tend to have above average ratings. This is not surprising: more people watch popular movies. To confirm this, we stratify the post 1993 movies by ratings per year and compute their average ratings. Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993} is a plot of average ratings versus ratings per year showing an estimate of the trend.

We see that the more a movie is rated, the higher the rating.

**Post-1993 movies**   

```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year post  1993\label{fig:movies_average_ratings_versus_ratings_per_year_post_1993}](figures/md_4-1.pdf) 

\newpage
**Pre-1993 movies**  
Compare Pre-1993 movies trend shown here in Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_pre_1993} Versus Post-1993 movies trend in Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993} above.


```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year pre  1993\label{fig:movies_average_ratings_versus_ratings_per_year_pre_1993}](figures/md_5-1.pdf) 


\newpage
**_Modify edx data for Release Date Effect_**
We stratify the movies by ratings per year and compute their average ratings based on what we learnt above, where we confirmed our intuition that more people watch popular movies. Finally edx data table looks as shown in Table \ref{tbl:movielens_edx_avg_release_date_effect} below. Figure \ref{fig:movies_average_ratings_versus_ratings_per_year_for_all_years_for_edx}"} is a plot of average ratings versus ratings per year showing an estimate of the trend.

**_All Years_**

```
`geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```

![Movies average ratings versus ratings per year for all years for edx\label{fig:movies_average_ratings_versus_ratings_per_year_for_all_years_for_edx}](figures/md_6-1.pdf) 


\begin{table}[H]

\caption{\label{tab:md_7}Movielens edx data with average rating due to release date effect\label{tbl:movielens_edx_avg_release_date_effect}}
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

\caption{\label{tab:md_8}Movielens validation data with average rating due to release date effect\label{tbl:movielens_validation_avg_release_date_effect}}
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

<!-- $$RMSE=\sqrt{\frac{1}{N}\sum_{u,i}(\hat{y}_{u,i}-y_{u,i})^{2}}$$ -->  

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

<!-- $$Y_{i,i}=\mu+\epsilon_{u,i}$$ -->

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

<!-- $$Y_{u,i}=\mu+b_{i}+\epsilon_{u,i}$$ -->

\equations{Model 2: Movie effects linear model Equation \ref{eq:EqModel2-1}}
\label{eq:EqModel2-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + \epsilon_{u,i}
\end{equation}

Statistics textbooks refer to the *b*s as effects or *"bias"*.

We can again use least squares to estimate the $b_{i}$ in the following way to get Equation \ref{eq:EqModel2-2}: 

<!-- ```{r, m_2_1, echo=TRUE, eval=FALSE, collapse = TRUE, comment="", highlight=TRUE, background='#F7F7F7', tidy=TRUE} -->
<!-- fit <- lm(rating ~ as.factor(userId), data = train_set) -->
<!-- ``` -->

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
movie_avgs <- train_set %>% group_by(movieId) %>% summarize(b_i = mean(rating - 
    mu))
```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_2}

![Movie effect or bias distribution\label{fig:model_2}](figures/m_2_3-1.pdf) 

Remember $\hat{\mu}$=3.5 so a $b_{i}$=1.5 implies a perfect five star rating.
Let's see how much our prediction improves once we use $\hat{y_{u,i}}=\hat{\mu}+\hat{b_{i}}$:


```r
predicted_ratings_model_2 <- mu + test_set %>% left_join(movie_avgs, 
    by = "movieId") %>% .$b_i
(model_2_rmse <- RMSE(predicted_ratings_model_2, test_set$rating))
[1] 0.9437429
```

\newpage
### Results Model 1-2

Let's add the movie effects model to our results table to get Table \ref{tbl:rmse_results_model_1-2}

\begin{table}[H]

\caption{\label{tab:m_2_5}RMSE Results Model 1-2\label{tbl:rmse_results_model_1-2}}
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
train_set %>% group_by(userId) %>% summarize(b_u = mean(rating)) %>% 
    filter(n() >= 100) %>% ggplot(aes(b_u)) + geom_histogram(bins = 30, 
    color = "black") + ggtitle("Average rating for users who have rated over 100 movies")
```

![Average rating for users who have rated over 100 movies\label{fig:average_ratings_for_users_who_have_rated_over_100_movies}](figures/ue_1-1.pdf) 

\newpage
Let's compute *b_u* the average rating for user *u* for those that have rated any movies, see Figure \ref{fig:average_ratings_for_users_who_have_rated_any_movies}


```r
train_set %>% group_by(userId) %>% summarize(b_u = mean(rating)) %>% 
    ggplot(aes(b_u)) + geom_histogram(bins = 30, color = "black") + 
    ggtitle("Average rating for users who have rated any movies")
```

![Average rating for users who have rated any movies\label{fig:average_ratings_for_users_who_have_rated_any_movies}](figures/ue_2-1.pdf) 

Notice that there is substantial variability across users as well: some users are very cranky and others love every movie. This implies that a further improvement to our model may be as shown in Equation \ref{eq:EqModel3-1}:

<!-- $$Y_{u,i}=\mu+b_{i}+b_{u}+\epsilon_{u,i}$$ -->

\equations{Model 3: Movie + User effects linear model Equation \ref{eq:EqModel3-1}}
\label{eq:EqModel3-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \epsilon_{u,i}
\end{equation}

where $b_{u}$ is a user-specific effect. Now if a cranky user (negative $b_{u}$) rates a great movie (positive $b_{i}$), the effects counter each other and we may be able to correctly predict that this user gave this great movie a 3 rather than a 5.

To fit this model, we could again use lm() as shown in Equation \ref{eq:EqModel3-2}:

<!-- ```{r, ue_3, echo=TRUE, eval=FALSE} -->
<!-- fit <- lm(rating ~ as.factor(movieId) + as.factor(userId), data = train_set) -->
<!-- ``` -->

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
user_avgs <- train_set %>% left_join(movie_avgs, by = "movieId") %>% 
    group_by(userId) %>% summarize(b_u = mean(rating - mu - b_i))
```

We can see that these estimates vary substantially, see Figure \ref{fig:model_3}

![User effect or bias distribution\label{fig:model_3}](figures/ue_4-1.pdf) 

We can now construct predictors and see how much the RMSE improves:


```r
predicted_ratings_model_3 <- test_set %>% left_join(movie_avgs, 
    by = "movieId") %>% left_join(user_avgs, by = "userId") %>% 
    mutate(pred = mu + b_i + b_u) %>% .$pred
(model_3_rmse <- RMSE(predicted_ratings_model_3, test_set$rating))
[1] 0.865932
```

\newpage
### Results Table Model 1-3
Let's add the user effects model to our results table to get Table \ref{tbl:rmse_results_model_1-3}

\begin{table}[H]

\caption{\label{tab:ue_6}RMSE Results Model 1-3\label{tbl:rmse_results_model_1-3}}
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

<!-- $$Y_{u,i}=\mu+b_{i}+b_{u}+\sum_{k=1}^Kx_{u,i}\beta_k+\epsilon_{u,i}$$ -->
<!-- with $x_{u,i}^k=1$ if $g_{u,i}$ is genre *k* -->

\equations{Model 4: Movie + User + Genre effects linear model Equation \ref{eq:EqModel4-1}}
\label{eq:EqModel4-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + \epsilon_{u,i}
\end{equation}

\begin{center}
with $x_{u,i}^k=1$ if $g_{u,i}$ is genre *k*
\end{center}


```r
train_set %>% group_by(genres) %>% summarize(mu_g = mean(rating)) %>% 
    ggplot(aes(mu_g)) + geom_histogram(bins = 30, color = "black") + 
    ggtitle("Average rating for movies of category genres")
```

![Average rating for movies of category genres\label{fig:average_ratings_for_movies_of_category_genres}](figures/ge_1-1.pdf) 



To fit this model, we could again use the lm() function as shown in Equation \ref{eq:EqModel4-2}:

<!-- ```{r, ge_3, echo=TRUE, eval=FALSE} -->
<!-- fit <- lm(rating ~ as.factor(movieId) + as.factor(userId) + as.factor(genres), data = train_set) -->
<!-- ``` -->

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
genres_avgs <- train_set %>% left_join(movie_avgs, by = "movieId") %>% 
    left_join(user_avgs, by = "userId") %>% group_by(genres) %>% 
    summarize(b_g = mean(rating - mu - b_i - b_u))
```

<!-- \newpage -->
We can see that these estimates vary substantially, see Figure \ref{fig:model_4}

![Genres effect or bias distribution\label{fig:model_4}](figures/ge_5-1.pdf) 

We can now construct predictors and see how much the RMSE improves:


```r
predicted_ratings_model_4 <- test_set %>% left_join(movie_avgs, 
    by = "movieId") %>% left_join(user_avgs, by = "userId") %>% 
    left_join(genres_avgs, by = "genres") %>% mutate(pred = mu + 
    b_i + b_u + b_g) %>% .$pred
(model_4_rmse <- RMSE(predicted_ratings_model_4, test_set$rating))
[1] 0.8655941
```

\newpage
### Results Table Model 1-4

Let's add the genres effects model to our results table to get Table \ref{tbl:rmse_results_model_1-4}

\begin{table}[H]

\caption{\label{tab:ge_7}RMSE Results Model 1-4\label{tbl:rmse_results_model_1-4}}
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
## Model 5 : Rating Time effect
The movielens dataset also includes a time stamp. This variable represents the time and date in which the rating was provided. Earlier in the EDA/Data wrangling section we created a new column date with the time stamp.   

We computed the average rating for each week and plotted this average against day.   

The plot shows some evidence of a time effect. If we define $d_{u,i}$ as the day for user's *u* rating of movie *i*, then the following updated model as shown in Equation \ref{eq:EqModel5-1} is most appropriate:   
<!-- $$Y_{u,i}=\mu+b_{i}+b_{u}+\sum_{k=1}^Kx_{u,i}\beta_k+f(d_{u,i})+\epsilon_{u,i}$$    -->

\equations{Model 5: Movie + User + Genre + Rating time effects linear model Equation \ref{eq:EqModel5-1}}
\label{eq:EqModel5-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + f(d_{u,i}) + \epsilon_{u,i}
\end{equation}

\begin{center}
with f a smooth function of $d_{u,i}$
\end{center}

To fit this model, we could again use lm() function as shown in Equation \ref{eq:EqModel5-2}:

<!-- ```{r, rte_1, echo=TRUE, eval=FALSE} -->
<!-- fit <- lm(rating ~ as.factor(movieId) + as.factor(userId) + as.factor(genres) + as.factor(date), data = train_set) -->
<!-- ``` -->
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
time_effect_avgs <- train_set %>% left_join(movie_avgs, by = "movieId") %>% 
    left_join(user_avgs, by = "userId") %>% left_join(genres_avgs, 
    by = "genres") %>% group_by(date) %>% summarize(b_d = mean(avg_rating - 
    mu - b_i - b_u - b_g))
```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_4}

![Rating time effect or bias distribution\label{fig:model_5}](figures/rte_3-1.pdf) 

We can now construct predictors and see how much the RMSE improves


```r
predicted_ratings_model_5 <- test_set %>% left_join(movie_avgs, 
    by = "movieId") %>% left_join(user_avgs, by = "userId") %>% 
    left_join(genres_avgs, by = "genres") %>% left_join(time_effect_avgs, 
    by = "date") %>% mutate(pred = mu + b_i + b_u + b_g + b_d) %>% 
    .$pred
(model_5_rmse <- RMSE(predicted_ratings_model_5, test_set$rating))
[1] 0.8654205
```

\newpage
### Results Table Model 1-5
Let's add the Rating Time effects model to our results table to get Table \ref{tbl:rmse_results_model_1-5}

\begin{table}[H]

\caption{\label{tab:rte_5}RMSE Results Model 1-5\label{tbl:rmse_results_model_1-5}}
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


<!-- ```{r knitr_knit_exit} -->
<!-- 	knitr::knit_exit() -->
<!-- ``` -->

\newpage
## Model 6 Release Date Effect
The plots in Figures \ref{fig:ratings_movie_release_date_all_dates},  \ref{fig:25_movies_avg_and_most_ratings_per_year_post_1993}, \ref{fig:movies_average_ratings_versus_ratings_per_year_post_1993}, \ref{fig:movies_average_ratings_versus_ratings_per_year_pre_1993} above shows some evidence of a Release Date effect based on the when the movie was released and it's popularity given by the mean rating. If we define $arr_{r,i,y}$ as the average rating *r=mean(rating)* since release date *y=n_year* for movie *i* (in the formula for plots above), then the following updated model is most appropriate:   
<!-- $$Y_{u,i}=\mu+b_{i}+b_{u}+\sum_{k=1}^Kx_{u,i}\beta_k+f(d_{u,i})+f(arr_{r,i,y})+\epsilon_{u,i}$$    -->
<!-- with f a smooth function of $arr_{r,i,y}$ -->

\equations{Model 6: Movie + User + Genre + Rating time + Release date effects linear model Equation \ref{eq:EqModel6-1}}
\label{eq:EqModel6-1}
\begin{equation}
  Y_{u,i} = \mu + b_{i} + b_{u} + \sum_{k=1}^Kx_{u,i}\beta_k + f(d_{u,i}) + f(arr_{r,i,y}) + \epsilon_{u,i}
\end{equation}

\begin{center}
with f a smooth function of $arr_{r,i,y}$
\end{center}

To fit this model, we could again use lm() function as shown in Equation \ref{eq:EqModel6-2}:

<!-- ```{r, m5_rd_1, echo=TRUE, eval=FALSE} -->
<!-- Sys.time() -->
<!-- fit <- lm(rating ~ as.factor(movieId) + as.factor(userId) + as.factor(genres) + as.factor(date) + as.factor(movie_dt), data = edx_md2) -->
<!-- ``` -->

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
rel_effect_avgs <- train_set %>% left_join(movie_avgs, by = "movieId") %>% 
    left_join(user_avgs, by = "userId") %>% left_join(genres_avgs, 
    by = "genres") %>% left_join(time_effect_avgs, by = "date") %>% 
    group_by(movieId) %>% summarize(b_r = mean(avg_rating_rel - 
    mu - b_i - b_u - b_g - b_d))
```

\newpage
We can see that these estimates vary substantially, see Figure \ref{fig:model_6}

![Release Date effect or bias distribution\label{fig:model_6}](figures/rde_3-1.pdf) 

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
### Results Table
Let's add the Release Date effects model to our results table
\begin{table}[H]

\caption{\label{tab:rde_5}RMSE Results Model 1-6\label{tbl:rmse_results_model_1-6}}
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
6 & Movie + User + Genres + Time + Releasedate Effects Model & 0.8633330\\
\bottomrule
\end{tabular}
\end{table}


```r
	knitr::knit_exit()
```









