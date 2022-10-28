# (PART\*) R scripting {-}



# Control structures

<br/><small>*This chapter is currently a draft.*</small>

<br/><small><a href="javascript:if(window.print)window.print()">Print this chapter</a></small>

<!--
This chapter illustrates the use of **conditional statements** and code **loops**. However, those constructs require a bit more than the single lines of code we have seen so far.
-->


## Conditional statements

Conditional statements are fundamental in (procedural) programming, as they allow to execute or not execute part of a procedure depending on whether a certain condition is true. The condition is tested, and the part of the procedure to execute in the case the condition is true is included in a *code block*.


```r
temperature <- 25

if (temperature > 25) {
  cat("It really warm today!")
}
```

A simple conditional statement can be created using `if` as in the example above. A more complex structure can be created using both `if` and `else` to provide not only a procedure to execute in case the condition is true but also an alternative procedure to be executed when the condition is false.


```r
temperature <- 12

if (temperature > 25) {
  cat("It really warm today!")
} else {
  cat("Today is not warm")
}
```

```
## Today is not warm
```

Finally, conditional statements can be **nested**. That is, a conditional statement can be included as part of the code block to be executed after the condition is tested. For instance, in the example below, a second conditional statement is included in the code block to be executed in the case the condition is false.


```r
temperature <- -5

if (temperature > 25) {
  cat("It really warm today!")
} else {
  if (temperature > 0) {
    cat("There is a nice temperature today")
  } else {
    cat("This is really cold!")
  }
}
```

```
## This is really cold!
```

Similarly, the first example seen in the lecture should be coded as follows.


```r
a_value <- -7

if (a_value == 0) {
  cat("Zero")
} else {
  if (a_value < 0) {
    cat("Negative") 
  } else {
    cat("Positive")
  }
}
```

```
## Negative
```



## Loops

Loops are another core component of (procedural) programming and implement the idea of solving a problem or executing a task by performing the same set of steps a number of times.  There are two main kinds of loops in R - **deterministic** and **conditional** loops.  The former is executed a fixed number of times,  specified at the beginning of the loop.  The latter is executed until a specific condition is met. Both deterministic and conditional loops are extremely important in working with vectors.


### Conditional Loops

In R, conditional loops can be implemented using `while` and `repeat`. The difference between the two is mostly syntactical: the first tests the condition first and then execute the related code block if the condition is true; the second executes the code block until a `break` command is given (usually through a conditional statement).


```r
a_value <- 0
# Keep printing as long as x is smaller than 2
while (a_value < 2) {
  cat(a_value, "\n")
  a_value <- a_value + 1
}
```

```
## 0 
## 1
```


```r
a_value <- 0
# Keep printing, if x is greater or equal than 2 than stop
repeat {
  cat(a_value, "\n")
  a_value <- a_value + 1
  if (a_value >= 2) break
}
```

```
## 0 
## 1
```



### Deterministic Loops

The deterministic loop executes the subsequent code block iterating through the elements of a provided vector. During each iteration (i.e., execution of the code block), the current element of the vector (*<VECTOR>* in the definition below) is assigned to the variable in the statement (*<VAR>* in the definition below), and it can be used in the code block.


```r
for (<VAR> in <VECTOR>) {
    ... code in loop ... 
    }
```

It is, for instance, possible to iterate over a vector and print each of its elements.


```r
east_midlands_cities <- c("Derby", "Leicester", "Lincoln", "Nottingham")
for (city in east_midlands_cities){
  cat(city, "\n")
}
```

```
## Derby 
## Leicester 
## Lincoln 
## Nottingham
```

It is common practice to create a vector of integers on the spot (e.g., using the `:` operator) to execute a certain sequence of steps a pre-defined number of times.


```r
for (iterator in 1:3) {
  cat("Exectuion number", iterator, ":\n")
  cat("    Step1: Hi!\n")
  cat("    Step2: How is it going?\n")
}
```

```
## Exectuion number 1 :
##     Step1: Hi!
##     Step2: How is it going?
## Exectuion number 2 :
##     Step1: Hi!
##     Step2: How is it going?
## Exectuion number 3 :
##     Step1: Hi!
##     Step2: How is it going?
```


## Exercise 114.1

**Question 114.1.1:** Use the modulo operator `%%` to create a conditional statement that prints `"Even"` if a number is even and `"Odd"` if a number is odd.

**Question 114.1.2:** Encapsulate the conditional statement written for *Question 114.1.1* into a `for` loop that executes the conditional statement for all numbers from 1 to 10.

**Question 114.1.3:** Encapsulate the conditional statement written for *Question 114.1.1* into a `for` loop that prints the name of cities in odd positions (i.e., first, third, fifth) in the vector `c("Birmingham", "Derby", "Leicester", "Lincoln", "Nottingham", "Wolverhampton")`.

**Question 114.1.4:** Write the code necessary to print the name of the cities in the vector `c("Birmingham", "Derby", "Leicester", "Lincoln", "Nottingham", "Wolverhampton")` as many times as their position in the vector (i.e., once for the first city, two times for the second, and so on and so forth).


<!--
## Debugging

