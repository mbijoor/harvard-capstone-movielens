---
title: "Movielens  \n Movie Recommendation System  \n A Harvard Capstone Project"
author: "Manoj Bijoor"
email: manoj.bijoor@gmail.com
date: "March 14, 2021"
output: 
  pdf_document: 
    latex_engine: xelatex
    number_sections: yes
    keep_tex: yes
    keep_md: yes
    df_print: kable
    highlight: pygments
    extra_dependencies: "subfig"
  md_document:
    variant: markdown_github 
    # check https://bookdown.org/yihui/rmarkdown/markdown-document.html#markdown-variants
  github_document: default
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
  \usepackage[]{hyperref}
  \hypersetup{
    backref,
    colorlinks=true,
    linkcolor=blue,
    filecolor=magenta,      
    urlcolor=cyan,
    pdftitle={"Movielens Harvard Capstone"},
    bookmarks=true,
    pdfpagemode=FullScreen,
    pdfstartpage=1,
    hyperindex=true,
    pageanchor=true,
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

  # \usepackage{letltxmacro}%
  # \usepackage{float}
  # \usepackage{flafter}
  # \usepackage[titles]{tocloft}
---



<!-- #```{r, time_it = TRUE, eval=FALSE, echo=FALSE} -->
<!-- # Sys.sleep(2) -->
<!-- #``` -->



<!-- ------------------------------ -->

\pagenumbering{roman}     <!-- first page with Roman numbering -->

\newpage                  <!-- new page -->

<!-- ------------------------------ -->

\newpage 

\begin{center}

\large{Abstract}

\end{center}

\bigskip

...this is the abstract text...

<!-- ------------------------------ -->

\newpage 
\clearpage
\phantomsection
\setcounter{secnumdepth}{5}
\setcounter{tocdepth}{5}

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
\par\noindent\textbf{equations \theequations. #1}
\addcontentsline{equ}{equations}{\thesection. \protect\numberline{\theequations}#1}\par}
\xpretocmd{\listofequations}{\addcontentsline{toc}{section}{\listequationsname}}{}{}

<!-- \setlength{\cftequationsindent}{1.5em} -->
<!-- \setlength{\cftequationsnumwidth}{2.3em} -->
<!-- \renewcommand{\cftequtitlefont}{\normalfont\Large\bfseries} -->

\makeatletter
\let\l@equations\l@figure
\makeatother

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
I will develop my algorithm using the edx set. For a final test of my final algorithm, I predict movie ratings in the validation set (the final hold-out test set) as if they were unknown. [RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation) will be used to evaluate how close my predictions are to the true values in the validation set (the final hold-out test set).  


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

#### Movie Release Date - a closer look

Computing the number of ratings for each movie and then plotting it against the year the movie came out, that is the release date and using the square root transformation on the counts using Table \ref{tbl:movielens_edx_movie_release_date} , we get see Figure \ref{fig:ratings_movie_release_date_all_dates} :

<!-- # ```{r, md_1, echo=FALSE, eval=TRUE, comment="", fig.pos="h!", fig.show="hold", out.width="50%", fig.cap="Ratings Movie Release Date - All dates\\label{fig:ratings_movie_release_date_all_dates}"} -->

\begin{figure}[h!]

{\centering \subfloat[All data points only\label{fig:md_1-1}]{\includegraphics[width=0.7\linewidth]{figures/md_1-1} }\newline\subfloat[Smooth line through all data points\label{fig:md_1-2}]{\includegraphics[width=0.7\linewidth]{figures/md_1-2} }

}

\caption{Ratings Movie Release Date - All dates\label{fig:ratings_movie_release_date_all_dates}}\label{fig:md_1}
\end{figure}

we see that, on average, movies that came out after 1993 get more ratings. We also see that with newer movies, starting in 1993, the number of ratings decreases with year: the more recent a movie is, the less time users have had to rate it.


\newpage
Among movies that came out in 1991 or later, we select the top 25 movies with the highest average number of ratings per year (n/year) and calculate the average rating of each of them. To calculate number of ratings per year, use 2018 as the end year. See Figure \ref{fig:25_movies_avg_and_most_ratings_per_year_post_1991} :


```
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![25 Movies with the most ratings per year and their average rating post 1991\label{fig:25_movies_avg_and_most_ratings_per_year_post_1991}](figures/md_2-1.pdf) 

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
#### Movie Rating Date-Time - a closer look  

The Movielens edx data Table \ref{tbl:movielens_data}  also includes a time stamp. This variable represents the time and date in which the rating was provided. The units are seconds since January 1, 1970. We create a new column date with the date named _rating_date_ in subsection [Modify edx](#modify_edx) to get Table \ref{tbl:movielens_edx_movie_release_date} . 

We compute the average rating for each week and plot this average against day. See Figure \ref{fig:movies_average_ratings_for_each_week_versus_day} : 


```
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![Movies average ratings for each week versus day\label{fig:movies_average_ratings_for_each_week_versus_day}](figures/rd_1-1.pdf) 

The plot shows some evidence of a time effect. If we define $d_{u,i}$ as the day for user's *u* rating of movie *i*, then the following model given by Equation \ref{eq:EqTimeEffect} is most appropriate:

<!-- $Y_{u,i}=\mu+b_{i}+b_{u}+f(d_{u,i})+\epsilon_{u,i}$, with f a smooth function of $d_{u,i}$ -->

\equations{Equation \ref{eq:EqTimeEffect}}
\label{eq:EqTimeEffect}
\begin{equation}
Y_{u,i}=\mu+b_{i}+b_{u}+f(d_{u,i})+\epsilon_{u,i}\text{, with f a smooth function of }d_{u,i}
\end{equation}

**_Modify edx_**
Let's update the **_edx_** table to get Table \ref{tbl:movielens_edx_avg_rating_time_effect} below:

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
#### Genres combinations per movie - a closer look   

The movielens data Table \ref{tbl:movielens_edx_avg_rating_time_effect} also has a genres column. This column includes every genre that applies to the movie. Some movies fall under several genres. We define a category as whatever combination appears in this column. 

Here we keep only categories with more than 1,000 ratings. Then compute the average and standard error for each category, and plot these as error bar plots. See Figure \ref{fig:genres_error_bar_plots} : 

![Movies genres error bar plots\label{fig:genres_error_bar_plots}](figures/gnr-1.pdf) 

The plot shows strong evidence of a genre effect.


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

res2 <- edx_md %>%
  filter(movie_dt >= 1991) %>%
  group_by(movieId) %>%
  # summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - min(movie_dt)) %>% 
  summarize(avg_rating = mean(rating), n = n(), title=title[1], years=2018 - first(movie_dt)) %>%
  mutate(n_year = n / years) %>%
  top_n(25, n_year) %>%
  arrange(desc(n_year))

# qplot(res2$years,res2$n)
ggplot(res2,aes(years,n)) + 
  geom_line() + 
  geom_smooth() + 
  geom_vline(xintercept = 25) + # 1993
  geom_vline(xintercept = 27) # 1991
rm(res2)
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

rm(validation_md)
edx_md %>% group_by(genres) %>%
  summarize(n = n(), avg = mean(rating), se = sd(rating)/sqrt(n())) %>%
  filter(n >= 1000) %>% 
  mutate(genres = reorder(genres, avg)) %>%
  ggplot(aes(x = genres, y = avg, ymin = avg - 2*se, ymax = avg + 2*se)) + 
  geom_point() +
  geom_errorbar() + 
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
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







