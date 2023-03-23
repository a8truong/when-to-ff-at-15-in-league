# when-to-ff-at-15-in-league
Final Project for DSC80

## Framing the Problem

We want to know whether or not surrendering 15 minutes into a league match is the best course of action. To determine this, we are performing binary classification. The response variable is whether or not the result is a win. We chose this because it was integral to solving our prediction problem: if we can predict whether or not we will win based on the stats at the 15 minute mark, we can determine whether or not surrendering is the better option. At the time of prediction (15 minutes into the match), we would have information on the stats for both sides in the match: gold, xp, creep score, kills, assists, as well as the team composition. We can use these statistics to determine the result of the match. We chose to evaluate our model on accuracy over other suitable metrics because we care about how well our model performs on both positive and negative data, rather than focusing on recall or precision which emphasize performance on positive data. Accuracy is also arguably the most intuitive metric making it easiest to explain to other people.

## Baseline Model

For our baseline model, we chose to do DecisionTree classification with a max depth of 10 (using no max depth lead to severe overfitting, so we chose 10 arbitrarily, we will tune max depth properly in the final model). For our features, we chose all of the statistics available to us at the 15-minute mark, minus the features that were linear combinations of others: gold, xp, cs, gold diff (difference between gold and opposing team’s gold), xp diff (difference between xp and opposing team’s xp), cs diff (difference between creep score and opposing team’s creep score), kills, assists, opposing kills, opposing assists. All of these features are quantitative (10 quantitative features). We believe that these features improved our model’s performance because they give us information that determines which side is being favored at the 15-minute mark, which could potentially reflect how the rest of the match will turn out. 

Our resulting model had a training accuracy of 79.0% and a testing accuracy of 72.4%. We believe that while our current model is not the best, it functions decently enough for a model predicting the outcome of a video game match (considering how volatile they can be), with a 72.4% accuracy on unseen data. 

## Final Model

To set up the Pipeline for our final model, we engineered a few new features. First, we transformed our gold data by applying the square root function to it because when we plotted the data, it was sigmoidal but emphasized after the pivot point, so applying the square root function would mostly linearize the data, which should help the model interpret the relationship between gold and result more easily. For our next features, we thought that team composition might have an influence on the result of the match, due to some champions “scaling” differently and therefore being stronger at certain points in the match, so we decided to one hot encode the champions in each role. 

For our model, we decided to go with RandomForestClassifier since it should be more accurate than a single tree (what we used for our baseline model). To determine our hyperparameters, we used grid search with a selection of hyperparameters for criterion, max depth, and min samples split. We ended up with the criterion ‘gini’, a max depth of 1, and a min samples split of 2. 

The resulting accuracy for our final model was 74.4%, which is better than our baseline model’s accuracy by 72.4%. 

## Fairness Analysis

To determine the fairness of our model, we need to evaluate whether our model performs differently for certain groups. We decided to compare the accuracy of our model on the red team vs the blue team. 

Our null hypothesis was that our model is fair and its accuracy for the red and blue team are similar, if not the same. Our alternative hypothesis was that our model is unfair and is less accurate for the blue team. 

Our test statistic was the signed difference of means at the significance level of 0.05. We ran a permutation test and got a resulting p-value of 0.38, which is above the significance level of 0.05, so we fail to reject the null hypothesis that our model is fair.