The term *bug* is commonly used in computer science to refer to an error, failure, or fault in a system. The term ``debugging'' in programming thus refers to the procedure of searching for mistakes in the code. This procedure can be simply done by re-reading and revising the written code, but R provides a useful command called `debug`, that allows you to follow the execution of the code, and more easily identify mistakes.

Try debugging the function - since it is working properly,  you won't (hopefully!) find any errors,  but that will demonstrate the debug facility, by typing `debug(cube_root)` in the RStudio *Console*.  That tells R that you want to run `cube_root` in debug mode.  Next enter `cube_root(-50)` in the RStudio *Console* and see how repeatedly pressing the return key steps you through the function.  Note particularly what happens at the `if` statement.  

At any stage in the process, you can type an R expression to check its value.  For instance, when you get to the `if` statement, you can enter `input_value > 0` in the RStudio *Console* to see whether it is true or false.  Checking the value of expressions at various points when stepping through the code is a good way of identifying potential bugs or glitches in your code.  Try running through the code for a few other cube root calculations,  by replacing -50 above with different numbers,  to get used to using the debugging facility.  

When you are done, enter `undebug(cube_root)`. That tells R that you are ready to return `cube_root` to running in normal mode.  For further details about the debugger,  enter `help(debug)` in the RStudio *Console*.
-->

<!--
## Exercise 6.2

Create a new R script as part of *Project_06*, named `Data_Wrangling_with_Functions.R`. Copy from the script `Data_Wrangling_Example.R` created in *Project_03* the first part that included loading both datasets, the part that created the tibble `leicester_IMD2015_decile_wide` and the part that left-joined it with `leicester_2011OAC` to create `leicester_2011OAC_IMD2015`.

Add the following snippet of code that uses the `pull` from the `dplyr` library to extract the column `supgrpname` from `leicester_2011OAC` as a vector, and the function `unique` to extract the unique values from the vector. That effectively creates the vector `leicester_2011OAC_supergroups` listing all the names of the supergroups.


```r
leicester_2011OAC_supergroups <- leicester_2011OAC %>%
  pull(supgrpname) %>%
  unique()
```

Extend the code in the script `Data_Wrangling_with_Functions.R` to include the code necessary to solve the questions below -- which as you might notice are a variation on the questions seen in Practical 3.

**Question 6.2.1**: Write a piece of code that loops over the supergroups names in `leicester_2011OAC_supergroups`, and for each one of those generates a table showing the percentage of EU citizens over total population, calculated grouping OAs by the related decile of the Index of Multiple Deprivations. **Tip:** use the `print` function at the end of the pipe that generates the table to print each table.

**Question 6.2.2**: Write a piece of code that loops over the supergroups names in `leicester_2011OAC_supergroups`, and for each one of those calculates the overall percentage of EU citizens over total population, and if that percentage is over 5%, then it prints the name of the supergroup. **Tip:** use `pull` at the end of the pipe to extract the calculated percentage.

**Question 6.2.3**: Write a functions named `median_index` with one input parameter `vector_of_numbers` as a numeric vector, implementing the index shown below where $v$ is `vector_of_numbers` and $index$ is the output value of the function. The index tends to `-1` when the median is close to the minimum, and it tends to `1` when the median is close to the maximum. Write a piece of code that extracts a colum of your choice from the `leicester_2011OAC_IMD2015` dataset as a vector and apply the index.

$$index = \frac{median(v)-min(v)}{max(v)-min(v)} - \frac{max(v)-median(v)}{max(v)-min(v)}$$

**Question 6.2.4**: If implemented carelessly, the index above can encounter a problem when all values are the same. In that case, $max(v)-min(v)$ is zero and thus a division by zero might return a `NaN` value. If you haven't done so yet, edit the function to take that case into account and simply return the value `0` in that case. Furthermore, include a check to verify that the input is a numeric vector.


## Solutions



### Exercise 6.1

**Question 6.1.3**: Write a function with two parameters, a vector of numbers and a vector of characters (text). The function should check that the input has the correct data type. If all the numbers in the first vector are larger than zero, return the elements of the second vector from the first to the length of the first vector. 


```r
function_question_1_3 <- function (in_numbers, in_text) {
  if (is.numeric(in_numbers) & is.character(in_text)) {
    if (all(in_numbers > 0)) {
      return(in_text[1:length(in_numbers)])
    } else {
      cat("Not all numbers are greater than zero")
    }
  }else{
      cat("Input error")
  }
}

# Examples
function_question_1_3(c(2, 3), c("Derby", "Leicester", "Lincoln", "Nottingham") )
function_question_1_3(c(3, -1, 0), c("Derby", "Leicester", "Lincoln", "Nottingham") )
```

### Exercise 6.2

A full R Script is available in the Exercises folder (`docs/exercises`) of the repository (`111_X_Data_Wrangling_with_Functions.R`). Upload the prepared script to your *Practical_111* project folder, click on the uploaded file to open it in a new editor tab and compare it to your script.
-->


---

<small>by [Stefano De Sabbata](https://sdesabbata.github.io/) -- text licensed under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), contains public sector information licensed under the [Open Government Licence v3.0](http://www.nationalarchives.gov.uk/doc/open-government-licence), code licensed under the [GNU GPL v3.0](https://www.gnu.org/licenses/gpl-3.0.html).</small>