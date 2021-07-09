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
      
    #summary(interaction_data)

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

    interaction_data_subset <- interaction_data[, c('journal', 'data_availability', 'design', 'analysis', 't_tests', 'tests', 'post_hoc', 'ixn_reported')]


    #Just in case
    #
    factor_columns <- c("journal", "data_availability", "design", "analysis", "t_tests", "tests", "post_hoc", "ixn_reported")
    interaction_data_asfactor <- interaction_data_subset
    interaction_data_asfactor[ , 
                 (factor_columns) := lapply(.SD, as.factor),
                 .SDcols = factor_columns]

    #playing with heatmaps just because

    interaction_1 <- ggplot(data = interaction_data, aes(x= data_availability, y = design  , fill = analysis)) +
      geom_raster()

    interaction_2 <- ggplot(data = interaction_data, aes(x= data_availability, y = ixn_reported  , fill = analysis)) +
      geom_raster()

    interaction_3 <- ggplot(data = interaction_data, aes(x= design, y = ixn_reported  , fill = analysis)) +
      geom_raster()

    interaction_4 <- ggplot(data = interaction_data, aes(x= tests, y = post_hoc  , fill = analysis)) +
      geom_raster()

    interaction_5 <- ggplot(data = interaction_data, aes(x= tests, y = ixn_reported  , fill = analysis)) +
      geom_raster()

    interaction_1

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    interaction_2

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-2.png)

    interaction_3

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-3.png)

    interaction_4

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-4.png)

    interaction_5

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-5.png)

    interaction_data_asnumeric <- interaction_data_asfactor
    interaction_data_asfactor[ , 
                 (factor_columns) := lapply(.SD, as.numeric),
                 .SDcols = factor_columns] %>%
      data.table()

    ##      journal data_availability design analysis t_tests tests post_hoc
    ##   1:       6                 1      1        2       2     5       NA
    ##   2:       6                 1      1        2       2     5       NA
    ##   3:      33                 1      9        2       1    10       NA
    ##   4:      33                 1      1        2       1    10       NA
    ##   5:      16                 1      1        2       2    14       NA
    ##  ---                                                                 
    ## 398:      15                 1      1        2       2    14       NA
    ## 399:      15                 1      1        2       2    NA       NA
    ## 400:      15                 1      1        2       2    NA       NA
    ## 401:      15                 1      1        2       2    14       NA
    ## 402:      15                 1      1        2       2    14       NA
    ##      ixn_reported
    ##   1:            1
    ##   2:            1
    ##   3:            1
    ##   4:            1
    ##   5:            1
    ##  ---             
    ## 398:            1
    ## 399:            1
    ## 400:            1
    ## 401:            1
    ## 402:            1

    interaction_data_asmatrix <- as.matrix(interaction_data_asnumeric)
    heatmap(interaction_data_asmatrix)

![](Interaction_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    journal_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    data_availability_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = data_availability,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    data_availability_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    design_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    analysis_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-10-1.png)

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

![](Interaction_files/figure-markdown_strict/unnamed-chunk-13-1.png)

    t_tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    post_hoc_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    journal_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_data_availability

![](Interaction_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    journal_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    journal_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-18-1.png)

    journal_vs_analysis_c <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = design)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_c

![](Interaction_files/figure-markdown_strict/unnamed-chunk-19-1.png)

    journal_vs_design <- ggplot(data = interaction_data,
                                 aes(x =  design,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_design

![](Interaction_files/figure-markdown_strict/unnamed-chunk-20-1.png)

    journal_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-21-1.png)

    journal_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    journal_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    design_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-24-1.png)

    analysis_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-26-1.png)

    post_hoc_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-27-1.png)

    t_tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-28-1.png)

    design_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-29-1.png)

    design_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    design_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    design_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-32-1.png)

    design_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-33-1.png)

    tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-34-1.png)

    post_hoc_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-35-1.png)

    t_tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-36-1.png)

    tests_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-37-1.png)

    post_hoc_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-38-1.png)
