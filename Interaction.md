---
title: "Interaction"
author: "Shylo Burrell"
date: "08/10/2021"
output: 
  html_document:
    keep_md: TRUE
---




```r
data_folder <- "factorial_design"
data_from <- "Interaction"
file_name <- "interaction - testing.xlsx"
```



```r
#import data and create data table

file_path <- here(data_folder, data_from, file_name)
interaction_data_raw <- read_excel(file_path,
                                     sheet = "Sheet1",
                                     range = "A1:Z476") %>%
  clean_names()  %>%
  data.table()
#View(interaction_data_table)
#relevant rows & columns
interaction_data <- interaction_data_raw[
  include == "yes", 
  !c(
    "include","doi", "quartile", "year", "issue", "jaw_tests", "jaw", "smb", "comment", "report",  "synergy_used", "antagonism_used",  "ixn_to_support_syn_ant"
    )]

#add new column combining all tests performed
interaction_data$all_tests <- paste(interaction_data$tests, interaction_data$post_hoc, sep = ",")

#View(interaction_data)
```



```r
design_summary <- table(interaction_data$design)
kable(design_summary, col.names = c("design", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> design </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> 2x2 </td>
   <td style="text-align:right;"> 249 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2x2x2 </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 2x2x2+1 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3x2 </td>
   <td style="text-align:right;"> 103 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3x2+1 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3x2x2 </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3x3 </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4x2 </td>
   <td style="text-align:right;"> 26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4x3 </td>
   <td style="text-align:right;"> 16 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4x3x2 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 4x4 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5x2 </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 6x4 </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
</tbody>
</table>

```r
data_availability_summary <- table(interaction_data$data_availability)
kable(data_availability_summary, col.names = c("data_availability", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> data_availability </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> no </td>
   <td style="text-align:right;"> 365 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> yes </td>
   <td style="text-align:right;"> 57 </td>
  </tr>
</tbody>
</table>

```r
analysis_summary <- table(interaction_data$analysis)
kable(analysis_summary, col.names = c("analysis", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> analysis </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> factorial </td>
   <td style="text-align:right;"> 54 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> flat </td>
   <td style="text-align:right;"> 325 </td>
  </tr>
</tbody>
</table>

```r
t_tests_summary <- table(interaction_data$t_tests)
kable(t_tests_summary, col.names = c("t_tests", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> t_tests </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> combined </td>
   <td style="text-align:right;"> 160 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> separate </td>
   <td style="text-align:right;"> 219 </td>
  </tr>
</tbody>
</table>

```r
tests_summary <- table(interaction_data$tests)
kable(tests_summary, col.names = c("tests", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> tests </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> anova </td>
   <td style="text-align:right;"> 21 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> duncan </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dunnett </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> generalized linear mixed-effects model </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> kruskal-wallis </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> linear mixed-effects model </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> moderated t </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> mww </td>
   <td style="text-align:right;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova </td>
   <td style="text-align:right;"> 105 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova repeated measures </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova, kruskal-wallis </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova, tukey </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> paired-sample t test </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> t </td>
   <td style="text-align:right;"> 173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> tukey </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova </td>
   <td style="text-align:right;"> 50 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova repeated measures </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> welch t </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
</tbody>
</table>

```r
all_tests_summary <- table(interaction_data$all_tests)
kable(all_tests_summary, col.names = c("all_tests", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> all_tests </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> anova,bonferroni </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> anova,missing </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> anova,NA </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> anova,sidak </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> anova,snk </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> anova,tukey </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> duncan,NA </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dunnett,NA </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> generalized linear mixed-effects model,NA </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> kruskal-wallis,dunn </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> kruskal-wallis,missing </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> linear mixed-effects model,NA </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> moderated t,NA </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> mww,NA </td>
   <td style="text-align:right;"> 19 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> NA,NA </td>
   <td style="text-align:right;"> 28 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova repeated measures,missing </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova, kruskal-wallis,NA </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova, tukey,tukey </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,bonferroni </td>
   <td style="text-align:right;"> 27 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,duncan </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,dunnett </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,missing </td>
   <td style="text-align:right;"> 26 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,scheffe </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,sidak </td>
   <td style="text-align:right;"> 6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,snk </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,t </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> one-way anova,tukey </td>
   <td style="text-align:right;"> 36 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> paired-sample t test,NA </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> t,NA </td>
   <td style="text-align:right;"> 173 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> tukey,NA </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova repeated measures,bonferroni </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova repeated measures,sidak </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,bonferroni </td>
   <td style="text-align:right;"> 10 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,dunnett </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,holm-sidak </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,missing </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,sidak </td>
   <td style="text-align:right;"> 8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,sidak's multiple comparison </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,sidak, dunnett </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> two-way anova,tukey </td>
   <td style="text-align:right;"> 13 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> welch t,NA </td>
   <td style="text-align:right;"> 3 </td>
  </tr>
</tbody>
</table>

```r
post_hoc_summary <- table(interaction_data$post_hoc)
kable(post_hoc_summary, col.names = c("post_hoc", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> post_hoc </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> bonferroni </td>
   <td style="text-align:right;"> 41 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> duncan </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dunn </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> dunnett </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> holm-sidak </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> missing </td>
   <td style="text-align:right;"> 52 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> scheffe </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sidak </td>
   <td style="text-align:right;"> 17 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sidak's multiple comparison </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sidak, dunnett </td>
   <td style="text-align:right;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> snk </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> t </td>
   <td style="text-align:right;"> 2 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> tukey </td>
   <td style="text-align:right;"> 55 </td>
  </tr>
</tbody>
</table>

```r
ixn_reported_summary <- table(interaction_data$ixn_reported)
kable(ixn_reported_summary, col.names = c("ixn_reported", "freq")) %>%
  kable_styling()
```

<table class="table" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> ixn_reported </th>
   <th style="text-align:right;"> freq </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> no </td>
   <td style="text-align:right;"> 378 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> yes </td>
   <td style="text-align:right;"> 12 </td>
  </tr>
</tbody>
</table>

### Pairwise comparisons

design by journal

```r
journal_vs_design <- ggplot(data = interaction_data,
                            aes(x =  design,
                                y = journal,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1),
        axis.text.y = element_blank())

journal_vs_design
```

![](Interaction_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

data availability by journal

```r
journal_vs_data_availability <- ggplot(data = interaction_data,
                                       aes(x =  data_availability,
                                           y = journal,
                                           color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
    theme(axis.text.y = element_blank())

journal_vs_data_availability
```

![](Interaction_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


journal vs analysis

```r
journal_vs_analysis <- ggplot(data = interaction_data,
                                aes(x =  analysis,
                                    y = journal,
                                    color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
    theme(axis.text.y = element_blank())

journal_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-6-1.png)<!-- -->



t tests by journal

```r
journal_vs_t_tests <- ggplot(data = interaction_data,
                             aes(x =  t_tests,
                                 y = journal,
                                 color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.y = element_blank())

journal_vs_t_tests
```

![](Interaction_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

tests by journal

```r
journal_vs_tests <- ggplot(data = interaction_data,
                           aes(x =  tests,
                               y = journal,
                               color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  #theme(axis.text.x = element_text(angle = 90, hjust = 1)) #to view test names
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank())

journal_vs_tests
```

![](Interaction_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

post hoc by journal

```r
journal_vs_post_hoc <- ggplot(data = interaction_data,
                              aes(x =  post_hoc,
                                  y = journal,
                                  color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1),
        axis.text.y = element_blank())

journal_vs_post_hoc
```

![](Interaction_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

all tests by journal

```r
journal_vs_all_tests <- ggplot(data = interaction_data,
                           aes(x =  all_tests,
                               y = journal,
                               color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  #theme(axis.text.x = element_text(angle = 90, hjust = 1)) #to view test names
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank())

journal_vs_all_tests
```

![](Interaction_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

interaction by journal

```r
journal_vs_ixn_reported <- ggplot(data = interaction_data,
                                  aes(x =  ixn_reported,
                                      y = journal,
                                      color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.y = element_blank())

journal_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

design by rank

```r
rank_vs_design <- ggplot(data = interaction_data,
                                  aes(x =  design,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

rank_vs_design
```

![](Interaction_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


data availability by rank

```r
rank_vs_data_availability <- ggplot(data = interaction_data,
                                  aes(x = data_availability,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

rank_vs_data_availability
```

![](Interaction_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

analysis by rank

```r
rank_vs_analysis <- ggplot(data = interaction_data,
                                  aes(x = analysis,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

rank_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

t tests by rank

```r
rank_vs_t_tests <- ggplot(data = interaction_data,
                                  aes(x = t_tests,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

rank_vs_t_tests
```

![](Interaction_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

tests by rank

```r
rank_vs_tests <- ggplot(data = interaction_data,
                                  aes(x = tests,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

rank_vs_tests
```

![](Interaction_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

post hoc by rank

```r
rank_vs_post_hoc <- ggplot(data = interaction_data,
                                  aes(x = post_hoc,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

rank_vs_post_hoc
```

![](Interaction_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

all tests by rank

```r
rank_vs_all_tests <- ggplot(data = interaction_data,
                                  aes(x = all_tests,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) +
  theme(axis.text.x = element_blank())
#axis.text.x = element_text(angle = 90, hjust = 1))

rank_vs_all_tests
```

![](Interaction_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

interaction reported by rank

```r
rank_vs_ixn_reported<- ggplot(data = interaction_data,
                                  aes(x = ixn_reported,
                                      y = rank,
                                      color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

rank_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

design vs data availability (color by journal)

```r
design_vs_data_availability_a <- ggplot(data = interaction_data,
                                      aes(x =  data_availability,
                                          y = design,
                                          color = journal)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

design_vs_data_availability_a 
```

![](Interaction_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

design vs data availability (color by rank)

```r
design_vs_data_availability <- ggplot(data = interaction_data,
                                      aes(x =  data_availability,
                                          y = design,
                                          color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

design_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

design vs analysis

```r
design_vs_analysis_a <- ggplot(data = interaction_data,
                               aes(x =  analysis,
                                   y = design,
                                   color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

design_vs_analysis_a
```

![](Interaction_files/figure-html/unnamed-chunk-22-1.png)<!-- -->

design vs analysis (color by tests)

```r
design_vs_analysis_b <- ggplot(data = interaction_data,
                               aes(x =  analysis,
                                   y = design,
                                   color = tests)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = FALSE) 

design_vs_analysis_b
```

![](Interaction_files/figure-html/unnamed-chunk-23-1.png)<!-- -->

design vs t tests

```r
design_vs_t_tests <- ggplot(data = interaction_data,
                            aes(x =  t_tests,
                                y = design,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

design_vs_t_tests
```

![](Interaction_files/figure-html/unnamed-chunk-24-1.png)<!-- -->


design vs tests (color by rank)

```r
design_vs_tests <- ggplot(data = interaction_data,
                            aes(x = tests,
                                y = design,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

design_vs_tests
```

![](Interaction_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

design vs post hoc

```r
design_vs_post_hoc <- ggplot(data = interaction_data,
                             aes(x =  post_hoc,
                                 y = design,
                                 color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

design_vs_post_hoc
```

![](Interaction_files/figure-html/unnamed-chunk-26-1.png)<!-- -->
design vs all tests (color by rank)

```r
design_vs_all_tests <- ggplot(data = interaction_data,
                            aes(x = all_tests,
                                y = design,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_blank())
    #axis.text.x = element_text(angle = 90, hjust = 1))

design_vs_all_tests
```

![](Interaction_files/figure-html/unnamed-chunk-27-1.png)<!-- -->

design vs interaction reported


```r
design_vs_ixn_reported <- ggplot(data = interaction_data,
                                 aes(x =  ixn_reported,
                                     y = design,
                                     color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

design_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-28-1.png)<!-- -->


data availability by analysis

```r
analysis_vs_data_availability <- ggplot(data = interaction_data,
                                        aes(x =  data_availability,
                                            y = analysis,
                                            color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

analysis_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-29-1.png)<!-- -->

data availability by t tests

```r
t_tests_vs_data_availability <- ggplot(data = interaction_data,
                                       aes(x =  data_availability,
                                           y = t_tests,
                                           color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

t_tests_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-30-1.png)<!-- -->

data availability by tests

```r
tests_vs_data_availability <- ggplot(data = interaction_data,
                                     aes(x =  data_availability,
                                         y = tests,
                                         color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

tests_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-31-1.png)<!-- -->

data availability by post hoc

```r
post_hoc_vs_data_availability <- ggplot(data = interaction_data,
                                        aes(x =  data_availability,
                                            y = post_hoc,
                                            color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

post_hoc_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

data availability by all tests

```r
all_tests_vs_data_availability <- ggplot(data = interaction_data,
                                     aes(x =  data_availability,
                                         y = all_tests,
                                         color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

all_tests_vs_data_availability 
```

![](Interaction_files/figure-html/unnamed-chunk-33-1.png)<!-- -->

data availability vs interaction reported

```r
data_availability_vs_ixn_reported <- ggplot(data = interaction_data,
                                            aes(x =  ixn_reported,
                                                y = data_availability,
                                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

data_availability_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-34-1.png)<!-- -->

analysis by t tests

```r
t_tests_vs_analysis <- ggplot(data = interaction_data,
                              aes(x =  analysis,
                                  y = t_tests,
                                  color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

t_tests_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-35-1.png)<!-- -->

analysis by tests

```r
tests_vs_analysis <- ggplot(data = interaction_data,
                            aes(x =  analysis,
                                y = tests,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

tests_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-36-1.png)<!-- -->

analysis by post hoc

```r
post_hoc_vs_analysis <- ggplot(data = interaction_data,
                               aes(x =  analysis,
                                   y = post_hoc,
                                   color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

post_hoc_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-37-1.png)<!-- -->

analysis by all tests

```r
all_tests_vs_analysis <- ggplot(data = interaction_data,
                            aes(x =  analysis,
                                y = all_tests,
                                color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

all_tests_vs_analysis
```

![](Interaction_files/figure-html/unnamed-chunk-38-1.png)<!-- -->


analysis vs interaction reported

```r
analysis_vs_ixn_reported <- ggplot(data = interaction_data,
                                   aes(x =  ixn_reported,
                                       y = analysis,
                                       color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

analysis_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-39-1.png)<!-- -->

t tests vs tests

```r
tests_vs_t_tests <- ggplot(data = interaction_data,
                           aes(x =  t_tests,
                               y = tests,
                               color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

tests_vs_t_tests
```

![](Interaction_files/figure-html/unnamed-chunk-40-1.png)<!-- -->

t tests vs post hoc

```r
t_tests_vs_post_hoc <- ggplot(data = interaction_data,
                                  aes(x =  post_hoc,
                                      y = t_tests,
                                      color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
 
t_tests_vs_post_hoc
```

![](Interaction_files/figure-html/unnamed-chunk-41-1.png)<!-- -->


all tests vs t tests

```r
all_tests_vs_t_tests <- ggplot(data = interaction_data,
                           aes(x =  t_tests,
                               y = all_tests,
                               color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

all_tests_vs_t_tests
```

![](Interaction_files/figure-html/unnamed-chunk-42-1.png)<!-- -->

t tests vs interaction reported

```r
t_tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                  aes(x =  ixn_reported,
                                      y = t_tests,
                                      color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

t_tests_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-43-1.png)<!-- -->

tests vs post hoc

```r
tests_vs_post_hoc <- ggplot(data = interaction_data,
                                aes(x =  post_hoc,
                                    y = tests,
                                    color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))

tests_vs_post_hoc
```

![](Interaction_files/figure-html/unnamed-chunk-44-1.png)<!-- -->

tests vs interaction reported

```r
tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                aes(x =  ixn_reported,
                                    y = tests,
                                    color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

tests_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-45-1.png)<!-- -->



```r
post_hoc_vs_ixn_reported <- ggplot(data = interaction_data,
                                   aes(x =  ixn_reported,
                                       y = post_hoc,
                                       color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

post_hoc_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-46-1.png)<!-- -->


all tests vs interaction reported

```r
all_tests_vs_ixn_reported <- ggplot(data = interaction_data,
                                aes(x =  ixn_reported,
                                    y = all_tests,
                                    color = rank)) +
  geom_point(alpha = 0.5, show.legend = FALSE) +
  geom_jitter(width = 0.3, show.legend = TRUE) 

all_tests_vs_ixn_reported
```

![](Interaction_files/figure-html/unnamed-chunk-47-1.png)<!-- -->



```r
#png( "mygraph1.png", width = 864, height = 864)
#p <- ggplot(interaction_data, aes(ixn_reported, analysis)) + geom_point()

#p + facet_grid(vars(data_availability), vars(design))
```


```r
#png( "mygraph2.png", width = 2304, height = 1536)
#p <- ggplot(interaction_data, aes(ixn_reported, analysis)) + geom_point()

#p + facet_grid(vars(design), vars(journal))
```



Example 1: Estimation of a treatment effect relative to a control effect (“Something different”)  
Example 2: Estimation of the effect of background condition on an effect (“it depends”)  
Example 3: Estimation of synergy (“More than the sum of the parts”)  
