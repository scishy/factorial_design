    data_folder <- "factorial_design"
    data_from <- "Interaction"
    file_name <- "interaction - testing.xlsx"

    file_path <- here(data_folder, data_from, file_name)
    interaction_data_table <- read_excel(file_path,
                        sheet = "Sheet1",
                        range = "B1:Q436") %>%
      clean_names()  %>%
      data.table()
      interaction_data <- interaction_data_table[c(6, 12, 19, 25, 27:30, 33:39, 41, 42, 44:46, 48, 50:55, 57:68, 72:82, 84:436),-c("jaw_tests", "comment")] #relevant rows
      #na.omit() # danger!

    #View(interaction_data)

    #Import as csv

    #data_folder <- "factorial_design"

    #data_from <- "Interaction"
    #file_name <- "interaction - testing - Sheet1.csv"
    #file_path <- here(data_folder, data_from, file_name)

    #Import CSV file

    #interaction_data <- read.csv(file_path, header = TRUE, fileEncoding="UTF-8-BOM") %>%
    #  data.table() %>%
    #  clean_names()

    #View(interaction_data)

    journal_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-3-1.png)

    data_availability_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = data_availability,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    data_availability_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-4-1.png)

    design_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    analysis_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-6-1.png)

    tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-7-1.png)

    t_tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-8-1.png)

    post_hoc_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_ixn_reported

![](Interaction_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    journal_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_data_availability

![](Interaction_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    journal_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-11-1.png)

    journal_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-12-1.png)

    journal_vs_analysis_c <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = journal,
                                     color = design)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_analysis_c

![](Interaction_files/figure-markdown_strict/unnamed-chunk-13-1.png)

    journal_vs_design <- ggplot(data = interaction_data,
                                 aes(x =  design,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_design

![](Interaction_files/figure-markdown_strict/unnamed-chunk-14-1.png)

    journal_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-15-1.png)

    journal_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-16-1.png)

    journal_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = journal,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    journal_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-17-1.png)

    design_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-18-1.png)

    analysis_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = analysis,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    analysis_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-19-1.png)

    tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-20-1.png)

    post_hoc_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-21-1.png)

    t_tests_vs_data_availability <- ggplot(data = interaction_data,
                                 aes(x =  data_availability,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_data_availability 

![](Interaction_files/figure-markdown_strict/unnamed-chunk-22-1.png)

    design_vs_analysis_a <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_a

![](Interaction_files/figure-markdown_strict/unnamed-chunk-23-1.png)

    design_vs_analysis_b <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = design,
                                     color = tests)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_analysis_b

![](Interaction_files/figure-markdown_strict/unnamed-chunk-24-1.png)

    design_vs_tests <- ggplot(data = interaction_data,
                                 aes(x =  tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-25-1.png)

    design_vs_post_hoc <- ggplot(data = interaction_data,
                                 aes(x =  post_hoc,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_post_hoc

![](Interaction_files/figure-markdown_strict/unnamed-chunk-26-1.png)

    design_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = design,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    design_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-27-1.png)

    tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-28-1.png)

    post_hoc_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-29-1.png)

    t_tests_vs_analysis <- ggplot(data = interaction_data,
                                 aes(x =  analysis,
                                     y = t_tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    t_tests_vs_analysis

![](Interaction_files/figure-markdown_strict/unnamed-chunk-30-1.png)

    tests_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = tests,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    tests_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-31-1.png)

    post_hoc_vs_t_tests <- ggplot(data = interaction_data,
                                 aes(x =  t_tests,
                                     y = post_hoc,
                                     color = journal)) +
      geom_point(alpha = 0.5, show.legend = FALSE) +
      geom_jitter(width = 0.3, show.legend = FALSE) 

    post_hoc_vs_t_tests

![](Interaction_files/figure-markdown_strict/unnamed-chunk-32-1.png)
