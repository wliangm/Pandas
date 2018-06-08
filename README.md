
                                                                                                Pandas HW - Lucas Liang

#  PyCity Schools Analysis:

###### \- Based on the analysis, schools with smaller budgets of \\$615 or less per student have better test results than schools with higher budgets. In general, small budgets schools have -/+ 95% of overall passing rate, but high budgets schools have -/+ 75% of overall passing rate.

###### \- According to scores by school sizes analysis, small to medium sized school performed much better test result than large sized school, with overall passing rate of 94-95%. In contrast, large sized school have overall passing rate of 75%.

###### \- Per school type, Charter schools performed much better than District schools on test result with overall passing rate of 95%.  On other hand, District schools have overall passing rate of 73% only.


```python
import numpy as np
import pandas as pd
```


```python
#get school file
schools_file = "C:/Users/LENOVO USER/Desktop/USC Data Bootcamp/4-HW, Python Panda, 6-9-18/04-Numpy-Pandas/Instructions/PyCitySchools/raw_data/schools_complete.csv"

school_df = pd.read_csv(schools_file, encoding="ISO-8859-1")

school_df = pd.read_csv(schools_file)

sorted_school_df = school_df.sort_values('name',  ascending=True)

#check
#sorted_school_df.head()
```


```python
#get student file
student_file = "C:/Users/LENOVO USER/Desktop/USC Data Bootcamp/4-HW, Python Panda, 6-9-18/04-Numpy-Pandas/Instructions/PyCitySchools/raw_data/students_complete.csv"

student_df = pd.read_csv(student_file, encoding="ISO-8859-1")

student_df = pd.read_csv(student_file)

#check
#student_df.head()
```


```python
#count for total school
total_schools = school_df['name'].count()

#check
#print(total_schools)
```


```python
#count for total student
total_students = student_df['name'].count()

#check
#print(total_students)
```


```python
#calculate total budget
total_budget = school_df['budget'].sum()

#check
#print(total_budget)
```


```python
#calculate average of math score
avg_math = student_df['math_score'].mean()

#check
#print(avg_math)
```


```python
#calculate average of reading score
avg_reading = student_df['reading_score'].mean()

#check
#print(avg_reading)
```


```python
#get a list of passing math student with math score greater than or equal to 70
passing_math = student_df[student_df['math_score'] >= 70]
#print(passing_math)

#count student name from passing math student list
number_passing_math = passing_math["name"].count()
#print(number_passing_math)

#calculate % passing math score
percent_passing_math = (number_passing_math/total_students)*100

#check
#print(str(percent_passing_math) + '%')
```


```python
#get a list of passing read student with reading score greater than or equal to 70
passing_read = student_df[student_df['reading_score'] >= 70]
#print(passing_math)

#count student name from passing read student list
number_passing_read = passing_read["name"].count()
#print(number_passing_math)

#calculate % passing reading score
percent_passing_read = (number_passing_read/total_students)*100

#check
#print(str(percent_passing_read) + '%')
```


```python
#calculate the average of % of passing math and % of passing reading
overall_pass_rate = (percent_passing_math + percent_passing_read)/2

#check
#print(str(overall_pass_rate) + '%')
```

# District Summary:


