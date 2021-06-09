# Machine-Learning-on-Classification Problem with challenge to deal high dimentional datasets

<h2>1. Business Problem </h2>

<p> 

Founded in 2000 by a high school teacher in the Bronx, DonorsChoose.org empowers public school teachers from across the country to request much-needed materials and experiences for their students. At any given time, there are thousands of classroom requests that can be brought to life with a gift of any amount

DonorsChoose.org receives hundreds of thousands of project proposals each year for classroom projects in need of funding. Right now, a large number of volunteers is needed to manually screen each submission before it's approved to be posted on the DonorsChoose.org website.

Next year, DonorsChoose.org expects to receive close to 500,000 project proposals. As a result, there are three main problems they need to solve:

How to scale current manual processes and resources to screen 500,000 projects so that they can be posted as quickly and as efficiently as possible

How to increase the consistency of project vetting across different volunteers to improve the experience for teachers

How to focus volunteer time on the applications that need the most assistance

The goal of the competition is to predict whether or not a DonorsChoose.org project proposal submitted by a teacher will be approved, using the text of project descriptions as well as additional metadata about the project, teacher, and school. DonorsChoose.org can then use this information to identify projects most likely to need further review before approval.

With an algorithm to pre-screen applications, DonorsChoose.org can auto-approve some applications quickly so that volunteers can spend their time on more nuanced and detailed project ​vetting processes, including doing more to help teachers develop projects that qualify for specific funding opportunities.

Your machine learning algorithm can help more teachers get funded more quickly, and with less cost to DonorsChoose.org, allowing them to channel even more funding directly to classrooms across the country.

</br>

<h2> 1.1 Real World / Business Objectives and Constraints </h2>


**A. No latency constraint:**  

**B. Model interpretability important:**


<h2>2. Machine Learning problem </h2>

<h3> 2.1 Data </h3>

<h3> 2.1.1 Data Overview </h3>

<p> 
Data fields

The files consist of a list of product listings. These files are tab-delimited.

** project essay dataset ** 

* id - unique id of the project application
* teacher_id - id of the teacher submitting the application
* teacher_prefix - title of the teacher's name (Ms., Mr., etc.)
* school_state - US state of the teacher's school
* project_submitted_datetime - application submission timestamp
* project_grade_category - school grade levels (PreK-2, 3-5, 6-8, and 9-12)
* project_subject_categories - category of the project (e.g., "Music & The Arts")
* project_subject_subcategories - sub-category of the project (e.g., "Visual Arts")
* project_title - title of the project
* project_essay_1 - first essay*
* project_essay_2 - second essay*
* project_essay_3 - third essay*
* project_essay_4 - fourth essay*
* project_resource_summary - summary of the resources needed for the project
* teacher_number_of_previously_posted_projects - number of previously posted * applications by the submitting teacher
* project_is_approved - whether DonorsChoose proposal was accepted (0="rejected", 1="accepted"); train.csv only
* Note: Prior to May 17, 2016, the prompts for the essays were as follows:

* project_essay_1: "Introduce us to your classroom"
* project_essay_2: "Tell us more about your students"
* project_essay_3: "Describe how your students will use the materials you're requesting"
* project_essay_4: "Close by sharing why your project will make a difference" 

** Resource dataset **

Proposals also include resources requested. Each project may include multiple requested resources. Each row in resources.csv corresponds to a resource, so multiple rows may tie to the same project by id.

* id - unique id of the project application; joins with test.csv. and train.csv on id
* description - description of the resource requested
* quantity - quantity of resource requested
* price - price of resource requested

 </br>

Refer: https://www.kaggle.com/c/donorschoose-application-screening/overview/evaluation

<h2>2.2 Mapping the real-world problem to a Machine Learning Problem </h2>

<h3> 2.2.1 Type of Machine Learning Problem </h3>

<p> It is a classification problem  <br>

<h3>2.2.2 Performance metric </h3>

<p> The goal of this competition is to predict whether an application to DonorsChoose is accepted. Submissions are evaluated on area under the ROC curve between the predicted probability and the observed target. <br>

<h2>3 Results </h2>

