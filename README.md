import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score
df = pd.read_csv('/content/mail_data.csv')
df
data = df.where(pd.notnull(df)," ")
data.head()
data.loc[data['Category']=='spam','Category'] = 0
data.loc[data['Category']=='ham','Category'] = 1
x = data['Message']
y = data['Category']
print(x)
print(y)
x_train,x_test,y_train,y_test = train_test_split(x,y,test_size=0.2,random_state = 3)
print(x.shape)
print(x_train.shape)
print(x_test.shape)
print(y.shape)
print(y_train.shape)
print(y_test.shape)
# Define the TF-IDF vectorizer with specified parameters
feature_extraction = TfidfVectorizer(min_df=1, stop_words='english', lowercase=True)

# Fit and transform the training data
x_train_features = feature_extraction.fit_transform(x_train)

# Only transform the test data (don't fit again)
x_test_features = feature_extraction.transform(x_test)

# Convert labels to integers if necessary
y_train = y_train.astype('int')
y_test = y_test.astype('int')
print(x_train)
print(x_train_features)
model = LogisticRegression()
model.fit(x_train_features,y_train)
prediction_on_training_data = model.predict(x_train_features)
accuracy_score_on_training_data = accuracy_score(y_train,prediction_on_training_data)
print('accuracy in my training data is :',accuracy_score_on_training_data)
prediction_on_test_data = model.predict(x_test_features)
accuracy_score_on_test_data = accuracy_score(y_test,prediction_on_test_data)
print('accuracy in my test data is :',accuracy_score_on_test_data)
input = ["SIX chances to win CASH! From 100 to 20,000 pounds txt> CSH11 and send to 87575. Cost 150p/day, 6days, 16+ TsandCs apply Reply HL 4 info"]
input_data_features = feature_extraction.transform(input)
prediction = model.predict(input_data_features)
print(prediction)
if(prediction[0]==1):
  print("valid email")
else :
  print("spam mail")
