# Categorical Feature Scaling

------

# What is Feature Scaling?

- Feature scaling is a method used to normalize the range of independent variables or features of data. In data processing, it is also known as data normalization and is generally performed during the data preprocessing step. Just to give you an example — if you have multiple independent variables like age, salary, and height; With their range as (18–100 Years), (25,000–75,000 Euros), and (1–2 Meters) respectively, feature scaling would help them all to be in the same range, for example- centered around 0 or in the range (0,1) depending on the scaling technique.

# Nominal and Ordinal Variables

* **Numerical data**, as its name suggests, involves features that are only composed of numbers, such as integers or floating-point values.

* **Categorical data** are variables that contain label values rather than numeric values. Categorical variables are often called nominal. Some examples include:
  * A “*pet*” variable with the values: “*dog*” and “*cat*“.
  * A “*color*” variable with the values: “*red*“, “*green*“, and “*blue*“.
  * A “*place*” variable with the values: “*first*“, “*second*“, and “*third*“.


- Each value represents a different category.

  Some categories may have a natural relationship to each other, such as a natural ordering.

  The “*place*” variable above does have a natural ordering of values. This type of categorical variable is called an ordinal variable because the values can be ordered or ranked.

  A numerical variable can be converted to an ordinal variable by dividing the range of the numerical variable into bins and assigning values to each bin. For example, a numerical variable between 1 and 10 can be divided into an ordinal variable with 5 labels with an ordinal relationship: 1-2, 3-4, 5-6, 7-8, 9-10. This is called discretization.

  - **Nominal Variable** (*Categorical*). Variable comprises a finite set of discrete values with no relationship between values.
  - **Ordinal Variable**. Variable comprises a finite set of discrete values with a ranked ordering between values.

  Some algorithms can work with categorical data directly.

  For example, a decision tree can be learned directly from categorical data with no data transform required (this depends on the specific implementation).

  Many machine learning algorithms cannot operate on label data directly. They require all input variables and output variables to be numeric.

  In general, this is mostly a constraint of the efficient implementation of machine learning algorithms rather than hard limitations on the algorithms themselves.

  Some implementations of machine learning algorithms require all data to be numerical. For example, scikit-learn has this requirement.

  This means that categorical data must be converted to a numerical form. If the categorical variable is an output variable, you may also want to convert predictions by the model back into a categorical form in order to present them or use them in some application.

 

------

# Encoding Categorical Data

* There are three common approaches for converting ordinal and categorical variables to numerical values. They are:
  * *Ordinal Encoding*
  * *One-Hot Encoding*
  * *Dummy Variable Encoding*

------



# Ordinal Encoding



## Ordinal Encoder

* In ordinal encoding, each unique category value is assigned an integer value. For example, “*red*” is $1$, “*green*” is $2$, and “*blue*” is $3$.

  For some variables, an ordinal encoding may be enough. The integer values have a natural ordered relationship between each other and machine learning algorithms may be able to understand and harness this relationship.

  It is a natural encoding for ordinal variables. For categorical variables, **it imposes an ordinal relationship where no such relationship may exist. This can cause problems and a one-hot encoding may be used instead.**

* This ordinal encoding transform is available in the scikit-learn Python machine learning library via the [OrdinalEncoder class](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html).

  By default, it will assign integers to labels in the order that is observed in the data. If a specific order is desired, it can be specified via the “*categories*” argument as a list with the rank order of all expected labels.

------



## Label Encoder

* If a **categorical target variable** needs to be encoded for a classification predictive modeling problem, then the [LabelEncoder class](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html) can be used. It does the same thing as the *OrdinalEncoder*, although it expects a one-dimensional input for the single target variable.
* `LabelEncoder()` is desinged to be used for the target variable (y). That is the reason for getting the positional argument error, when `columnTransformer()` tries to feed `X, y=None, fit_params={}`.

------



## Label Binarizer

* Several regression and binary classification algorithms are available in scikit-learn. A simple way to extend these algorithms to the multi-class classification case is to use the so-called **one-vs-all scheme**.

------



## Multi Label Binarizer

* Transform between iterable of iterables and a multilabel format. Although a list of sets or tuples is a very intuitive format for **multilabel data**

------



# One-Hot Encoding

* For categorical variables **where no ordinal relationship exists**, the integer encoding may not be enough, at best, or misleading to the model at worst.

* Forcing an ordinal relationship via an ordinal encoding and allowing the model to assume a natural ordering between categories may result in poor performance or unexpected results (predictions halfway between categories).

  In this case, a one-hot encoding can be applied to the ordinal representation. This is where the integer encoded variable is removed and one new binary variable is added for each unique integer value in the variable.

* This one-hot encoding transform is available in the scikit-learn Python machine learning library via the [OneHotEncoder class](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html).

------



# Dummy Variable Encoding

* The one-hot encoding creates one binary variable for each category.

  The problem is that this representation includes redundancy. For example, if we know that [1, 0, 0] represents “*blue*” and [0, 1, 0] represents “*green*” we don’t need another binary variable to represent “*red*“, instead we could use 0 values for both “*blue*” and “*green*” alone, e.g. [0, 0].

  This is called a dummy variable encoding, and always represents C categories with C-1 binary variables.

* In addition to being slightly less redundant, a dummy variable representation is required for some models.

  ==For example==, in the case of a linear regression model (and other regression models that have a bias term), a one hot encoding will case the matrix of input data to become singular, meaning it cannot be inverted and the linear regression coefficients cannot be calculated using [linear algebra](https://machinelearningmastery.com/linear-algebra-machine-learning-7-day-mini-course/). For these types of models a dummy variable encoding must be used instead.

* We can use the *OneHotEncoder* class to implement a dummy encoding as well as a one hot encoding.

  The “*drop*” argument can be set to indicate which category will be come the one that is assigned all zero values, called the “*baseline*“. We can set this to “*first*” so that the first category is used. When the labels are sorted alphabetically, the first “blue” label will be the first and will become the baseline.

------

