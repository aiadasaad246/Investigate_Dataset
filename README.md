# Investigate_Dataset

In Brazil, patients make a doctor appointment before going to the clinic. However, the clinics have complained that about 30% of the patients miss their scheduled medical appointment. Therefore, they have collected a dataset containing information about all patients. They just want to know what are the factors that affect whether the patient will show on his/her medical appointment or not? Are the health conditions affect the no-show possibility? Does sending a reminder SMS to affect the possibility no-show? What are the characteristics of No-show appointments?







Project: Medical Appointment No Shows Data Analysis
Table of Contents
Introduction
Data Wrangling
Exploratory Data Analysis
Conclusions

Introduction
In Brazil, patients make a doctor appointment before going to the clinic. However, the clinics have complained that about 30% of the patients miss their scheduled medical appointment. Therefore, they have collected a dataset containing information about all patients. They just want to know what are the factors that affect whether the patient will show on his/her medical appointment or not? Are the health conditions affect the no-show possibility? Does sending a reminder SMS to affect the possibility no-show? What are the characteristics of No-show appointments?

 # First I import the packages that I will  use to facilitate the process of analysis the dataset
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

Data Wrangling
General Properties
# to Load dataset 
ds=pd.read_csv("Dataset.csv")
# to print the first few lines
ds.head()
PatientId	AppointmentID	Gender	ScheduledDay	AppointmentDay	Age	Neighbourhood	Scholarship	Hipertension	Diabetes	Alcoholism	Handcap	SMS_received	No-show
0	2.987250e+13	5642903	F	2016-04-29T18:38:08Z	2016-04-29T00:00:00Z	62	JARDIM DA PENHA	0	1	0	0	0	0	No
1	5.589978e+14	5642503	M	2016-04-29T16:08:27Z	2016-04-29T00:00:00Z	56	JARDIM DA PENHA	0	0	0	0	0	0	No
2	4.262962e+12	5642549	F	2016-04-29T16:19:04Z	2016-04-29T00:00:00Z	62	MATA DA PRAIA	0	0	0	0	0	0	No
3	8.679512e+11	5642828	F	2016-04-29T17:29:31Z	2016-04-29T00:00:00Z	8	PONTAL DE CAMBURI	0	0	0	0	0	0	No
4	8.841186e+12	5642494	F	2016-04-29T16:07:23Z	2016-04-29T00:00:00Z	56	JARDIM DA PENHA	0	1	1	0	0	0	No
Now we know some information about our data. there are a patient id , appointment id , gender of the patient, scheduled day, appointment day, age, neighbourhood, scolarship, hiportension, diabetes, alcoholism, handcap, sms_received and no-show colums. From this we can notice that there are some typos error in the columns names

patient id , appointment id are just unique ids for each patient
gender give us whether the patient (M) male or (F) female
scheduled day is the day in which the patient call the clinic and take an appointment
appointment day is the day in which the patient should go to the medical appointment
age is the age of that patient
neighbourhood is Where the appointment takes place
scolarship whether the patient take a support or not
hiportension, diabetes, alcoholism, handcap whether the patient has suffer from them or not
sms_received a message sent to the patient
no-show yes mean that the patient did not show the medical appointment no mean he/she attend the appointment
#the niumber of colums and rows in the dataset
ds.shape
(110527, 14)
#To see if there are dublicated data or not
sum(ds.duplicated())
0
From that we know that there are no any duplicated data

# To obtain a summary descriptive statstics about the dataset
ds.describe()
PatientId	AppointmentID	Age	Scholarship	Hipertension	Diabetes	Alcoholism	Handcap	SMS_received
count	1.105270e+05	1.105270e+05	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000	110527.000000
mean	1.474963e+14	5.675305e+06	37.088874	0.098266	0.197246	0.071865	0.030400	0.022248	0.321026
std	2.560949e+14	7.129575e+04	23.110205	0.297675	0.397921	0.258265	0.171686	0.161543	0.466873
min	3.921784e+04	5.030230e+06	-1.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
25%	4.172614e+12	5.640286e+06	18.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
50%	3.173184e+13	5.680573e+06	37.000000	0.000000	0.000000	0.000000	0.000000	0.000000	0.000000
75%	9.439172e+13	5.725524e+06	55.000000	0.000000	0.000000	0.000000	0.000000	0.000000	1.000000
max	9.999816e+14	5.790484e+06	115.000000	1.000000	1.000000	1.000000	1.000000	4.000000	1.000000
From the output we can see that the min age is -1 which is not a valid value.

ds.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 110527 entries, 0 to 110526
Data columns (total 14 columns):
 #   Column          Non-Null Count   Dtype  
