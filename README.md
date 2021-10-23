# School District Analysis

## Project Overview
Perform analysis on a school district's test scores to uncover trends that can assist with budget allotment and prioritization for future use.

## Resources
Softwares: Python 3.7.6, Anaconda 4.10.1, Jupyter Notebook
Data Sources: schools_complete.csv, students_complete.csv, clean_students_complete.csv

## Results

- There are 15 high schools included in the School District with a total of 39,170 students.  After suspected academic dishonesty, math and reading scores for the ninth graders at Thomas High School were replaced with nulls to uphold the state's testing standards. After removing these results we have rerun the district and per school summaries to compare the impact of this change. 

```
thomas_ninth_count_df = school_data_complete_df.loc[(school_data_complete_df["school_name"]=="Thomas High School") & (school_data_complete_df["grade"]=="9th")]
thomas_ninth_count = thomas_ninth_count_df["Student ID"].count()
student_count = school_data_complete_df["Student ID"].count()
clean_student_count = student_count - thomas_ninth_count
```

   ### How the district summary is affected
    The district summary had only a few very small changes to scores after the Thomas High School modifications were made.  The overall passing percentage dropped from *65.1% to 64.9%*, which was the most significant change. 

![district_summary original](https://user-images.githubusercontent.com/90050622/138536649-ca20c033-f422-4b09-8daf-bfc89197576b.PNG)
<img width="733" alt="district_summary THS Updates" src="https://user-images.githubusercontent.com/90050622/138536655-484b6a24-f093-4ac5-94a9-b87b660f592f.PNG">

   ### How the school summary is affected
    The only changes made from the original dataset were to Thomas High School ninth graders, so when looking at the school summary that is the only area with any altered scores.  With that said the most major change was to the overall passing percentage, but even that only dropped *0.31%*.
    
<img width="898" alt="per_school_summary_THS original" src="https://user-images.githubusercontent.com/90050622/138536983-8e388c80-a7e3-40e2-8f90-da1489a51f80.PNG">
<img width="887" alt="per_school_summary_THS THS Updates" src="https://user-images.githubusercontent.com/90050622/138536992-2ae5df78-ac6e-499a-a62b-3d8248552d3d.PNG">

  ### How replacing ninth graders' math and reading scores affects Thomas High School's performance relative to the other schools
    Overall replacing the ninth graders' math and reading scores did not have an impact on their school's performance relative to the other schools.  In both the original data and the modified data Thomas High School was the number two school for overall passing percentage.  While their was a very slight change to their overall passing percentage, it was not significant enough to change their rank. 
    
  ### How replacing ninth graders math and reading scores affects the following:
        - Math and reading scores by grade
          - The math and reading scores for the ninth graders at Thomas High School were completely removed, so there is no longer a comparison between their grade and the other schools ninth graders.  No other data was altered, so the schools comparison otherwise remains the same. 
        - Scores by school spending
          - Thomas High School fell into the spending category of $630 - $644, so that is the only category we needed to look for changes in.  After the Thomas High School               modifications the scores for this particular category did go down slightly, but only by a few decimal points here and there.  
        - Scores by school size
          - Thomas High School fell into the size category of medium.  Similar to the school spending changes, the medium size category did see a slight drop in scores, but again just decimals, and not enough to even change the whole number when rounding.
        - Scores by school type 
          - Thomas High School is a charter school.  Comparing the scores from the original data to the updated there are only very minor changes to the scores, once again only            being decimals.  

```
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
per_school_summary_df["School Size"] = pd.cut(per_school_summary_df["Total Students"], size_bins, labels=group_names)
size_math_scores = per_school_summary_df.groupby(["School Size"]).mean()["Average Math Score"]
size_reading_scores = per_school_summary_df.groupby(["School Size"]).mean()["Average Reading Score"]
size_passing_math = per_school_summary_df.groupby(["School Size"]).mean()["% Passing Math"]
size_passing_reading = per_school_summary_df.groupby(["School Size"]).mean()["% Passing Reading"]
size_overall_passing = per_school_summary_df.groupby(["School Size"]).mean()["% Overall Passing"]
size_summary_df = pd.DataFrame({
          "Average Math Score" : size_math_scores,
          "Average Reading Score": size_reading_scores,
          "% Passing Math": size_passing_math,
          "% Passing Reading": size_passing_reading,
          "% Overall Passing": size_overall_passing})
```

## Summary
When looking at the full district summary, removing the ninth grade reading and math scores from Thomas High School didn't have much of an impact.  Ninth graders at Thomas High School only make up about 1% of the student population for the district, so it's not a surprise to see such minor changes being made.  The reports that are most affected from this grade being removed are ones that analyze smaller sets of students, such as looking at Thomas High School individually. When looking at the overall passing percent for Thomas High School the original data showed the result of *90.94%*.  After removing the ninth graders from the data the overall passing percent changed to *90.63%*.  As you can see even this isn't a significant change, but nonetheless still a change.  The average reading score of students at Thomas was the only area that actually improved when removing the ninth graders.  This went from a score of *83.84 to 83.89*.  Additional changes to the Thomas High School reports include a change in average math score from *83.41 to 83.35*, and a percentage of students passing math change from *93.27% to 93.18%*. The data modification also changes the outlook of district wide grade-to-grade data, because now 1 of the 15 schools wouldn't have anything to add.  We can't even do a ninth grade comparison by schools like we can the other grades because of this. While the changes to the Thomas High School data may not seem significant from the numbers, I would advise the client to refrain from doing too much school-to-school comparison that includes THS, since we are making a variety of assumptions.   

![per_school_summary original](https://user-images.githubusercontent.com/90050622/138537241-3b287466-4d12-4160-921b-c24c2e101705.PNG)
![per_school_summary THS updates](https://user-images.githubusercontent.com/90050622/138537246-366f7f9b-c83b-4bcf-9239-8fb57825f64b.PNG)

