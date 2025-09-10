# Fitness-Data
Fitness Data from three sources

#Import the Data
PS C:\Users\Jumaa> cd C:\Users\Jumaa\OneDrive\Documents\tableau_data
PS C:\Users\Jumaa\OneDrive\Documents\tableau_data> python
Python 3.13.7 (tags/v3.13.7:bcee1c3, Aug 14 2025, 14:15:11) [MSC v.1944 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import pandas as pd
>>> nesthub = pd.read_json("googlefitdata.json")
>>> ringconn = pd.read_csv("ringconn_sleep.csv")
>>> zepp = pd.read_csv("zepp_activity.csv")
>>> print(nesthub.head())
  fitnessActivity                 startTime                   endTime    duration       date
0           sleep  2025-08-01T05:55:09.621Z  2025-08-01T14:27:23.332Z  merged -s 2025-08-01
1           sleep  2025-08-02T09:09:47.174Z  2025-08-02T16:19:08.327Z  25761.153s 2025-08-02
2           sleep  2025-08-03T07:56:46.039Z  2025-08-03T15:38:49.380Z  27723.341s 2025-08-03
3           sleep  2025-08-04T06:29:11.114Z  2025-08-04T14:07:04.213Z  27473.099s 2025-08-04
4           sleep  2025-08-05T05:08:36.200Z  2025-08-05T14:25:54.794Z  33438.594s 2025-08-05
>>> 
#Standardize Date columns
>>> ringconn['date'] = pd.to_datetime(ringconn['date']).dt.date
>>> nesthub ['date'] = pd.to_datetime(nesthub['date']).dt.date
>>>  zepp['date'] = pd.to_datetime(zepp['date']).dt.date

#Merge Files

>>> merged = ringconn.merge(nesthub, on="date", how="outer")
>>> merged = merged.merge(zepp, on="date", how="outer")
>>> print(merged.head())
         date  duration_x fitnessActivity                 startTime  ... steps distance  runDistance  calories
0  2025-08-01         383           sleep  2025-08-01T05:55:09.621Z  ...  8271     6029         4680       410
1  2025-08-02         275           sleep  2025-08-02T09:09:47.174Z  ...  2034     1439         1268       105
2  2025-08-03         313           sleep  2025-08-03T07:56:46.039Z  ...  3665     2598         2110       171
3  2025-08-04         359           sleep  2025-08-04T06:29:11.114Z  ...  8686     6309         5040       658
4  2025-08-05         285           sleep  2025-08-05T05:08:36.200Z  ...  6686     4716         3991       674

[5 rows x 10 columns]
>>> merged.to_csv("cleaned_date.csv", index=False)