---  ------          --------------   -----  
 0   PatientId       110527 non-null  float64
 1   AppointmentID   110527 non-null  int64  
 2   Gender          110527 non-null  object 
 3   ScheduledDay    110527 non-null  object 
 4   AppointmentDay  110527 non-null  object 
 5   Age             110527 non-null  int64  
 6   Neighbourhood   110527 non-null  object 
 7   Scholarship     110527 non-null  int64  
 8   Hipertension    110527 non-null  int64  
 9   Diabetes        110527 non-null  int64  
 10  Alcoholism      110527 non-null  int64  
 11  Handcap         110527 non-null  int64  
 12  SMS_received    110527 non-null  int64  
 13  No-show         110527 non-null  object 
dtypes: float64(1), int64(8), object(5)
memory usage: 11.8+ MB
From the above information, there is not any null value. The SheduledDsay and AppointmentDay are DateTime not object.

Data Cleaning
As we can figure after assessing the dataset. we find that:

the age has a non-valid value
there are some typos error in the columns names
The SheduledDsay and AppointmentDay are DateTime not object.
</ul> I will clean those error
#Here clean the data errors
#First we rename the columns that have a wrong names
ds = ds.rename(columns={'Hipertension': 'Hypertension', 'Handcap': 'Handicap', 'No-show': 'No_Show' })
# deleting the non valid values of the age that have a value less than 0
ds=ds[ds.Age > 0]
# Convert the type of the AppointmentDay 
ds['AppointmentDay']=pd.to_datetime(ds["AppointmentDay"])
ds['ScheduledDay']=pd.to_datetime(ds["ScheduledDay"])
ds.info()
<class 'pandas.core.frame.DataFrame'>
Int64Index: 106987 entries, 0 to 110526
Data columns (total 14 columns):
 #   Column          Non-Null Count   Dtype              
---  ------          --------------   -----              
 0   PatientId       106987 non-null  float64            
 1   AppointmentID   106987 non-null  int64              
 2   Gender          106987 non-null  object             
 3   ScheduledDay    106987 non-null  datetime64[ns, UTC]
 4   AppointmentDay  106987 non-null  datetime64[ns, UTC]
 5   Age             106987 non-null  int64              
 6   Neighbourhood   106987 non-null  object             
 7   Scholarship     106987 non-null  int64              
 8   Hypertension    106987 non-null  int64              
 9   Diabetes        106987 non-null  int64              
 10  Alcoholism      106987 non-null  int64              
 11  Handicap        106987 non-null  int64              
 12  SMS_received    106987 non-null  int64              
 13  No_Show         106987 non-null  object             
dtypes: datetime64[ns, UTC](2), float64(1), int64(8), object(3)
memory usage: 12.2+ MB
I think the patient and the appointment ID, scheduledDay and Neighbourhood will not give us information so I will drop them

ds.drop(["PatientId","AppointmentID","ScheduledDay",'Neighbourhood'], axis=1, inplace=True)
ds.head()
Gender	AppointmentDay	Age	Scholarship	Hypertension	Diabetes	Alcoholism	Handicap	SMS_received	No_Show
0	F	2016-04-29 00:00:00+00:00	62	0	1	0	0	0	0	No
1	M	2016-04-29 00:00:00+00:00	56	0	0	0	0	0	0	No
2	F	2016-04-29 00:00:00+00:00	62	0	0	0	0	0	0	No
3	F	2016-04-29 00:00:00+00:00	8	0	0	0	0	0	0	No
4	F	2016-04-29 00:00:00+00:00	56	0	1	1	0	0	0	No
ds.info()
<class 'pandas.core.frame.DataFrame'>
Int64Index: 106987 entries, 0 to 110526
Data columns (total 10 columns):
 #   Column          Non-Null Count   Dtype              
---  ------          --------------   -----              
 0   Gender          106987 non-null  object             
 1   AppointmentDay  106987 non-null  datetime64[ns, UTC]
 2   Age             106987 non-null  int64              
 3   Scholarship     106987 non-null  int64              
 4   Hypertension    106987 non-null  int64              
 5   Diabetes        106987 non-null  int64              
 6   Alcoholism      106987 non-null  int64              
 7   Handicap        106987 non-null  int64              
 8   SMS_received    106987 non-null  int64              
 9   No_Show         106987 non-null  object             
