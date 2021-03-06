---
layout : post
title : Part 1 - Plotting Using Seaborn - Violin, Box and Line Plot
categories: [python, visualisation]
tags: [python, seaborn, matplotlib, pandas, plot, violin plot, box plot, line plot]
---

---
### Introduction and Data preparation
Please follow the folloing links regarding data preparation and previous posts to follow along -

* <b> For Data Preparation </b>  - [Part 0 - Plotting Using Seaborn - Data Preparation](/python/visualisation/2019/08/20/Plotting-Seaborn-Data-Preparation.html){:target="_blank"}

---

### Violin Plot showing distribution of score for each track by complexity

```python
plot1 = sns.catplot(y="Track",
                 x="Score",
                 data=test_scores, 
                 jitter=True, 
                 height=9, 
                 aspect=0.5,
                 kind = "violin", 
                 col = "Complexity",
                 cut = 0,
                 col_order = ["Easy","Medium","Difficult"],
                 palette = sns.color_palette(["#a05195","#d45087","#f95d6a"])) \
                .set_ylabels(fontsize=25) \
                .set_xlabels(fontsize = 25, label = "Score") \
                .set_xticklabels(fontsize=20) \
                .set_yticklabels(fontsize=20) \
                .set_titles(size = 20) 

plot1.fig.subplots_adjust(top=0.8)
plot1.fig.suptitle('Violin Plot showing distribution of score for each track by complexity',size = 30)
#plot1.savefig("Violin Plot -1.png",dpi=100,bbox_inches='tight')
```

<!--break-->



![Violin Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_4_1.png "Violin Plot showing distribution of score for each track by complexity")

---
### Box Plot showing distribution of average score percentage for each track by complexity

```python
plot2 = sns.catplot(y="Track", 
                 x="Percent", 
                 data=test_scores, 
                 height=9, 
                 aspect=0.5,
                 kind = "box", 
                 col = "Complexity",
                 col_order = ["Easy","Medium","Difficult"],
                 palette = sns.color_palette(["#a05195","#d45087","#f95d6a"])) \
                .set_ylabels(fontsize=25) \
                .set_xlabels(fontsize = 25, label = "Score %") \
                .set_xticklabels(fontsize=20) \
                .set_yticklabels(fontsize=20) \
                .set_titles(size = 20)
plot2.fig.subplots_adjust(top=0.8)
plot2.fig.suptitle('Box Plot showing distribution of average score percentage for each track by complexity', size = 30)
#plot2.savefig("Box Plot -1.png",dpi=100,bbox_inches='tight')
```


![Box Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_5_1.png "Box Plot showing distribution of average score percentage for each track by complexity")

---
### Violin Plot showing distribution of score for each designation by complexity

```python
plot3 = sns.catplot(y="Designation", 
                 x="Score", 
                 data=test_scores, 
                 jitter=True, 
                 height=9, 
                 aspect=0.5,
                 kind = "violin", 
                 cut = 0,
                 col = "Complexity",
                 col_order = ["Easy","Medium","Difficult"],
                 order = ['Associate','Lead','Manager', 'Consultant','Associate Director and above'],
                 palette = sns.color_palette(["#a05195","#d45087","#f95d6a","#ff7c43","#ffa600"])) \
                .set_ylabels(fontsize=25) \
                .set_xlabels(fontsize = 25, label = "Score") \
                .set_xticklabels(fontsize=20) \
                .set_yticklabels(fontsize=20) \
                .set_titles(size = 20)

plot3.fig.subplots_adjust(top=0.8)
plot3.fig.suptitle('Violin Plot showing distribution of score for each designation by complexity', size = 30)
#plot3.savefig("Violin Plot -2.png",dpi=100,bbox_inches='tight')
```

![Violin Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_6_1.png "'Violin Plot showing distribution of score for each designation by complexity'")

---

### Box Plot showing distribution of average score percentage for each designation by complexity