| Model/Algorithm  | Vectorizer    | Max_depth | n_estimators | Test -AUC |
| :------------ |:---------------:| -----:|-----:|-----:|
| XG Boost | TFIDF W2V with Response Coding | 2 | 150 | 0.687 |
| XG Boost | TF IDF with Response Coding | 2 | 150 | 0.737 |
| Decision Tree |  TFIDF | 10 | 500 | 0.64 |
| Decision Tree |  TF IDF W2V | 10 | 500 | 0.61 |
| Naive Bayes | BOW | 1 | 150 | 0.687 |
| Naive Bayes | TF IDF | 0.01 | 150 | 0.667 |

<h2>3 Conclusion </h2>

<h3>3.2 Observation of Decision Tree model buildling on DonorsChoose </h3>

<p> 
  1) we transform our essay into sentiment score so that we can see how much our sentence is positive, negative, neutral (ration) (all three sum to 1) it means it gives values how much oot 100% sentence is positive, negative and neutral whereas compound gives us values between -1 to 1, if value towards 1 then it is positive and value towards -1 it is negative

2) fit and transform: so we only fit on training data because we use fit_transform() on the train data so that we learn the parameters of scaling on the train data and in the same time we scale the train data. We only use transform() on the test data because we use the scaling paramaters learned on the train data to scale the test data.

3) probability score to find AUC: here we use AUC as metric so that we need probability score to get AUC score and we care about positive class so that we use x_train [:,1] in the code will give you the probabilities of getting the output as 1. If you replace 1 with 0 in the above code, you will only get the probabilities of getting the output as 0. So that here we use 1 because we want to get those probabilti values which gives us as output 1 positive class

4) AUC interpretation: Auc gives us probabilt score let's say 0.70 which means 70% of chances that out model can classify in between positive and negative class but in our case we only use probability for class 1 

5) After computing confusion metrix, we have asked to do some calculation on false positive class. Now as we know interpretation of type 1 and type 2 error can change as per problem for example cancer case study, false positive as it predict cancer but it was not cancer while false negative, it predicts no cancer but it was cancer so in that case false negative is more dangor then false positive. However, in our case false positive means it predicts gives fund but it was not actually, and false negative means it predicts no funding but it was yes funcding so here false positive is more important than false negative as we are giving funds to those projects which are actually not important. . 

6) Flase negative in interpretation on wordcloud, previously posted projects and price: here we want to see that in what cases where we get false postitve values so that we can interpret our model. From, word cloud, we can say that thee are words which is responsible to get false positive values. For price feature, we want box plot so first we create dataframe of false positive points and then plot box plot of price data so that we can see in which price values we get false positive values. From cdf-pdf plot of previously posted features, we can interpret that there ara 90% chances of false positive when we have previously posted projects number is <= 40. while pdf says 50% false positive data points has only 5 previously posted project num while there are majority very few number of previously projects submitted. 
  <br>

  <h3>3.3 GBDT model building on DonorsChoose </h3>
  
  <p>
  1) GBDT model building we have changed calculation of categorical features rather than tfidf we have used response coding approach the key difference is when we apply response coding we limit our dimension while with TFIDF or other vectorizer dimensions increases 

2) GBDT is faster than normal decison tree as we have used XGBoost implementation and XGBoost internally use parallel computation despite of model's sequencial calculation 

3) GBDT works faily well with TFIDf and response coding it gives aroung 0.73 AUC score 
  <br>
  
   <h3>3.4 Naive Bayes model building on DonorsChoose </h3>
  
  <p> 
  1) Naive Bayes works well for text data and categorical data while numerical data, data has to be gusssian distributed though we can use gaussinNB model for it but typically Naive bayes works well for text and categorical data.

2) In this case study, we apply simple BOW and TFIDF for NB model building and we have got 0.70 AUC score for BOW implementation. 

3) Also we get high probability features for both positive class and negative class classification so that we can interpreat model for further decision making 
  <br>
  
   <h3>3.5 Closing Remarks </h3>
   
   <p> 
  We can go with GBDT as it gives higher AUC score and faster to train and we can interpreat the model however if we are after simple model then Naive Bayes will be the choic which is works faily well for small dataset, faster to tarin and easily interpreatbel with probabiltiy score. 
  <br>