dtypes: datetime64[ns, UTC](1), int64(7), object(2)
memory usage: 9.0+ MB
ds.head()
Gender	AppointmentDay	Age	Scholarship	Hypertension	Diabetes	Alcoholism	Handicap	SMS_received	No_Show
0	F	2016-04-29 00:00:00+00:00	62	0	1	0	0	0	0	No
1	M	2016-04-29 00:00:00+00:00	56	0	0	0	0	0	0	No
2	F	2016-04-29 00:00:00+00:00	62	0	0	0	0	0	0	No
3	F	2016-04-29 00:00:00+00:00	8	0	0	0	0	0	0	No
4	F	2016-04-29 00:00:00+00:00	56	0	1	1	0	0	0	No
Now we have 10 variable. the only dependent variable is No_Show and the other are all independent variables


Exploratory Data Analysis
Below we see that about 80 % of the patients have shown up for the medical appointment and 20 % have not shown up.

fig, ax =plt.subplots(figsize=(10,7))
a= ds["No_Show"].value_counts()
print("The number od the patients whow show up is {} and te num of those who did not show up is {}".format(a[0],a[1]))
a.plot(kind="bar", title="No_Show vs Show");
ax.set_xlabel(" Show'No'    No_show'Yes' ")
ax.set_ylabel("Num of No_show and Show patients")
The number od the patients whow show up is 85307 and te num of those who did not show up is 21680
Text(0, 0.5, 'Num of No_show and Show patients')

#This will give us the rows in which the patient show the mediacl appointment
Show=ds.No_Show=='No'
#This will give us the rows in which the patient no show the mediacl appointment
No_Show=ds.No_Show=='Yes'
#to see whether the Age affect showing up for the medical appointment or not 
fig, ax =plt.subplots(figsize=(10,7))
ds.Age[Show].hist(alpha=1,label='show');
ds.Age[No_Show].hist(alpha=1, label='no_show');
ax.set_title("Age Distripution")
ax.set_xlabel("Age value")
ax.set_ylabel("Show and No_Show")
plt.legend()
<matplotlib.legend.Legend at 0x2670da87e20>

From above there is no any indication that can tell us the age affect the show up or not, therefore, I think Rhe age is not a factor by which we can indicate whether a patient will show up or not

fig, ax =plt.subplots(figsize=(8,8))
#count the num of males and females
a=ds.Gender.value_counts()
#count the num of males and females who show up for the medial appointment
b=ds.Gender[Show].value_counts()
b.plot(kind='bar')
ax.set_title("Gender Distripution")
ax.set_xlabel("F for Female, M for Males")
ax.set_ylabel("Show and No_Show")
#count the num of males and females who did not  show up for the medial appointment
c=ds.Gender[No_Show].value_counts()
# the percentage of the female who show up 
f=(b[0]/a[0])*100
# the percentage of the female who did not show up 
f1=(c[0]/a[0])*100
# the percentage of the male who show up
m=(b[1]/a[1])*100
# the percentage of the male who did not show up

m1=(c[1]/a[1])*100
print("The percentage of the female who show up is {:0.2f} % and the percentage who did not show up is {:0.2f}% " .format(f,f1))
print("the percentage of the male who show up is {:0.2f} % and the percentage who did not show up is {:0.2f} %".format(m,m1))
The percentage of the female who show up is 79.64 % and the percentage who did not show up is 20.36% 
the percentage of the male who show up is 79.92 % and the percentage who did not show up is 20.08 %

Also from above, we can't consider the gender as a factor affecting whether the patient will show up or not Because both female and male have the same percentage od showing up

In the next step, I'm going to build a function to plot and print the percentage of the patients who have shown up and those who have not shown up

def plotfunc(l):
    #l represnt a data of a column for example l can be ds.Scholarship
    #to see whether the factor affect showing up for the medical appointment or not 
    fig, ax =plt.subplots(figsize=(10,7))
    ax.hist( l[Show],alpha=0.5,label='show');
    ax.hist( l[No_Show],alpha=1, label='no_show');
    ax.set_title("Result of {}".format(l.name))
    ax.set_xlabel(l.name)
    ax.set_ylabel("Show and No_Show")
    plt.legend()

    #this is the num of the patient with and withput factor
    wo=l.value_counts()
    #this is the num of the patient with and withput factor and show up 
    s=(l[Show]).value_counts()
    #this is the num of the patient with and withput factor and did not show up 
    n=(l[No_Show]).value_counts()
    sw=(s[1]/wo[1])*100
    so=(s[0]/wo[0])*100
    name=l.name
    print("The percentage of patients with {} and show up is {:.2f} %".format(name,sw))
    print("The percentage of patients WithOUt {} and show up is {:.2f} %".format(name,so))  
