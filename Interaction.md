    data_folder <- "factorial_design"
    data_from <- "Interaction"
    file_name <- "interaction - testing.xlsx"

    file_path <- here(data_folder, data_from, file_name)
    interaction_data_table <- read_excel(file_path,
                        sheet = "Sheet1",
                        range = "B1:Q436") %>%
      clean_names()  %>%
      data.table()
    #relevant rows & columns
    interaction_data <- interaction_data_table[c(5, 11, 18, 24, 26:29, 32:37, 40, 41, 43:45, 47, 49:54, 56:67, 71:81, 83:435),-c("jaw_tests", "comment")] 

    #View(interaction_data)
      
    summary(interaction_data)

    ##    journal               year          issue           author         
    ##  Length:402         Min.   :2019   Min.   :   1.0   Length:402        
    ##  Class :character   1st Qu.:2019   1st Qu.:   2.0   Class :character  
    ##  Mode  :character   Median :2019   Median :   5.0   Mode  :character  
    ##                     Mean   :2019   Mean   : 229.2                     
    ##                     3rd Qu.:2019   3rd Qu.:   9.0                     
    ##                     Max.   :2019   Max.   :7789.0                     
    ##                     NA's   :2      NA's   :27                         
    ##     title              fig_id          data_availability     design         
    ##  Length:402         Length:402         Length:402         Length:402        
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##  sample_size          analysis           t_tests             tests          
    ##  Length:402         Length:402         Length:402         Length:402        
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ##    post_hoc         ixn_reported      
    ##  Length:402         Length:402        
    ##  Class :character   Class :character  
    ##  Mode  :character   Mode  :character  
    ##                                       
    ##                                       
    ##                                       
    ## 

    table(interaction_data$data_availability)

    ## 
    ##  no yes 
    ## 345  57

    table(interaction_data$design)

    ## 
    ##       2x2     2x2x2 2x2x2 + 2       3x2    3x2 +1     3x2+1     3x2x2       3x3 
    ##       236         9         1        96         1         1         5         6 
    ##       4x2       4x3     4x3x2       4x4       5x2       6x4 
    ##        25        15         1         1         2         1

    table(interaction_data$analysis)

    ## 
    ## factorial      flat 
    ##        56       305

    table(interaction_data$t_tests)

    ## 
    ## combined separate 
    ##      146      221

    table(interaction_data$tests)

    ## 
    ##                                  anova                                 duncan 
    ##                                     10                                      2 
    ##                                dunnett generalized linear mixed-effects model 
    ##                                      1                                      1 
    ##                         kruskal-wallis             linear mixed-effects model 
    ##                                      4                                      1 
    ##                      mixed model anova                            moderated t 
    ##                                      2                                      2 
    ##                                    mww                          one-way anova 
    ##                                     15                                     91 
    ##        one-way anova repeated measures          one-way anova, kruskal-wallis 
    ##                                      2                                      2 
    ##                   one-way anova, tukey                                      t 
    ##                                      5                                    166 
    ##                                  tukey                          two-way anova 
    ##                                      2                                     50 
    ##        two-way anova repeated measures                                welch t 
    ##                                      3                                      5 
    ##                   wilcoxon signed-rank 
    ##                                      4

    table(interaction_data$post_hoc)

    ## 
    ##     bonferroni         duncan           dunn        dunnett     holm-sidak 
    ##             36              2              2              4              2 
    ##        scheffe          sidak sidak, dunnett              t          tukey 
    ##              2             17              1              2             51

    table(interaction_data$ixn_reported)

    ## 
    ##  no yes 
    ## 361  11

## Example 1: Estimation of a treatment effect relative to a control effect (“Something different”)

## Example 2: Estimation of the effect of background condition on an effect (“it depends”)

## Example 3: Estimation of synergy (“More than the sum of the parts”)

    journal_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    data_availability_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = data_availability,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    data_availability_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    design_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    analysis_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    png( "mygraph1.png", width = 864, height = 864)
    p <- ggplot(interaction_data, aes(ixn_reported, analysis)) + geom_point()

    p + facet_grid(vars(data_availability), vars(design))

    png( "mygraph2.png", width = 2304, height = 1536)
    p <- ggplot(interaction_data, aes(ixn_reported, analysis)) + geom_point()

    p + facet_grid(vars(design), vars(journal))

    tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    t_tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    post_hoc_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-12-1.png)

    journal_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_data_availability

![](Interaction_files/figure-markdown_strict/unnamed-chunk-13-1.png)

    journal_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    journal_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    journal_vs_analysis_c <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = design)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_c

![](Interaction_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    journal_vs_design <- ggplot(data = interaction_data,
                                 aes(x =  design,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_design

![](Interaction_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    journal_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-18-1.png)

    journal_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-19-1.png)

    journal_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-20-1.png)

    design_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-21-1.png)

    analysis_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    post_hoc_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-24-1.png)

    t_tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    design_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-26-1.png)

    design_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-27-1.png)

    design_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-28-1.png)

    design_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-29-1.png)

    design_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    post_hoc_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-32-1.png)

    t_tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-33-1.png)

    tests_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-34-1.png)

    post_hoc_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-35-1.png)