```python
#create column header for new dataframe of summary
col_names = ['Total Schools','Total Students','Total Budget','Average Math Score','Average Reading Score','% Passing Math','% Passing Reading','% Overall Passing Rate']

#create new dataframe for summary
d_summary_df = pd.DataFrame(columns = col_names)

#append values into summary dataframe
d_summary_df.loc[len(d_summary_df)] = [total_schools,total_students,total_budget,avg_math,avg_reading,percent_passing_math,percent_passing_read,overall_pass_rate]

#format field values
d_summary_df['Total Schools'] = d_summary_df['Total Schools'].map("{:,.0f}".format)
d_summary_df['Total Students'] = d_summary_df['Total Students'].map("{:,.0f}".format)
d_summary_df['Total Budget'] = d_summary_df['Total Budget'].map("${:,.2f}".format)
d_summary_df['Average Math Score'] = d_summary_df['Average Math Score'].map("{:.2f}".format)
d_summary_df['Average Reading Score'] = d_summary_df['Average Reading Score'].map("{:.2f}".format)
d_summary_df['% Passing Math'] = d_summary_df['% Passing Math'].map("{:.2f}".format)
d_summary_df['% Passing Reading'] = d_summary_df['% Passing Reading'].map("{:.2f}".format)
d_summary_df['% Overall Passing Rate'] = d_summary_df['% Overall Passing Rate'].map("{:.2f}".format)

#display district summary
d_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>$24,649,428.00</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>74.98</td>
      <td>85.81</td>
      <td>80.39</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create a new table with the following:
#groupby and calculate each variables for mean of 'reading_score', mean of 'math_score', count of 'total student for each school'
#count of passing math per school, count of passing reading per school
#calculate % of passing math and reading per school

mean_read_grouped_student = student_df.groupby('school')['reading_score'].mean()
mean_math_grouped_student = student_df.groupby('school')['math_score'].mean()

total_student_per_school = student_df.groupby('school')['name'].count()
passing_math_grouped_student_df = passing_math.groupby('school')['name'].count()
passing_read_grouped_student_df = passing_read.groupby('school')['name'].count()


new_stats_student_df = pd.DataFrame({'avg_read': mean_read_grouped_student,'avg_mat':mean_math_grouped_student,'total_student':total_student_per_school
                              , 'total_passing_math':passing_math_grouped_student_df,'total_passing_read':passing_read_grouped_student_df})

new_stats_student_df = new_stats_student_df.reset_index()

new_stats_student_df['percent_passing_math'] = (new_stats_student_df.total_passing_math / new_stats_student_df.total_student)*100
new_stats_student_df['percent_passing_read'] = (new_stats_student_df.total_passing_read / new_stats_student_df.total_student)*100
new_stats_student_df['overall_passing_rate'] = (new_stats_student_df.percent_passing_math + new_stats_student_df.percent_passing_read)/2

#check
#new_stats_student_df.head()

new2_stats_student_df = new_stats_student_df[['school','avg_mat','avg_read','percent_passing_math','percent_passing_read','overall_passing_rate']]
new2_stats_student_df = new2_stats_student_df.reset_index(drop=True)

#check
#new2_stats_student_df.head()

```


```python
#create a new table to summary school level statistics
school_summary_df = sorted_school_df[['name','type','size','budget']]

#reset index for new data frame
new_school_summary_df = school_summary_df.reset_index(drop=True)

#rename column headers for inner join
new_school_summary_df = new_school_summary_df.rename(columns = {'name':'school', 'type':'school_type','size':'total_student','budget':'total_budget'})

#calculate budget per student
new_school_summary_df['per_student_budget'] = new_school_summary_df.total_budget / new_school_summary_df.total_student


#check for result of new data frame
#new_school_summary_df

```

# School Summary:


