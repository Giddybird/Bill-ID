# Bill-ID

# 1. Overview 

Every two years a new congress is elected, during that time any represenative can present a bill.  Few bills become laws.  From the congress in question (the 116th), only 46 bills became law of the nearly 5,600 that were proposed.  The purpose of this model is to attempt to predict the fate of a bill using its text as a  primary predictor as well as the number of other details as secondary predictors.  The purpose is to give anyone interested in anticpating a likely outcome of a bill to predict it with a reasonable degree of accuracy.  This could be a representative, a lobbyist, or any of the thousands of adminstrators involved in the law making process.  


# 2. Business Problem

Most bills dont become laws.    Since 1973, 342,545 bills have been proposed of those, 3% became laws and only 4% saw a vote.  Most bills die in committe.  Meaning they never make it to the floor for consideration after being submitted by a representative.  Writing and submitting a proposal is a lengthy process taking much time and effort, and bills can take years to be ammended.  Knowing if a proposal is likely to pass influences the entire nation anyone in that nation who this would effected by a bill or who would be interested in influencing the fate.   A tool capible of predicting the outcome of a bill in advance would be invaluable to anyone affected by the bill or who would be interested in influencing a bills outcome.

Consider beyond the Washington context, a bill if passed becomes a law in 10 days.  If a bussiness would be effected by a law that is likely to pass, getting head of the curve before that law passes would be a competitve advantage for a company.  Consider a hypthoetical bill, that would make use of a certian chemcial illegal, if a company could anticipate the passing of this law, it could adapt and be prepared before the law passes and be ahead of the competition.

The utlimate goal of this project is to create such system.  One that can predict the outcome of a bill passing given it’s topic, wording, and a  number other features.  

IMPORTANT HERE QUANTIFY THE COSTS OF PASSING A LAW MONEY TIME AND HOW THIS COULD REDUCE THEM

Sources:

https://www.govtrack.us/congress/bills/statistics

https://www.congress.gov/help/learn-about-the-legislative-process/how-our-laws-are-made

# 3. Data Understanding
The data consists of information scrapped from the office of the clerk’s website.  clerk.house.gov and submitted as a kaggle competition.  The focus of this analysis will be two CSV files.  One consisting of each bill (its committee assignments, cosponsors, and other details) and the second is a list of the elected representatives of the congress.

In keeping with the reality of dirty data.  The data set had only 5,600 proposals of which only 2400 had summaries included.   

Of those 2400 proposals only 1922 were bills and of those only 31 became laws.   

It was assumed this was complete data.  This is not the case.   Upon further investigation the actual data source appears to have 20,000 proposals for this session of congress, thus, the data sets used were incomplete.

Attempts to complete the dataset was proving difficult, the model and pipeline had been built on the incomplete data which would have a different format, and take time to replicate the scrapping format used in the kaggle dataset.  For the sake of time, it was decided to make a working prototype using the existing data and then complete the data at a later point.  The list of 2000 bills is enough to start training a model on.  However, it should be noted that 344 bills became laws during this session meaning our sample may be considered inaccurate of the “population” of bills.  Our data is roughly 10% if all the proposals of the 116th congress.  
It needs to be noted that this data did not include any bills that originated in the senate only house proposals.  

Within those there are two types of proposals, bills, and resolutions.  A bill, is a proposal to become a law.  A resolution is a statement of congressional sentiment.  Something that if agreed to by a majority, is noted as representing a majority opinion of the congress.  These latter proposals are useful political tools, and there are 3 types of resolutions.  However, each is either agreed to by the congress or remains as “introduced”.  Two are joint resolutions between both chambers, and the 3rd is a house specific resolution.  

There are numerous details within each proposal to consider.  What kind of proposal is it? Who sponsored it? Who cosponsored it, how many representatives cosponsored it? What where their party affiliations?  What committees was the bill they assigned to, what was the bill's topic? how many subcommittees were assigned the bill, was the bill introduced in the house or the senate first.   What state is the representative from?  What constituency concerns might they have to consider?  Who was the head of the committees the assigned to, what was their party affiliation?  Etc. etc.  All of this could serve as insightful and each would need to be created manually using the data.  

For the Sake of this project the following will be the categories used

- Proposal Summary Text
- Assigned Committee
- Bill Subject
- Policy Area
- Number of Cosponsors
- Ratio of Cosponsors