# Here this function give us details about the scholarship
plotfunc(ds.Scholarship)
The percentage of patients with Scholarship and show up is 76.21 %
The percentage of patients WithOUt Scholarship and show up is 80.13 %

Therefore we can see that the patient without scholarship show up for the medical appointment more than those with scholarship

plotfunc(ds.Hypertension)
The percentage of patients with Hypertension and show up is 82.70 %
The percentage of patients WithOUt Hypertension and show up is 78.98 %

Therefore we can see that the patient with Hypertension show up for the medical appointment more than those WithOUt Hypertension

plotfunc(ds.Diabetes)
The percentage of patients with Diabetes and show up is 82.00 %
The percentage of patients WithOUt Diabetes and show up is 79.55 %

Therefore we can see that the patient with Diabetes show up for the medical appointment more than those WithOUt Diabetes

plotfunc(ds.Alcoholism)
The percentage of patients with Alcoholism and show up is 79.85 %
The percentage of patients WithOUt Alcoholism and show up is 79.73 %

From above it's obvious that the factor of Alcoholism is not affecting whether the patient will show up or not

plotfunc(ds.Handicap)
The percentage of patients with Handicap and show up is 82.07 %
The percentage of patients WithOUt Handicap and show up is 79.69 %

Therefore we can see that the patient with Handicap show up for the medical appointment more than those WithOUt Handicap

plotfunc(ds.SMS_received)
The percentage of patients with SMS_received and show up is 72.33 %
The percentage of patients WithOUt SMS_received and show up is 83.27 %

Therefore we can see that the patient WithOUt SMS_received shows up for the medical appointment more than those with SMS_received. It seems so wired as it was expected the opposite. Therefore the ansower to the qustion of whether the sms affect or not is yes

A=ds[Show]
A represents all the dataset of only the patients who show up

#this function return the data of a certain condition to thoose patients who show up
def sdata( a,b, c):
    x=b==c
    d=a[x]
    return d
Show_Scholarship=sdata(A,A.Scholarship,1)
Show_Scholarship_No_Hypertension=sdata(Show_Scholarship,Show_Scholarship.Hypertension,0)
Show_S_No_Hyper_No_Alco=sdata(Show_Scholarship_No_Hypertension,Show_Scholarship_No_Hypertension.Alcoholism,0)
Show_no_Daibets=sdata(Show_S_No_Hyper_No_Alco,Show_S_No_Hyper_No_Alco.Diabetes,0)
show_no_Handicap=sdata(Show_no_Daibets,Show_no_Daibets.Handicap,0)
##This dataset represent the patients who show up and have no any condition health
Show_Scholarship_Without_Health_Conditions=show_no_Handicap
Now we have the data of the patients that have a scholarship and without Hypertension, Diabetes, Alcoholism or Handicap we can count the number of them now and the percentage from the whole patients who show up

#this is the Total num of the patients who Show up with Scholarship and without health condition
z=Show_Scholarship_Without_Health_Conditions.No_Show.value_counts()
#this is the total num of the patients who show up 
q=A.No_Show.value_counts()
print("the percentage of patients who show up and have a scholarship but no any health conditions is {:0.2f}".format((z[0]/q[0])*100))
the percentage of patients who show up and have a scholarship but no any health conditions is 7.41
This thing tells us that the patients that have a health condition and don't have scholarship show up way more than those who have a scholarship and no health condition. Therefore Now we can answer the question of whether the health conditions affect the no-show possibility or not.

Therefore the most important characteristics of the patients who show up are that they don't have a scholarship and also don't have any health condition


Conclusions
Limetations
The most challenge part is the part of combining the data together to know their effects at the same time. Moreover, I have found a difficulty of the plotting part that's because it was not addressed widely in the lessons I think the data is really sufficient and there is huge information that can be extracted from it to make a good prediction. Fortunately the data has no missing values but I think there is a non-valid data that should be taken for granted.

We can conclude that

the factors that affect the show up are Scholarship, SMS_received, Handicap, Diabetes, Hypertension
the factors that do not affect the show up are Gender, Age, Alcoholism
we also can say that the patient without scholarship and suffer from Handicap, Diabetes and Hypertension may have a higher prediction of show up for the medical appointment
we also can say that the patient without scholarship and suffer from Handicap, Diabetes and Hypertension may have a higher prediction of show up for the medical appointment The most important thing is that the probability of the patients who show up and don't have scholarship and health conditions is about 0.93 which is good for prediction

To sum up, Of course, there are many other factors that can be extracted from this dataset but it needs more information and research to do that

from subprocess import call
call(['python', '-m', 'nbconvert', 'Investigate_a_Dataset.ipynb'])
0