```python
#left join for student statistics data
merge_new_school_summary_df = new_school_summary_df.merge(new2_stats_student_df, on='school')

#rename column header
merge_new_school_summary_df = merge_new_school_summary_df.rename(columns = {'school':'School Name','school_type':'School Type',
                                                                'total_student':'Total Students','total_budget':'Total School Budget',
                                                                'per_student_budget':'Per Student Budget','avg_mat':'Average Math Score','avg_read':'Average Reading Score',
                                                                'percent_passing_math':'% Passing Math','percent_passing_read':'% Passing Reading',
                                                                'overall_passing_rate':'% Overall Passing Rate'})

#format field values
merge_new_school_summary_df['Total Students'] = merge_new_school_summary_df['Total Students'].map("{:,}".format)
merge_new_school_summary_df['Total School Budget'] = merge_new_school_summary_df['Total School Budget'].map("${:,.2f}".format)
merge_new_school_summary_df['Per Student Budget'] = merge_new_school_summary_df['Per Student Budget'].map("${:,.2f}".format)
merge_new_school_summary_df['Average Math Score'] = merge_new_school_summary_df['Average Math Score'].map("{:.2f}".format)
merge_new_school_summary_df['Average Reading Score'] = merge_new_school_summary_df['Average Reading Score'].map("{:.2f}".format)
merge_new_school_summary_df['% Passing Math'] = merge_new_school_summary_df['% Passing Math'].map("{:.2f}".format)
merge_new_school_summary_df['% Passing Reading'] = merge_new_school_summary_df['% Passing Reading'].map("{:.2f}".format)
merge_new_school_summary_df['% Overall Passing Rate'] = merge_new_school_summary_df['% Overall Passing Rate'].map("{:.2f}".format)

merge_new_school_summary_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4,976</td>
      <td>$3,124,928.00</td>
      <td>$628.00</td>
      <td>77.05</td>
      <td>81.03</td>
      <td>66.68</td>
      <td>81.93</td>
      <td>74.31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.13</td>
      <td>97.04</td>
      <td>95.59</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>65.99</td>
      <td>80.74</td>
      <td>73.36</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2,739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.31</td>
      <td>79.30</td>
      <td>73.80</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1,468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.39</td>
      <td>97.14</td>
      <td>95.27</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4,635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.29</td>
      <td>80.93</td>
      <td>66.75</td>
      <td>80.86</td>
      <td>73.81</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087.00</td>
      <td>$581.00</td>
      <td>83.80</td>
      <td>83.81</td>
      <td>92.51</td>
      <td>96.25</td>
      <td>94.38</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.68</td>
      <td>81.32</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.06</td>
      <td>81.22</td>
      <td>73.64</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.59</td>
      <td>95.95</td>
      <td>95.27</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.37</td>
      <td>80.22</td>
      <td>73.29</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1,761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.36</td>
      <td>83.73</td>
      <td>93.87</td>
      <td>95.85</td>
      <td>94.86</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.27</td>
      <td>97.31</td>
      <td>95.29</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.87</td>
      <td>96.54</td>
      <td>95.20</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1,800</td>
      <td>$1,049,400.00</td>
      <td>$583.00</td>
      <td>83.68</td>
      <td>83.95</td>
      <td>93.33</td>
      <td>96.61</td>
      <td>94.97</td>
    </tr>
  </tbody>
</table>
</div>



# Top Performing Schools (By Passing Rate):


```python
#sort by '% Overall Passing Rate' from top to bottom, descending order
top_sorted_merge_new_school_summary_df = merge_new_school_summary_df.sort_values(by=['% Overall Passing Rate'],ascending=False)

#reset index
top_sorted_merge_new_school_summary_df = top_sorted_merge_new_school_summary_df.reset_index(drop=True)

#select top 5 from sorted and re-index dataframe list
top_sorted_merge_new_school_summary_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1,858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>94.13</td>
      <td>97.04</td>
      <td>95.59</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1,635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>93.27</td>
      <td>97.31</td>
      <td>95.29</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1,468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>93.39</td>
      <td>97.14</td>
      <td>95.27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>94.59</td>
      <td>95.95</td>
      <td>95.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2,283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>93.87</td>
      <td>96.54</td>
      <td>95.20</td>
    </tr>
  </tbody>
</table>
</div>



# Bottom Performing Schools (By Passing Rate):


```python
#sort by '% Overall Passing Rate' from top to bottom, ascending order
bottom_sorted_merge_new_school_summary_df = merge_new_school_summary_df.sort_values(by=['% Overall Passing Rate'],ascending=True)

#reset index
bottom_sorted_merge_new_school_summary_df = bottom_sorted_merge_new_school_summary_df.reset_index(drop=True)

#select top 5 from sorted and re-index dataframe list
bottom_sorted_merge_new_school_summary_df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Name</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3,999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>66.37</td>
      <td>80.22</td>
      <td>73.29</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2,949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>65.99</td>
      <td>80.74</td>
      <td>73.36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2,917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>65.68</td>
      <td>81.32</td>
      <td>73.50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4,761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>66.06</td>
      <td>81.22</td>
      <td>73.64</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2,739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>68.31</td>
      <td>79.30</td>
      <td>73.80</td>
    </tr>
  </tbody>
</table>
</div>



# Math Score by Grade:


```python
#filter by grade equal to 9th grade
nineth_grade_df = student_df[student_df['grade'] == '9th']
#groupby school and calculate mean of math score
mean_math_grouped_9th_student = nineth_grade_df.groupby('school')['math_score'].mean()

