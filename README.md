# NLTK Analysis Program
This program is designed to evaluate the attitude on crawled text data from Twitter. 

# Import pandas for graphing purpose
import pandas as pd

# Separate out the 'text' column from crawled csv file. 
df = pd.read_csv('forestfire.csv',encoding = "ISO-8859-1")
reviews = df[['text']]

# Create dictionary list (prositive)

dict_p = []
f = open('positive-words.txt', 'r')   
for line in f:
    t = line.strip().lower()
    if t is not None and len(t) > 0:
        dict_p.append(t)
f.close()

# Create dictionary list (negative)

dict_n = []
f = open('negative-words.txt', 'r')
f = open('negative-words.txt', 'r', encoding='ISO-8859-1') 
for line in f:
    t = line.strip().lower()
    if t is not None and len(t) > 0:
         dict_n.append(t)
f.close()

# Count of positive and negative words that appeared in each message
# Net count which is calculated by positive count subtracting negative count. 

poscnt = []
negcnt = []
netcnt = []

for nrow in range(0,len(reviews)):
    
    text = df.text[nrow].lower()
    qa = 0
    qb = 0

    for word in dict_p :
        if (word in text) :
            qa = qa + 1

    for word in dict_n :
        if (word in text) :
            qb = qb + 1

    qc = qa - qb
    
    poscnt.append(qa)
    negcnt.append(qb)
    netcnt.append(qc)
    
# Count for each attitude category    
df['poscnt'] = poscnt
df['negcnt'] = negcnt
df['netcnt'] = netcnt
df[['text','poscnt','negcnt','netcnt']]

# Create a new dataframe and group the results together

df['result_NLTK'] = result_NLTK
df[['text','pos_score','neu_score','neg_socre','comp_score','result_NLTK']]

# Create the pie chart
pie_label = ['positive', 'nagative', 'neutral']
series = pd.Series([pos,neg,net], 
                   index=pie_label, 
                   name='sentiment output')
series.plot.pie(figsize=(6, 6),autopct='%1.1f%%')
