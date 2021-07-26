# item_sales_predictions
This project consists of cleaning a dataset and then performing some exploratory and explanatory data analysis, followed by the application of machine learning algorithms to predict the sales for an item.

## Data Dictionary
- **Item_Identifier**: Unique product ID
- **Item_Weight**:  Weight of product
- **Item_Fat_Content**: Whether the product is low or regular in fat content
-  **Item_Visibility**: The percentage of total display area of all products in a store allocated to the particular product
-  **Item_Type**: The category to which the product belongs
-  **Item_MRP**: Maximum Retail Price (list price) of the product
-  **Outlet_Identifier**: Unique store ID
-  **Outlet_Establishment_Year**: The year in which the store was established
-  **Outlet_Size**: The size of the store in terms of ground area covered
-  **Outlet_Location_Type**: The type of area in which the store is located
-  **Outlet_Type**: Whether the outlet is a grocery store or some sort of supermarket
-  **Item_Outlet_Sales**: Sales of the product in the particular store. This will be the target variable to be predicted.

## Methods
### Cleaning the Data
Two columns, Item_Weight and Outlet_Size had missing values. Both had a large number of missing values, so the rows were not dropped; what instead was done was that values were imputed by using an interpolation method for both columns. <br>
For the Outlet_Size column, the data was sorted in order of price before backfilling missing values because there was a clear relationship between average item_Outlet_Sales and the Outlet_Type. See Figure 1.

**Figure 1**
![image](https://user-images.githubusercontent.com/67303401/126942231-f902b8e0-622f-4417-a00a-de8c696074e5.png)


### Exploring the Data
When exploring the data, I was attempting to find where the strongest relationships were. To do this, one method that was used was the separation of the data into two subsections, one with a high Item_Outlet_Sales and another with a low Item_Outlet_Sales, these filters were then used to try to find different dristributions within other features when looking at rows with low sales versus high sales. For examples, see Figure 2 and Figure 3; in these images, the distributions for Item_Visibility were graphed and the distribution changes slightly, skewing to the left with higher sales.

**Figure 2**
![image](https://user-images.githubusercontent.com/67303401/126947065-1be3955d-b20d-4cf4-82a1-2d42b8a1a6b7.png)


**Figure 3**
![image](https://user-images.githubusercontent.com/67303401/126947137-a7d1a636-a135-4d8c-a5ac-ee2f952818d6.png)


## Machine Learning
### Setting Up the Data
Firstly, columns in the data set that were discrete had to be taken care of and turned into some sort of numerical data in order to be interpretable by the models. In the case of binary columns, such as Item_Fat_Content, one value was chosen to be 1 and the other 0, effectively transforming that column into a numerical form. In the case of columns with more than two types of values, I chose to use the method of One Hot Encoding. Using this method, the different categories of variables are translated into their own binary columns.<br><br>
Another thing done in order to prepare the data for machine learning, was splitting the data into a training data set and a testing data set. This split is done to avoid endorsing overfitting of the data; the split was 75-25. 
Next, preparing for the KNN model, the data was fit and transformed using a scaler. Later, this method was reversed when creating the model of Random Forest; this transformation of the data would only hurt random forest performance.

### Models
Two different types of models were used, K-Nearest-Neighbors for regression and Random Forest for regression. In both cases for the models, the number of neighbors/estimators was optimized by testing many different values in a range and finding what value gives the lowest Root Mean Squared Error (RMSE). The two models, KNN and random forests, had overall similar RMSEs but the random forest beat out KNN by a difference in RMSE of around 40. <br><br>

Due to the importances of the features, some recommendations to distributers of certain items are:
- Grocery Store types usually sell a lower amount of items, which leads to them having lower Item_Outlet_Sales in general, therefore if your goal is to increase the total amount of Item_Outlet_Sales, Grocery Stores are not where your items should be sold (or the item should be sold at more locations).
- Opposite to above, Supermarket Type3 has a relationship of **increasing** Item_Outlet_Sales in general. A recommendation if the goal is to maximize Item_Outlet_Sales is to sell items in a Supermarket Type3.
- Another feature with a positive correlation to Item_Outlet_Sales is Item_MRP, this might suggest that increasing Item_MRP might lead to higher item sales; this makes some sense because it does not mean that the actual price of the item will increase, but rather that the maximum price that the outlet can charge will increase, which can lead to a more flexible choosing of the price, which might lead to a better estimation of the equilibrium price.
