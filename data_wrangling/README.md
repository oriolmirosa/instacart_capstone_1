## Data Wrangling

The data used for this project was released by [Instacart](https://www.instacart.com) in May 2017, and it can be found at the following url:
https://www.instacart.com/datasets/grocery-shopping-2017

The data consists of seven files:

* aisles.csv
* departments.csv
* order_products__prior.csv
* order_products__train.csv
* orders.csv
* processed.csv
* products.csv

Instacart also provides a data dictionary:
https://gist.github.com/jeremystan/c3b39d947d9b88b3ccff3147dbcf6c6b

As the dictionary states, the dataset contains over 32 million rows, each referring to a product in a given order for a particular customer.

These are the data wrangling steps that I undertook before analyzing the data (the whole process with code can be found in [this notebook](https://github.com/oriolmirosa/instacart_capstone_1/data_wrangling/data_wrangling.ipynb)):

1. Loading and merging data

First, I read the seven cvs files into pandas dataframes separately, and then I proceed to merge them (in one case, append them vertically) one by one using the appropriate id columns.

2. Selecting relevant features

Next, I drop all the variables that won't be necessary for the analysis. These include the id columns that were used to merge the dataframes, as well as the variables referring to individual products and their characteristics. The reason for dropping the products is that there are over 50,000 of them, which would make clustering rather difficult. Since there is information on departments and aisles for each product (with over 130 of the latter), we already have enough variability for the task at hand without making the analysis intractable.

In this step, I also make the `user_id` feature the index of the dataframe.

3. Dealing with null values

Checking for null values shows that there are 2,078,068 NaNs, all in the `days_since_prior_order` feature. Further inspection, however, shows that these NaNs actually refer to the first orders for each user, which of course, being the first, have no prior order to count days from. However, I believe that this is still valuable information: is this a product that was bought in a prior order x days ago, or is this the first purchase? I therefore give a valid value to the NaNs in this column.

Although the dtypes of all other numeric variables are integers, in the case of `days_since_prior_order` they are floats in order to accomodate the presence of NaNs (this is a limitation of pandas). However, the transformation with a new `first_order` value will turn this into a categorical feature. In order to do the changes, I first assign the value -1 to all the NaNs in the column, and then make it an integer variable. This makes all the numbers integers, which is consistent with the other columns and make sense given that they only count numbers of days. Then I replace all the -1 values with `first_order`, which turns make this an object variable.

4. Transform features to category data types

At this point, I have two integer and three object variables in the dataframe (not counting the integer index for the user ids), and given that there are over 32 million rows, the dataframe is over 1.5GB. This makes some computations difficult to do in memory, particularly in some hardware configurations. The situation can be ameliorated by transforming the object variables into categorical features. Yet the transformation also makes sense for the two integer variables. For one of them, the integers refer to days of the week, and for the other to hours of the day, so they can be thought of as (ordered) categorical features, particularly in the first case.

Given this, all variables are transformed to categorical. Once this is done, the size of the dataset decreases to around 450MB, which is much more manageable.

5. Extending the dataframe to have a row for each user

One of the main reasons to make the variables categorical is that it makes it possible to use pandas' `get_dummies` function. This creates new columns, one for each unique value in a given categorical feature, which are filled by 1s or 0s depending on whether the category encoded in that column is present in the original variable for that user or not. The issue here is that the goal of the project is to run cluster analysis to find groups of consumers according to their shopping patterns, and this requires reducing all the observations for each user (there is one for each product shopped in each order) into a single row. For some features (such as day of the week or days since prior order) it would be possible to assign numeric values to each category and then calculate the average for each individual. Yet this would mean that we lose all the information encoded in the distribution of categories for each column and each user. In order to keep this information, I expand each feature into dummy variables, then add the number of 1s in each column for each individual, and finally calculate the proportion that those values represent for the group of features for the user. For example, if a user has a number of rows (i.e., of products bought in a given order) that show that the purchase took place 3 times on a Monday, 5 times on a Friday, and 2 times on a Saturday, the values for all the other days for that individual would be 0, the value for Monday would be 0.3 (3 / 10), the value for Friday 0.5 (5 / 10), and the value for Saturday 0.2.

I do these calculations, then, for each group of variables that have been 'dummified', such as Monday to Sunday for days of the week, or 0 to 23 for hours of the day. Given that there are over 130 categories in the `aisles` feature, the operations for this group are done separately in order to prevent memory errors, and the final dataframes are horizontally concatenated at the end.

The resulting dataframe has 206,209 rows, one for each customer, and 218 features.