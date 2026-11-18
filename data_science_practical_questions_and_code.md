# Data Science Practical Record

## Program 1

**Aim:** Import and Export Data into and from CSV file in R.

``` r
students <- data.frame(
    ID = c(1, 2, 3),
    Name = c("John", "Alice", "Bob"),
    Marks = c(85, 90, 78)
)

write.csv(students, "output.csv", row.names = FALSE)

df <- read.csv("output.csv", header = TRUE)

print(df)
```

## Program 2

**Aim:** Import and Export Data into and from Text file in R.

``` r
students <- data.frame(
    ID = c(1, 2, 3),
    Name = c("John", "Alice", "Bob"),
    Marks = c(85, 90, 78)
)

write.table(students, "students.txt")

students <- read.table("students.txt", header = TRUE)

print(students)
```

## Program 3

**Aim:** Import and Export Data into and from Excel file in R.

``` r
install.packages("stringi")
install.packages("readxl")
install.packages("openxlsx")

library(readxl)
library(openxlsx)

students <- data.frame(
    ID = c(1, 2, 3),
    Name = c("John", "Alice", "Bob"),
    Marks = c(85, 90, 78)
)

write.xlsx(students, "people.xlsx")

data <- as.data.frame(read_excel("people.xlsx"))

print(data)
```

## Program 4

**Aim:** Implement exception handling -- Progress and Timing.

``` r
start <- Sys.time()

tryCatch({
    print("Program started")

    for(i in 1:5) {
        print(paste("Progress:", i, "of 5"))
        Sys.sleep(1)
    }

    print("Program completed")
}, error = function(e) {
    print(paste("Error:", e$message))
})

end <- Sys.time()

print(paste("Time taken:", end - start))
```

## Program 5

**Aim:** Implement Data Visualization in R using scatter plot.

``` r
x <- c(10, 20, 30, 40, 50)

y <- c(15, 25, 28, 43, 55)

plot(x, y,
    main = "Scatter Plot Example",
    xlab = "X Values",
    ylab = "Y Values",
    col = "blue",
    pch = 16,
    cex = 1.2
)
```

## Program 6

**Aim:** Implement Data Visualization in R using box plot and whiskers
plot.

``` r
math <- c(60, 65, 70, 75, 80)
science <- c(68, 72, 74, 76, 85)
english <- c(55, 60, 65, 70, 75)

scores <- list(Math = math, Science = science, English = english)

boxplot(scores,
    main = "Scores by Subject",
    col = c("red", "green", "blue"),
    ylab = "Marks"
)
```

## Program 7

**Aim:** Implement Data Visualization in R using bar chart.

``` r
values <- c(25, 40, 35, 50)
categories <- c("A", "B", "C", "D")
colors <- c("red", "blue", "green", "orange")

barplot(values,
    names.arg = categories,
    col = colors,
    main = "Category-wise Values",
    xlab = "Categories",
    ylab = "Values",
    border = "black"
)
```

## Program 8

**Aim:** Implement Data Visualization in R using Histogram.

``` r
scores <- c(
    45, 50, 55, 60, 62, 65, 67, 70, 72, 75,
    78, 80, 82, 85, 88, 90, 92, 95, 68, 74,
    58, 63, 77, 81, 69, 73, 84, 87, 91, 96
)

hist(scores,
    main = "Distribution of Student Exam Scores",
    xlab = "Marks",
    ylab = "Number of Students",
    col = "lightblue",
    border = "black"
)
```

## Program 9

**Aim:** Implementation of Simple Linear Regression in R.

``` r
x <- c(151, 174, 138, 186, 128, 136, 179, 163, 152, 131)
y <- c(63, 81, 56, 91, 47, 57, 76, 72, 62, 48)

plot(y, x,
    col = "blue",
    main = "Height & Weight Regression",
    cex = 1.3,
    pch = 16,
    xlab = "Weight in kg",
    ylab = "Height in cm"
)

abline(lm(x ~ y))
```

## Program 10

**Aim:** Implementation of Multiple Linear Regression in R.

``` r
data <- mtcars[, c("mpg", "disp", "hp", "wt")]

print(head(data))

model <- lm(mpg ~ disp + hp + wt, data = data)

print(model)

summary(model)

new_car <- data.frame(
    disp = 221,
    hp = 102,
    wt = 2.91
)

predict(model, newdata = new_car)
```

## Program 11

**Aim:** Implement Descriptive Statistics in R: Central Tendency (Mean
and Median), Variability (Range and Variance).

``` r
x <- c(10, 2, 30, 4, 5, 6, 70, 8, 9, 10)

mean_value <- mean(x)

median_value <- median(x)

range_value <- range(x)

variance_value <- var(x)

print(mean_value)

print(median_value)

print(range_value)

print(variance_value)
```