#filter by grade equal to 10th grade
tenth_grade_df = student_df[student_df['grade'] == '10th']
#groupby school and calculate mean of math score
mean_math_grouped_tenth_student = tenth_grade_df.groupby('school')['math_score'].mean()

#filter by grade equal to 11th grade
eleven_grade_df = student_df[student_df['grade'] == '11th']
#groupby school and calculate mean of math score
mean_math_grouped_eleven_student = eleven_grade_df.groupby('school')['math_score'].mean()

#filter by grade equal to 12th grade
twelve_grade_df = student_df[student_df['grade'] == '12th']
#groupby school and calculate mean of math score
mean_math_grouped_twelve_student = twelve_grade_df.groupby('school')['math_score'].mean()

math_score_bygrade_df = pd.DataFrame({'12th':mean_math_grouped_twelve_student,'11th':mean_math_grouped_eleven_student
                                      ,'10th':mean_math_grouped_tenth_student,'09th':mean_math_grouped_9th_student})

#reset index
math_score_bygrade_df = math_score_bygrade_df.reset_index()

#nineth_grade_df
#mean_math_grouped_9th_student

math_score_bygrade_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>09th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



# Reading Score by Grade:


```python
#groupby school and calculate mean of reading score
mean_read_grouped_9th_student = nineth_grade_df.groupby('school')['reading_score'].mean()

#groupby school and calculate mean of reading score
mean_read_grouped_tenth_student = tenth_grade_df.groupby('school')['reading_score'].mean()

#groupby school and calculate mean of reading score
mean_read_grouped_eleven_student = eleven_grade_df.groupby('school')['reading_score'].mean()

#groupby school and calculate mean of reading score
mean_read_grouped_twelve_student = twelve_grade_df.groupby('school')['reading_score'].mean()

read_score_bygrade_df = pd.DataFrame({'12th':mean_read_grouped_twelve_student,'11th':mean_read_grouped_eleven_student
                                      ,'10th':mean_read_grouped_tenth_student,'09th':mean_read_grouped_9th_student})

#reset index
read_score_bygrade_df = read_score_bygrade_df.reset_index()


read_score_bygrade_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>09th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Spending:


```python
#create new data table, combining both student and school dataframe
merge_student_school_df = student_df.merge(new_school_summary_df, on='school')

#check for result
#merge_student_school_df
```


```python
#create bins series for spending range (per_student_budget)
spending_bins = [0,585,615,645,675]

#create bins series for school size
size_bins = [0,1000,2000,5000]

#create label for each bin of spending range
spending_group_names = ["<$585", "$585-615", "$615-645", "$645-675"]

#create label for each bin of school size
size_group_names = ['Small (<1000)','Medium (1000-2000)','Large (2000-5000)']
```


```python
#set spending category
merge_student_school_df["spending_range"] = pd.cut(merge_student_school_df["per_student_budget"], bins=spending_bins, labels=spending_group_names)

#set size category
merge_student_school_df["school_size"] = pd.cut(merge_student_school_df["total_student"], bins=size_bins, labels=size_group_names)

#check for result
#merge_student_school_df
```


```python
#create new data set for school per spending
score_school_spending_df = merge_student_school_df[['spending_range','name','reading_score','math_score']]
#reset index
score_school_spending_df = score_school_spending_df.reset_index(drop=True)

#create varaiables for % of passing math and reading calculation
passing_math_df = score_school_spending_df[score_school_spending_df ['math_score'] >= 70]
passing_read_df = score_school_spending_df[score_school_spending_df ['reading_score'] >= 70]

#count passing math and reading per each spending range
passing_math_grouped = passing_math_df.groupby('spending_range')['name'].count()
passing_read_grouped = passing_read_df.groupby('spending_range')['name'].count()

#calculate average of math and reading scores for each spending spending range
avg_score_grouped_math = score_school_spending_df.groupby('spending_range')['math_score'].mean()
avg_score_grouped_read = score_school_spending_df.groupby('spending_range')['reading_score'].mean()
#count total student per spending range
total_student_grouped = score_school_spending_df.groupby('spending_range')['name'].count()