```python
plot4 = sns.catplot(y="Designation", 
                 x="Percent", 
                 data=test_scores, 
                 height=9, 
                 aspect=0.5,
                 kind = "box",
                 col = "Complexity",
                 col_order = ["Easy","Medium","Difficult"],
                 order = ['Lead', 'Associate','Manager', 'Consultant','Associate Director and above'],
                 palette = sns.color_palette(["#a05195","#d45087","#f95d6a","#ff7c43","#ffa600"])) \
                .set_ylabels(fontsize=25) \
                .set_xlabels(fontsize = 25, label = "Score %") \
                .set_xticklabels(fontsize=20) \
                .set_yticklabels(fontsize=20) \
                .set_titles(size = 20)

plot4.fig.subplots_adjust(top=0.8)
plot4.fig.suptitle('Box Plot showing distribution of average score percentage for each designation by complexity', size = 30)

#plot4.savefig("Box Plot -2.png",dpi=100,bbox_inches='tight')
```

![Box Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_7_1.png "Box Plot showing distribution of average score percentage for each designation by complexity")

---

### Trend in average score for each designation by complexity


```python

# Score by Designation
test_scores_Designtion = test_scores.groupby(['Complexity','Designation'])[['Score','maximum_score']].mean().reset_index()
test_scores_Designtion['Percent'] = round(test_scores_Designtion['Score']/test_scores_Designtion['maximum_score'],2)*100

# Plotting
plot5 = sns.catplot(x="Designation", 
                 y="Score", 
                 hue="Complexity",
                 kind="point", 
                 data=test_scores_Designtion, 
                 height=6, 
                 aspect =1.5,
                 order = ['Associate','Lead', 'Manager', 'Consultant','Associate Director and above'],
                 hue_order = ['Easy','Medium','Difficult'],
                 col_order = ['Monday', 'Tuesday','Wednesday','Thursday','Friday','Saturday', 'Sunday'],
                 palette = sns.color_palette(["#a05195","#f95d6a","#ffa600"])) \
                .set_ylabels(fontsize=25, label = "Average Score") \
                .set_xlabels(fontsize = 20) \
                .set_xticklabels(fontsize=10) \
                .set_yticklabels(fontsize=15) \
                .set_titles(size = 17)
plt.grid()
plot5.fig.subplots_adjust(top=0.9)
plot5.fig.suptitle('Trend in average score for each designation by complexity', size = 20)
#plot5.savefig("Line Plot.png",dpi=200,bbox_inches='tight')
```


![Line Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_9_1.png "Trend in average score for each designation by complexity")

---

### Trend in average score percentage for each designation by complexity

```python
plot6 = sns.catplot(x="Designation", 
                 y="Percent", 
                 hue="Complexity",
                 kind="point", 
                 data=test_scores_Designtion, 
                 height=6, 
                 aspect =1.5,
                 order = ['Associate','Lead', 'Manager', 'Consultant','Associate Director and above'],
                 hue_order = ['Easy','Medium','Difficult'],
                 col_order = ['Monday', 'Tuesday','Wednesday','Thursday','Friday','Saturday', 'Sunday'],
                 palette = sns.color_palette(["#a05195","#f95d6a","#ffa600"])) \
                .set_ylabels(fontsize=25, label = "Average Score %") \
                .set_xlabels(fontsize = 20) \
                .set_xticklabels(fontsize=10) \
                .set_yticklabels(fontsize=15) \
                .set_titles(size = 17)
plt.grid()
plot6.fig.subplots_adjust(top=0.9)
plot6.fig.suptitle('Trend in average score percentage for each designation by complexity', size = 20)
#plot6.savefig("Line Plot 2.png",dpi=200,bbox_inches='tight')
```
![Line Plot](/static/img/posts/python/2019-08-21-Plotting-Seaborn-Violin-Box-Line/output_10_1.png "Trend in average score percentage for each designation by complexity")       

---
<b> Jupyter Notebook Link </b>   - [Part 1 - Plotting Using Seaborn - Violin, Box and Line Plot](https://nbviewer.jupyter.org/github/aakashkh/Sample-Jupyter-Notebooks/blob/master/Visualization%20With%20Seaborn/Basic.ipynb){:target="_blank"}

---