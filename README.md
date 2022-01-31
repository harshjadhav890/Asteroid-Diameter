<h1 align="center">☄️Measure Me</h1>
<h4 align="center"><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/Oumuamua.jpg?raw=true"  width="400" height="250"/>
<h2 align="center">ML models for Measuring Asteroids!!!</h2>

# Notebook includes:
- **Preprocessed Data**
- **Interactive Seaborn Widget 📊**  *(Download Notebook to access)*
- **ML models**
- **Comparison of models**

# Models Included:
- **Linear Regression**
- **Lasso**
- **Ridge**
- **Support Vector Regressor**
- **KNN**
- **Decision Tree**
- **Random Forest** (With Hyperparameter Optimization)
- **XGBoost**

  
<h1 align="center">The Process:</h1>
  
<h2>Data Processing</h2>
The dataset given to us was massive. Unlike anything, I've worked with before. It's big, not just in terms of data but also the number of variables, we had to train the model using. So I knew some serious work had to be done. I started by removing some unnecessary columns.</br></br>
  
- extent, GM, IR, BV, UB, G -> were removed because a majority of the values in these columns were missing. 
- spec_B, spec_T -> contained 34 classes. I wish I could encode at least the most frequent ones, but chose not to tamper with the data. It would have resulted in addition of multiple columns.
- neo, pha, class, condition_code -> These factors are not related to diameter.
  
Next, I extracted all the rows in which the values for diameter and rot_per were not missing. Some values for diameter were non-numeric and hence were cleaned. Out of the columns left, very few had any missing values in them so I interpolated the dataset to fill in median values. The values were rounded to 5 decimal places as they were too big to be classified as float. Then I converted the whole dataset to float, to remove any left out non-numeric values.
  
Fitted some models to evaluate my work so far. The results were terrible as expected. Later on, when I was tinkering with some seaborn plots, I realized that I had totally forgotten to remove the outliers. What I did after this helped a great deal . . . . . .
  
<h2>Data Analysis</h2>
I researched and found out some bad methods of detecting outliers on the internet. But I stumbled upon something called pairplot. Upon further research and playing with code I learned that you can make widgets that you can interact with in the jupyter notebook itself. Some to's and fro's between coding and then upadting the code, got me to what you can access right now. Detecting and removing outliers had never been easier!!!!
<br />
<br />
<div>
  
|With Outliers|Without Outliers|
|----|----|
|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/before_chart.png?raw=true"  width="330" height="330"/>|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/after_chart.png?raw=true"  width="330" height="330"/>|
  
</div>
  
- We can easily plot variables against each other, detect outliers and remove them with a single line of code (eg. df = df[df.a < 20])
   
<div>
<h3>Seaborn Widget (Download Notebook to access)</h3>
<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/Seaborn_widget.png" height="400" width="850" />
</div>
<br />
<br />
Removing outliers gave me a significant boost in model performance. My previous terrible results were now.... less terrible. For context, I was getting a mean square error of about 300 with linear regression previously. It was brought down to 217. Stratified sampling brought it down to.... 175.
  
[Learn about Stratified Sampling](https://www.youtube.com/watch?v=sYRUYJYOpG0)
  
<h2>Stratifying and Splitting the data</h2>
  An extra column called diameter_grp was created. This was a class column which contained values from 1 to 5 signifying how big the diameter of the asteroid was.
  The process is called Stratified Sampling and is done so that we can maintain the same proportion for each group(1-5) in both, the original dataframe and the splitted dataframe. The graph shows the number of values in each group:
<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/Stratified.png"/></br></br>
<div>
  Next, I performed stratified sampling using StratifiedShuffleSplit with respect to 'diameter_grp'. Lets check the proportions of each group in the original and splitted dataset. They look almost equal. This has a positive effect on our models. </br></br>
  
|Original proportions|Training proportions|
|----|----|
|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/orig_prop.png" width="400" height="200"/>|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/train_prop.png" width="400" height="200"/>|
</br>
</br>
Previous Best:</br> Best Model: Random Forest. </br>MSE: 14.0625 </br>RMSE: 3.7500 </br>MAE: 1.2805 </br></br>
Now Best (After sampling):</br> Best Model: XGBoost. </br>MSE: 9.8679</br>RMSE: 3.1413 </br>MAE: 1.2129 </br></br>

</div>
  
<h2>Trying out Models</h2>
  <h3>Linear, Lasso, Ridge</h3>
Eight models have been implemented in the notebook. The first one being Linear Regression. It gives a bad result because the dataset is too complex and almost none of variables share a linear relation with diameter. So do Lasso and Ridge which work on the same principal. I also heard that scaling the data would give a better result when it comes to these algorithms. It turned out to be false in this case because scaling caused these 3 models to fail miserably.
  
  <h3>SVR, KNN</h3>
SVR performs the worst among all the models. It also ran slower than all the models I had implemented until then. KNN did a good job overall. Ran quickly and produced an error(MSE) of 164.1. 
  <h3>Decision Tree, RandomForest, XGBoost</h3>
  Decision tree gives great results. Setting the tree depth to 4 gives the least error and the lowest runtime. RandomForest beat Decision tree by a quite a bit. It was the reigning champion for a long time. I decided to run hyperparameter optimization on RandomForest but it produced no significant improvements.
  <div align="center">
<h3>Decision Tree</h3>
<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/decision_tree_viz.png" height="400" width="850" />
  <h4>H shared a significant relation with diameter. This is clearly reflected in the tree.</h4>

</div></br></br>
Later I found something called pruning in Decision tree to reduce overfitting. I tried running cost complexity pruning on my decision tree but it once again produced nothing significant. Nevertheless, it's a great way to reduce overfitting and might work wonders on another dataset.

Know more:
https://scikit-learn.org/stable/auto_examples/tree/plot_cost_complexity_pruning.html
</br>
  XGboost outperformed RandomForest, and is currently giving the lowest error. All these algorithms do not require data to be scaled.


</br>
<h2>Final Results</h2>
 The final day results were as follows.....</br>
 
- Table:
<div>
 
|Model|MSE|RMSE|MAE|
|----|----|----|----|
|Linear Regression|175.66|13.25|8.43|
|Lasso|194.45|13.94|8.79|
|Ridge|175.25|13.23|8.41|
|SVR|209.44|14.47|6.34|
|KNN|190.45|13.80|7.48|
|Decision Tree|30.65|5.53|2.99|
|Random Forest|10.40|3.22|1.17|
|Random Forest (Tuned)|17.28|4.15|2.29|
|XGBoost|9.86|3.14|1.21|
 
  
</div>

- Chart

|Day 3|Final Day|
|----|----|
|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/Before_comparison.png" height="400" width="400" />|<h4><img src="https://github.com/harshjadhav890/Machine-Learning/blob/main/Final_comparison.png" width="400" height="400"/>|

Best Model: XGBoost. </br>MSE: 9.8679</br>RMSE: 3.1413 </br>MAE: 1.2129 </br></br>

# Libraries used:
- **Seaborn**
- **xgboost**
- **matplotlib**
- **sklearn**
- **ipywidgets**
- **numpy**
- **pandas** 

  
  </br></br>
<h1>☄🪐🌖🌠☄👽🚀🌟</h1>

  

  