1. Most Proposals have a summary, a body of text about 60 to 90 words with some being as long as 6000 and others as short as 4.  This text is a summary of the bill.
2. Assigned Committees, each bill once introduced to the house is assigned to a committee by the speaker of the house, these committees are chaired by members of congress who can debate, amend, and deliberate on a bill while hearing from topic experts and the public on the issue.  Most bills die in committee, meaning taking which committee a bill is assigned could be predictive.  Bills can be assigned to numerous committees by its primary committee as well, to gain addition insight into the topic or even to slow the bill down in hopes of killing it.  This can be a political move to prevent pork barreling or a legitimate action to gain more insight, say a bill will influence both the enviroment and energy sectors.  However, knowing which committees a bill is assigned to can still be insightful especially if a bill only ever stays with one committee or is given to many.  
3. Bill Subject: Each bill can have numerous subjects asscoiated with them which can help with the placement, in this case it can help as well in identifying if proposals on certain subjects are more or less likely to pass
4. Policy Area: Similar to but uniquely different than subject is the policy area, another category each bill may have
5. A bill may be cosponsored by any number of other representatives the thinking is, that more cosponsors may indicate greater support and thus a higher likelihood of passing.  However, in practice this may also suggest that a number of representatives merely want to be associated with a bill they know won't pass, or are seeking to help pass a bill they know will have trouble passing, a high number of cosponsors may not indicate a higher likelihood of passing automatically unless taking into account with other factors.
6. Cosponsor Ratio: In light of the previous point, it was thought that the partisan ratio of cosponsors may be insightful as to the likelihood of a proposal passing, to this end a feature was created to explore this possibility, each proposal was given a ratio of the partisan affiliation of cosponsors for both parties.  EG. 56 Cosponsors from Party A, to 0 from Party B indicating a party proposal rather than a joint one.

##### Limitations

As mentioned, the data is incomplete, in that it only contains roughly a tenth of proposals before congress.  

One other vital limitation to note, this data is only proposals in the house.  Meaning it has no ability to predict or anticpate the dynamics of the Senate.  Which is half of the legislative body.

Additional levels of nuance exist.  But are beyond the current scope of the model’s ability to detect.  For example, the degree to which a representatives works to get cosponsorship or votes for a proposal is not directly related to the topic itself. The reputation of the bill sponsor or consponsor, matters as much as the bill itself.  A representative with a poor reputation, one who is doesn’t work with others, backs stabs, is belligerently inflexible what have you, may have more or less success in passing legislation.   Enemies will vote together if they think there is something in it for them, or an issue of sufficient importance at stake.  The complexity of congressional relationships, representative platforms, constituency concerns and various private concerns are all lost on this model.  

Creating a unique profile on even one representative and on their relationship with other representatives would an entire project in itself, let alone for each of the 500 representatives of the house.

This aspect will limit the model’s predictive ability on unseen data.  What a representative may feel compelled to support does not always align with their party, their personal platform or their constituency concerns, many others factors influence a representative. That are not noted directly or even indirectly here. 

# Methods

Feature creation was a common element in this project.  The raw data offers numerous possibilities to explore as relationships, however none of them exisit in a preusable format.  Things like, the number of cosponors, the partisasin split of cosponsors, and the frequency of specific words and phrases in the text all have to be manually created from the data. 


What the tree has to split on depends on the information given it.  In this case since text data is being used extensively, it will be the majority of the spits in the data.  There are two options both of which have merit.  The first is a TF-IDF, the second it a count vectorizer.  A TF-IDF, is a comparision of the frequency of a phrase in a document compared to it frequnecy in every document.  Where as the second is merely a count of frequency of a word in a document.  Both have merit.  It is important to understand the differences here.  A TFIDF returns a number representing a frequency among all documents the higher the number the more unique to this document that phrase is.  The count vectorizer simply returns how many time a given phrase occurs in each document.

Ex: TF-IDF for the phrase “Increase funding”: .34  Count Vectorizer:  “64” counts of phrase

This resulted in a highly dimensional model.  Containing over nearly 300,000 unique ngrams across every bill summary.  Each ngram serves as a possible split in the data.  Further because the target is a multiclass classification problem and is highly dimensional a decision tree is ideal for fitting.  

In this case. the tree will be looking at node and deciding what intial element provides the best split.  The best split being the clearest seperation between passing and not.  For example, if the phrase “increase funds” ocurring more that 15 times leads to a signficnat split of passing and not passing bills then it will be used as the determining factor of that split.  Those that contain the phrase more than the specified number of times will be moved to the left and those that do not will be moved to the right.   

Then at the two nodes resulting from this split new splits are considered using the same process.  It is important to note that this model uses “replacement” meaning if a previously used phrase provides the most meaningful split at a different point in the data then it will be used again but liekly with a different splitting condition.  For example, if at a latter point “increase funds” occuring less than 7 times provides the most meaningful split in the data again it will be used as the determinatiant of split of that node.  

This process repeats until the model has every element as perfectly sorted as its parameters allow.  A model can be required to have no less or no more than a certain number of element in each group resulting from each split (for example each split having to have least 6 elements in each resulting group).  The purpose in doing is is to attempt to prevent model from overfitting to the training data and the hope is making the model more able to generalize to unseen data.  




The number of possible permutations of a decision tree are almost staggering.  

Enter the random forest which create as many models as you tell try and then compare the outcomes of those models combining their insights together to create a better model.  


There is a further element that can be given to a random forest and that is a gridsearch.  A grid search is a series of paraments fed to a model to try.  Think of them as different settings to try when fitting the model.  In this context things that could be tired are how many nodes are used, how many need to be in each split etc, this settings of the model can effect the models outcome and so the grid search runs through each permutation given it and records the results.  This allows for numerous iterations of the model to be tested without having to manually define each one.

