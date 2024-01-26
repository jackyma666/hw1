# hw1
---
title: "Homework 1"
author: "[Yuchen Ma]{style='background-color: yellow;'}"
toc: true
title-block-banner: true
title-block-style: default
format: html
#format: pdf
---

[Link to the Github repository](https://github.com/STAT380/hw1.git)

---
git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

::: {.callout-important}
## Due: Fri, Jan 26, 2024 @ 11:59pm



Please read the instructions carefully before submitting your assignment. 

1. This assignment requires you to:
    - Upload your Quarto markdown files to a `git` repository
    - Upload a `PDF` file on Canvas

1. Don't collapse any code cells before submitting. 

1. Remember to make sure all your code output is rendered properly before uploading your submission.


⚠️ Please add your name to the the author information in the frontmatter before submitting your assignment. 
:::

<br><br><br><br>
---

## Question 1
::: {.callout-tip}
## 20 points
:::


In this question, we will walk through the process of _forking_ a `git` repository and submitting a _pull request_. 

1. Navigate to the Github repository [here](https://github.com/STAT380/hw1.git) and fork it by clicking on the icon in the top right

![](images/fork.png)

> Provide a sensible name for your forked repository when prompted. 

2. Clone your Github repository on your local machine

    ```{bash}
    $ git clone <<https://github.com/jackyma666/hw1>>
    $ cd hw-1
    ```

3. In order to activate the `R` environment for the homework, make sure you have `renv` installed beforehand. To activate the `renv` environment for this assignment, open an instance of the `R` console from within the directory and type
    
    ```{R}
    renv::activate()
    ```
    Follow the instrutions in order to make sure that `renv` is configured correctly. 

4. Work on the _reminaing part_ of this assignment as a `.qmd` file.

    - Create a `PDF` and `HTML` file for your output by modifying the YAML frontmatter for the Quarto `.qmd` document

5. When you're done working on your assignment, push the changes to your github repository.

6. Navigate to the original Github repository [here](https://github.com/STAT380/hw1.git) and submit a pull request linking to your repository. 
    
    Remember to **include your name** in the pull request information!

If you're stuck at any step along the way, you can refer to the [official Github docs here](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)


<br><br><br><br>
<br><br><br><br>
---

## Question 2
::: {.callout-tip}
## 30 points
:::

Consider the following vector

```{R}
my_vec <- c(
    "+0.07",
    "-0.07",
    "+0.25",
    "-0.84",
    "+0.32",
    "-0.24",
    "-0.97",
    "-0.36",
    "+1.76",
    "-0.36"
)
```


For the following questions, provide your answers in a code cell.

1. What data type does the vector contain?
```{r}
#string
```
1. Create two new vectors called `my_vec_double` and `my_vec_int` which converts `my_vec` to Double & Integer types, respectively, 
```{r}
my_vec_double <- as.numeric(my_vec)

my_vec_int <- as.integer(as.numeric(my_vec))
```

1. Create a new vector `my_vec_bool` which comprises of:
    * ```r TRUE```if an element in `my_vec_double` is $\le 0$
    * ```r FALSE``` if an element in `my_vec_double` is $\ge 0$

    How many elements of `my_vec_double` are greater than zero?
```{r}
# Create a new logical vector based on the condition
my_vec_bool <- my_vec_double > 0

# Count how many elements are greater than zero
count_greater_than_zero <- sum(my_vec_bool)

# Output
my_vec_bool
count_greater_than_zero
    
```    

1. Sort the values of `my_vec_double` in ascending order. 

```{r}
my_vec_double_sorted <- sort(my_vec_double)

```
<br><br><br><br>
<br><br><br><br>
---

## Question 3
::: {.callout-tip}
## 50 points
:::

In this question we will get a better understanding of how `R` handles large data structures in memory. 

1. Provide `R` code to construct the following matrices:
$$
\begin{bmatrix} 
1 & 2 & 3\\
4 & 5 & 6\\
7 & 8 & 9\\
\end{bmatrix}
\quad \text{ and } \quad
\begin{bmatrix} 
1 & 2 & 3 & 4 & 5 & \dots & 100\\
1 & 4 & 9 & 16 & 25 & \dots & 10000\\
\end{bmatrix}
$$
```{r}
matrix_3x3 <- matrix(1:9, nrow = 3, byrow = TRUE)

first_row <- 1:100
second_row <- first_row^2
matrix_2x100 <- rbind(first_row, second_row)

# Output
matrix_3x3
matrix_2x100

```

::: {.callout-warning}
## Tip

Recall the discussion in class on how `R` fills in matrices
:::

In the next part, we will discover how knowledge of the way in which a matrix is stored in memory can inform better code choices. To this end, the following function takes an input $n$ and creates an $n \times n$ matrix with random entries. 

```{R}
generate_matrix <- function(n){
    return(
        matrix(
            rnorm(n^2),
            nrow=n
        )
    )
}
```

For example:

```{R}
generate_matrix(4)
```


Let `M` be a fixed $50 \times 50$ matrix

```{R}
M <- generate_matrix(50)
mean(M)
```


2. Write a function `row_wise_scan` which scans the entries of `M` one row after another and outputs the number of elements whose value is $\ge 0$. You can use the following **starter code**

```{R}
row_wise_scan <- function(x){
    n <- nrow(x)
    m <- ncol(x)

    # Insert your code here
    count <- 0
    for(i in 1:n){
        for(j in 1:m){
            if(x[i,j] >=0){
                count <- count + 1 
            }
        }
    }

    return(count)
}
```


3. Similarly, write a function `col_wise_scan` which does exactly the same thing but scans the entries of `M` one column after another

```{R}
col_wise_scan <- function(x){
    count <- 0
    
    n <- nrow(x)
    m <- ncol(x)
  
    for(j in 1:m){
        for(i in 1:n){
            if(x[i, j] >= 0){
                count <- count + 1
      }
    }
  }

    return(count)
}
```
You can check if your code is doing what it's supposed to using the function here[^footnote]

4. Between `col_wise_scan` and `row_wise_scan`, which function do you expect to take shorter to run? Why?
```{r}
#I expect col_wise_scan runing faster, because it accesses the elements of the matrix in the order they are stored in memory.
```
5. Write a function `time_scan` which takes in a method `f` and a matrix `M` and outputs the amount of time taken to run `f(M)`

```{R}
time_scan <- function(f, M){
    initial_time <- Sys.time()
    f(M)
    final_time <- Sys.time()
    
    total_time_taken <- final_time - initial_time
    return(total_time_taken)
}
```

Provide your output to

```{R}
times <- list(
    row_wise_time = time_scan(row_wise_scan, M),
    col_wise_time = time_scan(col_wise_scan, M)
)
times
```
Which took longer to run? 
```{r}
##$row_wise_time is faster

```
6. Repeat this experiment now when:
    * `M` is a $100 \times 100$ matrix
    * `M` is a $1000 \times 1000$ matrix
    * `M` is a $5000 \times 5000$ matrix
```{r}
M_100 <- matrix(runif(100 * 100), nrow = 100)
M_1000 <- matrix(runif(1000 * 1000), nrow = 1000)
M_5000 <- matrix(runif(5000 * 5000), nrow = 5000)


time_100 <- list(
  row_wise_time = time_scan(row_wise_scan, M_100),
  col_wise_time = time_scan(col_wise_scan, M_100)
)


time_1000 <- list(
  row_wise_time = time_scan(row_wise_scan, M_1000),
  col_wise_time = time_scan(col_wise_scan, M_1000)
)


time_5000 <- list(
  row_wise_time = time_scan(row_wise_scan, M_5000),
  col_wise_time = time_scan(col_wise_scan, M_5000)
)

# Output 
list(times_100 = time_100, times_1000 = time_1000, times_5000 = time_5000)

```

What can you conclude?
```{r}
#Column-wise scanning is consistently faster than row-wise in R, especially for large matrices.
```

<br><br><br><br>
<br><br><br><br>
---

# Appendix

Print your `R` session information using the following command

```{R}
sessionInfo()
```

[^footnote]: If your code is right, the following code should evaluate to be `TRUE`

    ``` R
    sapply(1:100, function(i) {
        x <- generate_matrix(100)
        row_wise_scan(x) == col_wise_scan(x)
    }) %>% sum == 100
    ```