#create new data set from the calculation above
new_score_school_spending_df = pd.DataFrame({'avg_read': avg_score_grouped_read,'avg_math':avg_score_grouped_math,'total_student':total_student_grouped
                              , 'total_passing_math':passing_math_grouped,'total_passing_read':passing_read_grouped})

#reset index
new_score_school_spending_df = new_score_school_spending_df.reset_index()

#calculate percentage of passing math, reading and overal passing
new_score_school_spending_df['percent_pass_math'] = (new_score_school_spending_df.total_passing_math / new_score_school_spending_df.total_student)*100
new_score_school_spending_df['percent_pass_read'] = (new_score_school_spending_df.total_passing_read / new_score_school_spending_df.total_student)*100
new_score_school_spending_df['overall_passing'] = (new_score_school_spending_df.percent_pass_math + new_score_school_spending_df.percent_pass_read)/2

#check for result
final_score_school_spending_df = new_score_school_spending_df[['spending_range','avg_math','avg_read','percent_pass_math',
                                                               'percent_pass_read','overall_passing']]

final_score_school_spending_df = final_score_school_spending_df.reset_index(drop=True)
final_score_school_spending_df = final_score_school_spending_df.rename(columns = {'spending_range':'Spending Ranges (Per Student)',
                                                                                 'avg_math':'Average Math Score','avg_read':'Average Reading Score',
                                                                                 'percent_pass_math':'% Passing Math','percent_pass_read':'% Passing Reading',
                                                                                 'overall_passing':'% Overall Passing Rate'})
final_score_school_spending_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Spending Ranges (Per Student)</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;$585</td>
      <td>83.363065</td>
      <td>83.964039</td>
      <td>93.702889</td>
      <td>96.686558</td>
      <td>95.194724</td>
    </tr>
    <tr>
      <th>1</th>
      <td>$585-615</td>
      <td>83.529196</td>
      <td>83.838414</td>
      <td>94.124128</td>
      <td>95.886889</td>
      <td>95.005509</td>
    </tr>
    <tr>
      <th>2</th>
      <td>$615-645</td>
      <td>78.061635</td>
      <td>81.434088</td>
      <td>71.400428</td>
      <td>83.614770</td>
      <td>77.507599</td>
    </tr>
    <tr>
      <th>3</th>
      <td>$645-675</td>
      <td>77.049297</td>
      <td>81.005604</td>
      <td>66.230813</td>
      <td>81.109397</td>
      <td>73.670105</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Size:


```python
#create new data set for school per size
score_school_size_df = merge_student_school_df[['school_size','name','reading_score','math_score']]
#reset index
score_school_size_df = score_school_size_df.reset_index(drop=True)


#create varaiables for % of passing math and reading calculation
passing_math_df2 = score_school_size_df[score_school_size_df['math_score'] >= 70]
passing_read_df2 = score_school_size_df[score_school_size_df['reading_score'] >= 70]

#count passing math and reading per each spending range
passing_math_grouped2 = passing_math_df2.groupby('school_size')['name'].count()
passing_read_grouped2 = passing_read_df2.groupby('school_size')['name'].count()

#calculate average of math and reading scores for each spending spending range
avg_score_grouped_math2 = score_school_size_df.groupby('school_size')['math_score'].mean()
avg_score_grouped_read2 = score_school_size_df.groupby('school_size')['reading_score'].mean()
#count total student per spending range
total_student_grouped2 = score_school_size_df.groupby('school_size')['name'].count()

#create new data set from the calculation above
new_score_school_size_df = pd.DataFrame({'avg_read': avg_score_grouped_read2,'avg_math':avg_score_grouped_math2,'total_student':total_student_grouped2
                              , 'total_passing_math':passing_math_grouped2,'total_passing_read':passing_read_grouped2})

#reset index
new_score_school_size_df = new_score_school_size_df.reset_index()

#calculate percentage of passing math, reading and overal passing
new_score_school_size_df['percent_pass_math'] = (new_score_school_size_df.total_passing_math / new_score_school_size_df.total_student)*100
new_score_school_size_df['percent_pass_read'] = (new_score_school_size_df.total_passing_read / new_score_school_size_df.total_student)*100
new_score_school_size_df['overall_passing'] = (new_score_school_size_df.percent_pass_math + new_score_school_size_df.percent_pass_read)/2


final_score_school_size_df = new_score_school_size_df[['school_size','avg_math','avg_read','percent_pass_math',
                                                               'percent_pass_read','overall_passing']]

final_score_school_size_df = final_score_school_size_df.reset_index(drop=True)
final_score_school_size_df = final_score_school_size_df.rename(columns = {'school_size':'School Size',
                                                                            'avg_math':'Average Math Score','avg_read':'Average Reading Score',
                                                                            'percent_pass_math':'% Passing Math','percent_pass_read':'% Passing Reading',
                                                                            'overall_passing':'% Overall Passing Rate'})
final_score_school_size_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Size</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Small (&lt;1000)</td>
      <td>83.828654</td>
      <td>83.974082</td>
      <td>93.952484</td>
      <td>96.040317</td>
      <td>94.996400</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Medium (1000-2000)</td>
      <td>83.372682</td>
      <td>83.867989</td>
      <td>93.616522</td>
      <td>96.773058</td>
      <td>95.194790</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Large (2000-5000)</td>
      <td>77.477597</td>
      <td>81.198674</td>
      <td>68.652380</td>
      <td>82.125158</td>
      <td>75.388769</td>
    </tr>
  </tbody>
</table>
</div>



# Scores by School Type:


```python
#create new data set for school per size
score_school_type_df = merge_student_school_df[['school_type','name','reading_score','math_score']]
#reset index
score_school_type_df = score_school_type_df.reset_index(drop=True)