The last element is cross validation.  For each model a number of companion models using the same model criteria is trained on different subsets of that data.  This allows the weights to be tested against a spread of data.  Effectivley training each model on numberous subsets of the training data.  Conventional wisdom holds that 3 folds (or 1 model with 2 companion models) is the ideal number of folds to maximine interpretability on both training and actual test data.  


Putting all of this together we had a grid search doing 300 fits (100 varitions of each model with each have 2 companion models for cross validation making the total number of fits 300).  And with each random forest set to make 1000 trees that means in the end the model trained 300,000 decision trees.  From which results were taken.


# Results and Going Forward

![Final Comparsion](images/Dummy_Confusion_Matrix.png)

It is clear that the final model preforms the best on its testing data.  Even when training data is reintroduced.  However as mentioned, this model is only preforming so well because it is including the original testing data.  The reality is, the tree was overfitting to the data it was given and when it was introduced to unseen data it could only predcit 1% petty than a dummy model.  Meaning the patterns detected in training the model did not apply universally to unseen data.

In the end, after 300,000 iterations of decision tree models (300 random forests of 1000 trees each), The model was only _slightly_ better at predicting a given bill's outcome on unseen data than the dummy model.  Given that 84% of the bills die as being introduced, our model is only able to predict 85% of the cases of "unseen data".  There is clearly more room for growth.  As can be seen above, the model still is overfitting on training data.  While adjusting the depth of the model is an option that can be pursued, it is more likely that there is nuance this model is not yet picking up on.  For instance, the scale of the data being only 10% of the total bills, the realities of politics involving more than the bills text alone, and many other factors are present here that the model cannot pick up on.

It was possible that the wording of the bill represented the reality of the bill itself and thus would indicate if a representative would support it.  However, a representative's support is garnered from more than just the wording of a bill.  Consider concerns of relection, of exchanged support for issues of greater concern, personal constituency matters, and other concerns that would influence a bill's fate.  Further a representative may have an increased affinity for a bill after repeated exposure and debate, either from having their opinions incorperated, or changed by the debate itself.  On the other hand a representative could find their opinion only strengthened by debates on a topic.  This matter of individual representative perception and opinion is beyond the models ability to interpret and will remain beyond the model's ability to understand without extensive data on the represetnatives themselves.  

There is a reason political science, is a bachelor of _Arts_ atalmost every university, because there is an _art_ to politics that transcends a models ability to see.  Understanding the lay of a political landscape at a given time, how to maneuver through it to accomplish objectives, how to garner support and change opinions, all of this and more that constitutes the art of politicing.  The data for such a think is not available for the model, and so its realities are lost on a model looking and language analysis, and political party affilation alone.  In principal the democratic process should be as simple as analyising the wording of a bill to see if phrasing is sufficently agreed upon however in practice there is far more naunce to the realities of politics as most anyone could tell you.  


That being said, should continued work be desired, timing of the bill could be considered, as well as number of times ammended. It could be possible to attempt to identify representative sentiment through analysis of representative tweets and other social media posts. This would have some limitations, as not every representative uses twitter nor would they post every thought they have but more likely would use such tools to signal sentiment.  
Further factors to consider could be,  the timing of the bill, recent news stories, the number or related bills (say if competting proposals on the same topic are splitting resource or lack sufficent public interest, or neccesity to interest a politicin in its creation), the fates of those bills (say if a bill passes on a topic does that make any subsequant bills on the same topic more or less likely to pass or be worked on as much my interested represenatives, parties, or lobbyists).   

Thus any model produced would need more information to be of use to any interested party in predicting the fate of a bill.  The simply reality is the initial writing of a bill is only the beginning of a bill's fate, it goes through countless ammendments and must have extensive support to pass.  It is this support that determines the fate of a proposal.  This support which indeed is inpart based on the text of a bill clearly has more to its giving than text alone.

However, as this is incomplete data, the possibility will always remain, that perhaps with a the full 20,000 proposals a complete model could be made to identify more fully the outcomes of every text permuatation a given bill may have, in addition to the realities of the bills sponsor, cosponsors (each one hot encoded) and other factors

However, given the shear immensity of possible wording and the varity of topics, in addition to the nauances of politics it may well be beyond the models ability to ever determine without a level of information on representatives that every representative works very hard to avoid coming into being.


```
├── images
│   ├── Confusion Matrixs
│   ├── Word Clouds
│   ├── Images of Decision Trees
├── Data
│   ├── bills_116.csv
│   ├── house_members_116.csv
│   ├── house_legislation_116.csv
│   ├── house_rollcall_info_116.csv
│   ├── house_rollcall_votes_116.csv
│   ├── Committee_Assignments_Cleaned.csv
│   ├── Committee_Assignments.csv
├── .gitignore
├── Bill_Analyzer.ipynb
├── 116Congress.pdf
└── README.md
```
