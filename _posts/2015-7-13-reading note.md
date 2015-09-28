---
layout: post

title: "Machine Learning for Hackers Ch02"

description: ""

category: English
published: true

tags: 
- R 
- ggplot2 
- ReadingNote
- Machine Learning


---


## Exploration versus Confirmation
-  Whenever you work with data, it's helpful to imagine breaking up your analysis into two compltetely separate parts: exploration and confirmation.
- In John Tukey's mind the exploratory steps in data analysis involve using summarytables and basic visualization to search forhidden patterns in your data.
- Comfirmatory data analysis usually employs two tools: <br/> 
  - Testing a formal modelof the pattern that you think you've found on a new data set that you didn't use to find the pattern.(交叉验证)
  - Using probability theory to test whether the pattern you've found in you original data set could reasonably have been produced by chance. （假设检验）

## Exporatory Data Visualization
  
    library('ggplot2')
    # Load the data from scratch for purity.
    data.file <- file.path('data', '01_heights_weights_genders.csv')
    heights.weights <- read.csv(data.file, header = TRUE, sep = ',')
    # Visualize how gender depends on height and weight.
    ggplot(heights.weights, aes(x = Height, y = Weight)) +
    geom_point(aes(color = Gender, alpha = 0.25)) +
    scale_alpha(guide = "none") + 
    scale_color_manual(values = c("Male" = "black", "Female" = "gray")) +
    theme_bw()
    # An alternative using bright colors.
    ggplot(heights.weights, aes(x = Height, y = Weight, color = Gender)) +
    geom_point()

The plot is :<img src="http://ww1.sinaimg.cn/large/82a7fa50gw1eu16v54h1aj20hf094t9g.jpg"> <img/>