#create varaiables for % of passing math and reading calculation
passing_math_df3 = score_school_type_df[score_school_type_df['math_score'] >= 70]
passing_read_df3 = score_school_type_df[score_school_type_df['reading_score'] >= 70]

#count passing math and reading per each spending range
passing_math_grouped3 = passing_math_df3.groupby('school_type')['name'].count()
passing_read_grouped3 = passing_read_df3.groupby('school_type')['name'].count()

#calculate average of math and reading scores for each spending spending range
avg_score_grouped_math3 = score_school_type_df.groupby('school_type')['math_score'].mean()
avg_score_grouped_read3 = score_school_type_df.groupby('school_type')['reading_score'].mean()
#count total student per spending range
total_student_grouped3 = score_school_type_df.groupby('school_type')['name'].count()

#create new data set from the calculation above
new_score_school_type_df = pd.DataFrame({'avg_read': avg_score_grouped_read3,'avg_math':avg_score_grouped_math3,'total_student':total_student_grouped3
                              , 'total_passing_math':passing_math_grouped3,'total_passing_read':passing_read_grouped3})

#reset index
new_score_school_type_df = new_score_school_type_df.reset_index()

#calculate percentage of passing math, reading and overal passing
new_score_school_type_df['percent_pass_math'] = (new_score_school_type_df.total_passing_math / new_score_school_type_df.total_student)*100
new_score_school_type_df['percent_pass_read'] = (new_score_school_type_df.total_passing_read / new_score_school_type_df.total_student)*100
new_score_school_type_df['overall_passing'] = (new_score_school_type_df.percent_pass_math + new_score_school_type_df.percent_pass_read)/2


final_score_school_type_df = new_score_school_type_df[['school_type','avg_math','avg_read','percent_pass_math',
                                                               'percent_pass_read','overall_passing']]

final_score_school_type_df = final_score_school_type_df.reset_index(drop=True)
final_score_school_type_df = final_score_school_type_df.rename(columns = {'school_type':'School Type',
                                                                            'avg_math':'Average Math Score','avg_read':'Average Reading Score',
                                                                            'percent_pass_math':'% Passing Math','percent_pass_read':'% Passing Reading',
                                                                            'overall_passing':'% Overall Passing Rate'})
final_score_school_type_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Charter</td>
      <td>83.406183</td>
      <td>83.902821</td>
      <td>93.701821</td>
      <td>96.645891</td>
      <td>95.173856</td>
    </tr>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>76.987026</td>
      <td>80.962485</td>
      <td>66.518387</td>
      <td>80.905249</td>
      <td>73.711818</td>
    </tr>
  </tbody>
</table>
</div>


