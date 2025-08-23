# HARI 2: ADVANCED STATISTICAL FRAMEWORK
## Materi Pelatihan Analisis Data Empiris Berbantuan AI

---

## **📋 LEARNING OBJECTIVES**

Setelah menyelesaikan Hari 2, peserta akan mampu:
1. **Merancang** robust analysis design dengan pertimbangan multilevel structure
2. **Menerapkan** publication standards dan reporting guidelines internasional
3. **Mengembangkan** journal selection strategy yang optimal
4. **Mengintegrasikan** AI dengan ethical considerations dalam research

---

## **📊 SESSION 2A: ROBUST ANALYSIS DESIGN**
### **⏰ Durasi: 2 Jam (09:00-11:00)**

#### **🎯 Learning Outcomes:**
- Membedakan kapan menggunakan multilevel vs single-level analysis
- Mengidentifikasi nested data structure dalam berbagai konteks
- Memahami random vs fixed effects dalam modeling
- Menerapkan causal inference considerations

#### **📚 MATERI INTI:**

### **Part 1: Multilevel vs Single-Level Analysis Decisions (45 menit)**

#### **A. Identifying Data Structure:**

**Hierarchical Data Recognition:**
```r
# Common nested structures in educational research
examples <- list(
  "Students nested in Classes" = c("student_id", "class_id"),
  "Classes nested in Schools" = c("class_id", "school_id"),
  "Schools nested in Districts" = c("school_id", "district_id"),
  "Repeated Measures" = c("student_id", "time_point"),
  "Items nested in Tests" = c("item_id", "test_id"),
  "Teachers nested in Schools" = c("teacher_id", "school_id")
)

# Data structure assessment function
assess_nesting <- function(data, level1_var, level2_var) {
  # Calculate intraclass correlation (ICC)
  library(lme4)
  null_model <- lmer(outcome ~ 1 + (1|level2_var), data=data)
  
  # Extract variance components
  var_components <- as.data.frame(VarCorr(null_model))
  between_var <- var_components$vcov[1]
  within_var <- var_components$vcov[2]
  
  # Calculate ICC
  icc <- between_var / (between_var + within_var)
  
  # Decision rule
  if(icc > 0.05) {
    recommendation <- "Multilevel analysis required"
  } else if(icc > 0.01) {
    recommendation <- "Consider multilevel analysis"
  } else {
    recommendation <- "Single-level analysis may be sufficient"
  }
  
  return(list(
    icc = icc,
    between_variance = between_var,
    within_variance = within_var,
    recommendation = recommendation
  ))
}
```

#### **B. Consequences of Ignoring Clustering:**

**Statistical Issues:**
```r
# Simulation demonstrating clustering effects
library(simstudy)

# Generate clustered data
def_cluster <- defData(varname = "cluster_effect", dist = "normal", 
                      formula = 0, variance = 0.5)
def_individual <- defDataAdd(varname = "outcome", dist = "normal",
                           formula = "2 + cluster_effect", variance = 1)

# Simulate data with 20 clusters, 10 individuals each
data_clustered <- genData(20, def_cluster)
data_clustered <- genCluster(data_clustered, "cluster", numIndsVar = 10)
data_clustered <- addColumns(def_individual, data_clustered)

# Compare analysis approaches
# 1. Naive analysis (ignoring clustering)
naive_model <- lm(outcome ~ treatment, data = data_clustered)

# 2. Multilevel analysis (accounting for clustering)
multilevel_model <- lmer(outcome ~ treatment + (1|cluster), data = data_clustered)

# Compare standard errors
comparison <- data.frame(
  Model = c("Naive", "Multilevel"),
  Coefficient = c(coef(naive_model)[2], fixef(multilevel_model)[2]),
  SE = c(summary(naive_model)$coefficients[2,2], 
         summary(multilevel_model)$coefficients[2,2]),
  p_value = c(summary(naive_model)$coefficients[2,4],
              summary(multilevel_model)$coefficients[2,4])
)

print(comparison)
```

**Design Effect Calculation:**
```r
calculate_design_effect <- function(icc, cluster_size) {
  design_effect <- 1 + (cluster_size - 1) * icc
  effective_n <- total_n / design_effect
  
  return(list(
    design_effect = design_effect,
    effective_sample_size = effective_n,
    power_adjustment = design_effect
  ))
}
```

### **Part 2: Nested Data Structure Identification (30 menit)**

#### **A. Common Nesting Patterns:**

**Educational Research Examples:**
```r
# 1. Cross-sectional nested data
library(dplyr)

educational_nesting <- data.frame(
  Context = c("Achievement Study", "Teacher Effectiveness", "Intervention Study"),
  Level_1 = c("Students", "Students", "Students"),
  Level_2 = c("Classrooms", "Teachers", "Schools"),
  Level_3 = c("Schools", "Schools", "Districts"),
  ICC_Typical = c("0.15-0.20", "0.10-0.15", "0.05-0.10")
)

# 2. Longitudinal nested data
longitudinal_structure <- list(
  Level_1 = "Time points (within students)",
  Level_2 = "Students (within schools)",
  Level_3 = "Schools (within districts)",
  Considerations = c("Time-varying covariates", "Growth trajectories", "Autocorrelation")
)

# 3. Cross-classified data
cross_classified <- list(
  Example = "Students taught by multiple teachers",
  Structure = "Students cross-classified by current teacher and previous teacher",
  Analysis = "Cross-classified random effects model"
)
```

#### **B. Data Exploration for Nesting:**

**Exploratory Analysis:**
```r
# Function to explore potential nesting
explore_nesting <- function(data, potential_clusters) {
  results <- list()
  
  for(cluster_var in potential_clusters) {
    # Cluster size distribution
    cluster_sizes <- data %>%
      group_by(!!sym(cluster_var)) %>%
      summarise(n = n()) %>%
      pull(n)
    
    # Basic statistics
    results[[cluster_var]] <- list(
      n_clusters = length(unique(data[[cluster_var]])),
      mean_cluster_size = mean(cluster_sizes),
      median_cluster_size = median(cluster_sizes),
      min_cluster_size = min(cluster_sizes),
      max_cluster_size = max(cluster_sizes),
      cluster_size_sd = sd(cluster_sizes)
    )
  }
  
  return(results)
}

# Visualize clustering structure
library(ggplot2)
plot_cluster_structure <- function(data, cluster_var, outcome_var) {
  ggplot(data, aes(x = factor(!!sym(cluster_var)), y = !!sym(outcome_var))) +
    geom_boxplot() +
    theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
    labs(title = paste("Outcome variation across", cluster_var),
         x = cluster_var, y = outcome_var)
}
```

### **Part 3: Random vs Fixed Effects Modeling (30 menit)**

#### **A. Conceptual Framework:**

**Decision Matrix:**
```r
effects_decision <- data.frame(
  Consideration = c("Number of levels", "Interest in levels", "Generalization", 
                   "Statistical power", "Model complexity"),
  Fixed_Effects = c(">5 levels problematic", "Specific level effects", 
                   "Limited to observed levels", "Higher power", "Simpler"),
  Random_Effects = c("Many levels okay", "Population of levels", 
                    "Generalizable", "Lower power", "More complex"),
  Recommendation = c("Random if >5", "Fixed if specific", "Random for generalization",
                    "Consider trade-off", "Start simple")
)

print(effects_decision)
```

#### **B. Model Specification Examples:**

**1. Random Intercept Model:**
```r
library(lme4)

# Basic random intercept
model_ri <- lmer(achievement ~ ses + prior_achievement + (1|school_id), 
                data = student_data)

# Interpretation: Schools have different baseline achievement levels
```

**2. Random Slope Model:**
```r
# Random slope for SES effect
model_rs <- lmer(achievement ~ ses + prior_achievement + 
                (1 + ses|school_id), data = student_data)

# Interpretation: SES effect varies across schools
```

**3. Cross-Level Interactions:**
```r
# School-level moderator
model_cross <- lmer(achievement ~ ses + prior_achievement + school_resources +
                   ses:school_resources + (1 + ses|school_id), 
                   data = student_data)

# Interpretation: School resources moderate the SES effect
```

### **Part 4: Causal Inference Considerations (15 menit)**

#### **A. Causal Identification Strategies:**

**Potential Outcomes Framework:**
```r
# Propensity score matching for causal inference
library(MatchIt)

# Estimate propensity scores
ps_model <- glm(treatment ~ covariate1 + covariate2 + covariate3,
               family = binomial, data = data)

# Perform matching
matched_data <- matchit(treatment ~ covariate1 + covariate2 + covariate3,
                       data = data, method = "nearest", ratio = 1)

# Extract matched data
final_data <- match.data(matched_data)

# Estimate treatment effect
causal_effect <- lm(outcome ~ treatment, data = final_data)
```

#### **B. Instrumental Variables:**

**IV Implementation:**
```r
library(AER)

# Two-stage least squares
iv_model <- ivreg(outcome ~ endogenous_var + control1 + control2 | 
                 instrument + control1 + control2, data = data)

# Test instrument strength
first_stage <- lm(endogenous_var ~ instrument + control1 + control2, data = data)
f_stat <- summary(first_stage)$fstatistic[1]

if(f_stat > 10) {
  cat("Strong instrument (F >10)")
} else {
  cat("Weak instrument - consider alternatives")
}
```

---

## **📊 SESSION 2B: PUBLICATION STANDARDS & REPORTING**
### **⏰ Durasi: 2 Jam (11:15-13:15)**

#### **🎯 Learning Outcomes:**
- Menguasai APA 7th edition statistical reporting standards
- Memahami CONSORT, STROBE, dan PRISMA guidelines
- Menerapkan transparency dan reproducibility standards
- Mengimplementasikan open science practices

#### **📚 MATERI INTI:**

### **Part 1: APA 7th Edition Statistical Reporting (45 menit)**

#### **A. Core Reporting Requirements:**

**Statistical Test Reporting Template:**
```r
# APA-compliant reporting function
apa_report <- function(test_result, test_type) {
  
  if(test_type == "t_test") {
    # t-test reporting
    t_value <- round(test_result$statistic, 2)
    df <- test_result$parameter
    p_value <- ifelse(test_result$p.value < 0.001, "< .001", 
                     paste("=", round(test_result$p.value, 3)))
    
    report <- paste0("t(", df, ") = ", t_value, ", p ", p_value)
    
  } else if(test_type == "anova") {
    # ANOVA reporting
    f_value <- round(test_result$`F value`[1], 2)
    df1 <- test_result$Df[1]
    df2 <- test_result$Df[2]
    p_value <- ifelse(test_result$`Pr(>F)`[1] < 0.001, "< .001",
                     paste("=", round(test_result$`Pr(>F)`[1], 3)))
    
    report <- paste0("F(", df1, ", ", df2, ") = ", f_value, ", p ", p_value)
    
  } else if(test_type == "correlation") {
    # Correlation reporting
    r_value <- round(test_result$estimate, 2)
    df <- test_result$parameter
    p_value <- ifelse(test_result$p.value < 0.001, "< .001",
                     paste("=", round(test_result$p.value, 3)))
    
    report <- paste0("r(", df, ") = ", r_value, ", p ", p_value)
  }
  
  return(report)
}

# Example usage
t_test_result <- t.test(group1, group2)
apa_report(t_test_result, "t_test")
```

#### **B. Effect Size Reporting:**

**Comprehensive Effect Size Reporting:**
```r
library(effectsize)

# Complete effect size reporting function
report_effect_size <- function(model, test_type) {
  
  if(test_type == "t_test") {
    # Cohen's d with confidence interval
    d_result <- cohens_d(model)
    d_value <- round(d_result$Cohens_d, 2)
    ci_lower <- round(d_result$CI_low, 2)
    ci_upper <- round(d_result$CI_high, 2)
    
    # Interpretation
    interpretation <- case_when(
      abs(d_value) < 0.2 ~ "negligible",
      abs(d_value) < 0.5 ~ "small",
      abs(d_value) < 0.8 ~ "medium",
      TRUE ~ "large"
    )
    
    report <- paste0("d = ", d_value, ", 95% CI [", ci_lower, ", ", ci_upper, 
                    "], ", interpretation, " effect")
    
  } else if(test_type == "anova") {
    # Eta-squared with confidence interval
    eta_result <- eta_squared(model, ci = 0.95)
    eta_value <- round(eta_result$Eta2, 3)
    ci_lower <- round(eta_result$CI_low, 3)
    ci_upper <- round(eta_result$CI_high, 3)
    
    report <- paste0("η² = ", eta_value, ", 95% CI [", ci_lower, ", ", ci_upper, "]")
  }
  
  return(report)
}
```

#### **C. Complete Statistical Reporting Example:**

**Integrated Reporting:**
```r
# Function for complete APA reporting
complete_apa_report <- function(model, effect_size_model, descriptives) {
  
  # Descriptive statistics
  desc_stats <- descriptives %>%
    summarise(
      M = round(mean(value, na.rm = TRUE), 2),
      SD = round(sd(value, na.rm = TRUE), 2),
      n = sum(!is.na(value))
    )
  
  # Statistical test
  stat_result <- apa_report(model, "t_test")
  
  # Effect size
  effect_result <- report_effect_size(effect_size_model, "t_test")
  
  # Complete sentence
  full_report <- paste0(
    "Results showed a significant difference between groups, ",
    stat_result, ", ", effect_result, ". ",
    "Group 1 (M = ", desc_stats$M[1], ", SD = ", desc_stats$SD[1], ", n = ", desc_stats$n[1], ") ",
    "compared to Group 2 (M = ", desc_stats$M[2], ", SD = ", desc_stats$SD[2], ", n = ", desc_stats$n[2], ")."
  )
  
  return(full_report)
}
```

### **Part 2: Research Reporting Guidelines (45 menit)**

#### **A. CONSORT Statement (Randomized Trials):**

**CONSORT Checklist Implementation:**
```r
# CONSORT checklist for RCT reporting
consort_checklist <- data.frame(
  Section = c("Title", "Abstract", "Introduction", "Methods", "Results", "Discussion"),
  Item = c("1a. RCT in title", "2a. Structured summary", "3a. Background", 
          "4a. Participants", "13a. Participant flow", "20. Limitations"),
  Requirement = c("Identification as randomized trial",
                 "Trial design, methods, results, conclusions",
                 "Scientific background and rationale",
                 "Eligibility criteria for participants",
                 "Flow of participants through each stage",
                 "Trial limitations and generalizability"),
  Status = rep("✓", 6)  # To be filled during manuscript preparation
)

# CONSORT flow diagram data structure
consort_flow <- list(
  Assessed_for_eligibility = 0,
  Excluded = list(
    Not_meeting_criteria = 0,
    Declined_participation = 0,
    Other_reasons = 0
  ),
  Randomized = 0,
  Allocated_intervention = 0,
  Allocated_control = 0,
  Lost_to_followup_intervention = 0,
  Lost_to_followup_control = 0,
  Analyzed_intervention = 0,
  Analyzed_control = 0
)
```

#### **B. STROBE Statement (Observational Studies):**

**STROBE Reporting Structure:**
```r
# STROBE checklist for observational studies
strobe_checklist <- data.frame(
  Item = c("Title and abstract", "Background", "Objectives", "Study design",
          "Setting", "Participants", "Variables", "Data sources",
          "Bias", "Study size", "Statistical methods"),
  Requirement = c("Study design in title/abstract",
                 "Explain scientific background",
                 "State specific objectives",
                 "Present key elements of study design",
                 "Describe setting and dates",
                 "Give eligibility criteria",
                 "Clearly define all outcomes",
                 "Give information on data sources",
                 "Describe efforts to address bias",
                 "Explain how study size arrived at",
                 "Describe all statistical methods"),
  Location = c("Title page", "Introduction", "Introduction", "Methods",
              "Methods", "Methods", "Methods", "Methods",
              "Methods", "Methods", "Methods")
)
```

#### **C. PRISMA Statement (Systematic Reviews):**

**PRISMA Flow Implementation:**
```r
# PRISMA systematic review reporting
prisma_flow <- function(search_results) {
  flow_data <- list(
    Records_identified = search_results$database_total,
    Additional_records = search_results$additional_sources,
    Records_after_duplicates = search_results$after_deduplication,
    Records_screened = search_results$title_abstract_screened,
    Records_excluded = search_results$excluded_title_abstract,
    Full_text_assessed = search_results$full_text_reviewed,
    Full_text_excluded = search_results$excluded_full_text,
    Studies_included_synthesis = search_results$final_included,
    Studies_included_meta_analysis = search_results$meta_analysis_included
  )
  
  return(flow_data)
}

# Quality assessment for included studies
quality_assessment <- data.frame(
  Study = character(),
  Selection_bias = character(),  # Low/High/Unclear
  Performance_bias = character(),
  Detection_bias = character(),
  Attrition_bias = character(),
  Reporting_bias = character(),
  Overall_quality = character()
)
```

### **Part 3: Transparency and Reproducibility Standards (15 menit)**

#### **A. Open Science Framework Implementation:**

**Reproducibility Checklist:**
```r
# Reproducibility documentation structure
reproducibility_package <- list(
  
  # 1. Data availability
  Data = list(
    Raw_data = "Link to raw data repository",
    Processed_data = "Link to cleaned/processed data",
    Codebook = "Variable descriptions and coding",
    Data_collection_protocol = "Detailed collection procedures"
  ),
  
  # 2. Analysis code
  Code = list(
    Data_cleaning = "R script for data preparation",
    Analysis_scripts = "All statistical analysis code",
    Visualization_code = "Figure and table generation",
    Package_versions = "sessionInfo() output"
  ),
  
  # 3. Materials
  Materials = list(
    Instruments = "Questionnaires, interview guides",
    Intervention_protocols = "Detailed intervention descriptions",
    Training_materials = "Staff training documents"
  ),
  
  # 4. Pre-registration
  Preregistration = list(
    Study_protocol = "Detailed study protocol",
    Analysis_plan = "Pre-specified statistical analysis plan",
    Hypotheses = "Primary and secondary hypotheses"
  )
)
```

#### **B. Version Control and Documentation:**

**R Markdown Reporting Template:**
```r
# R Markdown structure for reproducible reports
rmarkdown_template <- '
---
title: "Reproducible Analysis Report"
author: "Author Name"
date: "`r Sys.Date()`"
output: 
  html_document:
    code_folding: show
    toc: true
    toc_float: true
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)

# Session information
sessionInfo()

# Load required packages
library(tidyverse)
library(lme4)
library(effectsize)
```

# Data Import and Cleaning
```{r data-import}
# Documented data import process
```

# Exploratory Data Analysis  
```{r eda}
# Comprehensive data exploration
```

# Statistical Analysis
```{r analysis}
# Main statistical analyses
```

# Results Reporting
```{r results}
# APA-compliant results reporting
```
'
```

---

## **📊 SESSION 2C: JOURNAL SELECTION STRATEGY**
### **⏰ Durasi: 2 Jam (14:15-16:15)**

#### **🎯 Learning Outcomes:**
- Menganalisis journal impact factor vs research fit
- Melakukan scope and aims alignment analysis
- Mengembangkan reviewer expectation management
- Mengoptimalkan publication timeline

#### **📚 MATERI INTI:**

### **Part 1: Journal Impact Factor vs Research Fit (45 menit)**

#### **A. Impact Factor Analysis Framework:**

**Journal Evaluation Matrix:**
```r
# Journal assessment function
evaluate_journal <- function(journal_name, impact_factor, quartile, 
                           acceptance_rate, review_time) {
  
  # Impact factor interpretation
  if_tier <- case_when(
    impact_factor >= 5.0 ~ "Top tier",
    impact_factor >= 3.0 ~ "High tier", 
    impact_factor >= 1.5 ~ "Medium tier",
    impact_factor >= 0.5 ~ "Lower tier",
    TRUE ~ "Emerging"
  )
  
  # Acceptance difficulty
  difficulty <- case_when(
    acceptance_rate <= 15 ~ "Very difficult",
    acceptance_rate <= 25 ~ "Difficult",
    acceptance_rate <= 40 ~ "Moderate",
    acceptance_rate <= 60 ~ "Accessible",
    TRUE ~ "Open"
  )
  
  # Publication speed
  speed <- case_when(
    review_time <= 3 ~ "Fast",
    review_time <= 6 ~ "Standard",
    review_time <= 12 ~ "Slow",
    TRUE ~ "Very slow"
  )
  
  evaluation <- data.frame(
    Journal = journal_name,
    Impact_Factor = impact_factor,
    Quartile = quartile,
    IF_Tier = if_tier,
    Acceptance_Rate = paste0(acceptance_rate, "%"),
    Difficulty = difficulty,
    Review_Time_Months = review_time,
    Speed = speed
  )
  
  return(evaluation)
}

# Example journal database
journal_database <- data.frame(
  Journal = c("Journal of Educational Psychology", "Educational Researcher", 
             "Computers & Education", "British Journal of Educational Technology"),
  Impact_Factor = c(5.6, 4.2, 5.5, 3.1),
  Quartile = c("Q1", "Q1", "Q1", "Q1"),
  Field = rep("Education Technology", 4),
  Acceptance_Rate = c(18, 12, 22, 28),
  Avg_Review_Time = c(4, 6, 5, 3)
)
```

#### **B. Research-Journal Fit Assessment:**

**Fit Analysis Framework:**
```r
# Journal-research alignment assessment
assess_journal_fit <- function(research_characteristics, journal_scope) {
  
  fit_criteria <- list(
    
    # Methodological fit
    methodology_fit = case_when(
      research_characteristics$method %in% journal_scope$preferred_methods ~ 3,
      research_characteristics$method %in% journal_scope$accepted_methods ~ 2,
      TRUE ~ 1
    ),
    
    # Topic relevance
    topic_fit = case_when(
      research_characteristics$topic %in% journal_scope$core_topics ~ 3,
      research_characteristics$topic %in% journal_scope$related_topics ~ 2,
      TRUE ~ 1
    ),
    
    # Innovation level
    innovation_fit = case_when(
      research_characteristics$innovation_level == "High" & 
      journal_scope$innovation_preference == "High" ~ 3,
      research_characteristics$innovation_level == "Medium" ~ 2,
      TRUE ~ 1
    ),
    
    # Sample characteristics
    sample_fit = case_when(
      research_characteristics$population %in% journal_scope$target_populations ~ 3,
      TRUE ~ 2
    )
  )
  
  # Calculate overall fit score
  fit_score <- mean(unlist(fit_criteria))
  
  # Recommendation
  recommendation <- case_when(
    fit_score >= 2.5 ~ "Strong fit - Recommend submission",
    fit_score >= 2.0 ~ "Good fit - Consider submission",
    fit_score >= 1.5 ~ "Moderate fit - Revise or consider alternatives",
    TRUE ~ "Poor fit - Seek alternative journals"
  )
  
  return(list(
    fit_criteria = fit_criteria,
    overall_fit = fit_score,
    recommendation = recommendation
  ))
}
```

#### **C. Citation Analysis and Journal Positioning:**

**Citation Impact Prediction:**
```r
library(bibliometrix)

# Journal citation pattern analysis
analyze_citation_patterns <- function(journal_articles) {
  
  # Calculate citation metrics
  citation_metrics <- journal_articles %>%
    summarise(
      median_citations = median(total_citations, na.rm = TRUE),
      q75_citations = quantile(total_citations, 0.75, na.rm = TRUE),
      top_10_percent = quantile(total_citations, 0.90, na.rm = TRUE),
      citation_variance = var(total_citations, na.rm = TRUE)
    )
  
  # Identify high-impact article characteristics
  high_impact <- journal_articles %>%
    filter(total_citations >= citation_metrics$q75_citations) %>%
    summarise(
      avg_authors = mean(n_authors),
      avg_pages = mean(page_count),
      common_keywords = paste(unique(keywords), collapse = "; "),
      methodology_patterns = paste(unique(methodology), collapse = "; ")
    )
  
  return(list(
    citation_metrics = citation_metrics,
    success_patterns = high_impact
  ))
}
```

### **Part 2: Scope and Aims Alignment Analysis (30 menit)**

#### **A. Journal Scope Deep Dive:**

**Scope Analysis Framework:**
```r
# Systematic scope analysis
analyze_journal_scope <- function(journal_recent_articles) {
  
  scope_analysis <- journal_recent_articles %>%
    mutate(
      # Extract methodological approaches
      methodology_category = case_when(
        str_detect(tolower(methods), "experiment|randomized") ~ "Experimental",
        str_detect(tolower(methods), "survey|questionnaire") ~ "Survey",
        str_detect(tolower(methods), "qualitative|interview") ~ "Qualitative",
        str_detect(tolower(methods), "meta-analysis|systematic") ~ "Meta-analysis",
        TRUE ~ "Other"
      ),
      
      # Categorize research topics
      topic_category = case_when(
        str_detect(tolower(title), "technology|digital|online") ~ "Technology",
        str_detect(tolower(title), "assessment|evaluation|test") ~ "Assessment",
        str_detect(tolower(title), "teacher|instruction|pedagogy") ~ "Teaching",
        str_detect(tolower(title), "student|learning|achievement") ~ "Learning",
        TRUE ~ "Other"
      )
    ) %>%
    
    # Calculate distributions
    group_by(methodology_category) %>%
    summarise(
      n_articles = n(),
      percentage = round(n() / nrow(.) * 100, 1)
    )
  
  return(scope_analysis)
}

# Journal preference scoring
score_journal_preferences <- function(my_research, journal_patterns) {
  
  alignment_scores <- list(
    
    # Methodological alignment
    method_score = ifelse(
      my_research$methodology %in% journal_patterns$top_methods, 1, 0.5
    ),
    
    # Topic alignment  
    topic_score = ifelse(
      my_research$topic %in% journal_patterns$frequent_topics, 1, 0.5
    ),
    
    # Sample size alignment
    sample_score = ifelse(
      my_research$sample_size >= journal_patterns$median_sample_size, 1, 0.7
    ),
    
    # Innovation alignment
    innovation_score = case_when(
      my_research$innovation_level == journal_patterns$innovation_preference ~ 1,
      abs(my_research$innovation_level - journal_patterns$innovation_preference) <= 1 ~ 0.8,
      TRUE ~ 0.5
    )
  )
  
  overall_alignment <- mean(unlist(alignment_scores))
  return(overall_alignment)
}
```

#### **B. Editorial Board Analysis:**

**Editor and Reviewer Network Mapping:**
```r
# Editorial board expertise analysis
analyze_editorial_board <- function(board_members) {
  
  expertise_map <- board_members %>%
    separate_rows(expertise_areas, sep = ";") %>%
    mutate(expertise_areas = str_trim(expertise_areas)) %>%
    count(expertise_areas, sort = TRUE) %>%
    mutate(
      percentage = round(n / sum(n) * 100, 1),
      coverage_level = case_when(
        percentage >= 15 ~ "Core expertise",
        percentage >= 8 ~ "Strong coverage",
        percentage >= 3 ~ "Moderate coverage",
        TRUE ~ "Limited coverage"
      )
    )
  
  return(expertise_map)
}

# Recent publication patterns of editors
editor_publication_analysis <- function(editor_publications) {
  
  recent_trends <- editor_publications %>%
    filter(year >= (max(year) - 3)) %>%
    group_by(editor_name) %>%
    summarise(
      recent_publications = n(),
      methodology_focus = paste(unique(methodology), collapse = "; "),
      topic_focus = paste(unique(research_area), collapse = "; "),
      collaboration_pattern = mean(n_coauthors),
      .groups = 'drop'
    )
  
  # Identify potential reviewers based on expertise overlap
  potential_reviewers <- recent_trends %>%
    filter(
      str_detect(methodology_focus, my_research$methodology) |
      str_detect(topic_focus, my_research$topic)
    ) %>%
    arrange(desc(recent_publications))
  
  return(list(
    editorial_trends = recent_trends,
    potential_reviewer_pool = potential_reviewers
  ))
}
```

### **Part 3: Reviewer Expectation Management (30 menit)**

#### **A. Reviewer Profile Development:**

**Typical Reviewer Characteristics:**
```r
# Reviewer expectation framework
reviewer_expectations <- data.frame(
  Journal_Tier = c("Q1", "Q2", "Q3", "Q4"),
  
  Methodology_Rigor = c(
    "Advanced statistical methods, multiple robustness checks",
    "Solid methodology with some sophistication", 
    "Standard methods appropriately applied",
    "Basic methods sufficient"
  ),
  
  Innovation_Level = c(
    "Significant theoretical or methodological contribution",
    "Moderate innovation or novel application",
    "Incremental advancement acceptable",
    "Replication or basic application"
  ),
  
  Sample_Requirements = c(
    "Large samples, power analysis required",
    "Adequate samples with justification",
    "Reasonable samples for conclusions",
    "Small samples acceptable if appropriate"
  ),
  
  Writing_Quality = c(
    "Exceptional clarity and precision",
    "Clear writing with minor issues",
    "Generally clear communication",
    "Basic clarity requirements"
  ),
  
  Literature_Review = c(
    "Comprehensive, critical synthesis",
    "Thorough review with good integration", 
    "Adequate coverage of key studies",
    "Basic literature coverage"
  )
)

print(reviewer_expectations)
```

#### **B. Common Reviewer Concerns:**

**Anticipating and Addressing Concerns:**
```r
# Common reviewer feedback categories
reviewer_concerns <- list(
  
  Methodology = list(
    concerns = c(
      "Sample size insufficient for conclusions",
      "Statistical assumptions not tested",
      "Confounding variables not controlled",
      "Analysis method inappropriate for data type",
      "Missing data handling inadequate"
    ),
    prevention_strategies = c(
      "Conduct power analysis and report",
      "Include assumption testing results",
      "Control for major confounders",
      "Justify analysis choice with citations",
      "Use appropriate missing data methods"
    )
  ),
  
  Interpretation = list(
    concerns = c(
      "Conclusions not supported by results",
      "Causal claims from correlational data",
      "Overgeneralization from limited sample",
      "Effect sizes not reported or interpreted",
      "Alternative explanations not considered"
    ),
    prevention_strategies = c(
      "Align conclusions with findings",
      "Use appropriate causal language",
      "Acknowledge generalizability limits",
      "Report and interpret effect sizes",
      "Discuss alternative explanations"
    )
  ),
  
  Presentation = list(
    concerns = c(
      "Unclear research questions",
      "Poor organization of manuscript",
      "Insufficient detail in methods",
      "Tables and figures unclear",
      "Writing quality issues"
    ),
    prevention_strategies = c(
      "Use PICO framework for questions",
      "Follow journal structure guidelines",
      "Provide reproducible detail",
      "Follow APA table/figure standards",
      "Professional editing recommended"
    )
  )
)
```

#### **C. Proactive Response Preparation:**

**Response Strategy Framework:**
```r
# Prepare responses to likely concerns
prepare_reviewer_responses <- function(manuscript_characteristics) {
  
  potential_concerns <- list()
  
  # Sample size concerns
  if(manuscript_characteristics$sample_size < 200) {
    potential_concerns$sample_size <- list(
      likely_comment = "Sample size appears small for the proposed analyses",
      prepared_response = "Power analysis indicated adequate sample size for detecting medium effects (d=0.5) with 80% power. Sensitivity analyses confirm robustness of findings."
    )
  }
  
  # Methodology concerns
  if(manuscript_characteristics$design == "cross_sectional") {
    potential_concerns$causality <- list(
      likely_comment = "Cross-sectional design limits causal inferences",
      prepared_response = "We acknowledge the cross-sectional limitation and have revised conclusions to reflect correlational rather than causal language. Future longitudinal research is needed to establish temporal precedence."
    )
  }
  
  # Generalizability concerns
  if(manuscript_characteristics$setting == "single_institution") {
    potential_concerns$generalizability <- list(
      likely_comment = "Single institution limits generalizability",
      prepared_response = "While conducted at one institution, the demographic characteristics align with national averages (provide comparison data). Replication across diverse settings would strengthen conclusions."
    )
  }
  
  return(potential_concerns)
}
```

### **Part 4: Publication Timeline Optimization (15 menit)**

#### **A. Strategic Submission Planning:**

**Timeline Optimization Strategy:**
```r
# Publication timeline calculator
calculate_publication_timeline <- function(journal_characteristics, manuscript_status) {
  
  timeline_estimates <- list(
    
    # Pre-submission phase
    manuscript_preparation = case_when(
      manuscript_status == "first_draft" ~ 8,
      manuscript_status == "revision_needed" ~ 4,
      manuscript_status == "nearly_ready" ~ 2,
      TRUE ~ 6
    ),
    
    # Review phase
    initial_review = case_when(
      journal_characteristics$tier == "Q1" ~ 6,
      journal_characteristics$tier == "Q2" ~ 4,
      journal_characteristics$tier == "Q3" ~ 3,
      TRUE ~ 2
    ),
    
    # Revision phase (if major revisions)
    revision_period = case_when(
      journal_characteristics$typical_outcome == "major_revision" ~ 3,
      journal_characteristics$typical_outcome == "minor_revision" ~ 1,
      TRUE ~ 2
    ),
    
    # Second review (if needed)
    second_review = case_when(
      journal_characteristics$tier == "Q1" ~ 3,
      journal_characteristics$tier == "Q2" ~ 2,
      TRUE ~ 1.5
    ),
    
    # Production phase
    production = case_when(
      journal_characteristics$publication_model == "online_first" ~ 1,
      journal_characteristics$publication_model == "issue_based" ~ 3,
      TRUE ~ 2
    )
  )
  
  # Calculate total timeline
  total_months <- sum(unlist(timeline_estimates))
  
  # Risk factors that could extend timeline
  risk_factors <- list(
    reviewer_availability = ifelse(manuscript_characteristics$specialty_area == "niche", 1, 0),
    seasonal_delays = ifelse(manuscript_characteristics$submission_month %in% c(12, 1, 7, 8), 1, 0),
    complexity_delays = ifelse(manuscript_characteristics$complexity == "high", 2, 0)
  )
  
  adjusted_timeline <- total_months + sum(unlist(risk_factors))
  
  return(list(
    base_timeline = timeline_estimates,
    total_months = total_months,
    risk_adjustments = risk_factors,
    realistic_timeline = adjusted_timeline
  ))
}
```

#### **B. Portfolio Approach to Submission:**

**Multi-Journal Strategy:**
```r
# Journal portfolio optimization
optimize_journal_portfolio <- function(manuscript_characteristics) {
  
  journal_options <- list(
    
    # Primary target (ambitious but realistic)
    primary = list(
      criteria = "High impact, good fit, moderate acceptance rate",
      timeline = "8-12 months",
      probability = "25-40%",
      strategy = "Invest in thorough preparation"
    ),
    
    # Secondary target (solid backup)
    secondary = list(
      criteria = "Good impact, excellent fit, higher acceptance rate", 
      timeline = "6-9 months",
      probability = "50-70%",
      strategy = "Prepare adaptation strategy"
    ),
    
    # Safety option (high probability)
    safety = list(
      criteria = "Moderate impact, perfect fit, high acceptance rate",
      timeline = "4-6 months", 
      probability = "70-85%",
      strategy = "Quick turnaround option"
    )
  )
  
  # Submission sequence strategy
  submission_strategy <- list(
    parallel_submission = "Not recommended due to ethics",
    sequential_submission = "Primary → Secondary → Safety",
    timing_considerations = "Allow 2-3 months between rejections",
    adaptation_requirements = "Modify for each journal's requirements"
  )
  
  return(list(
    portfolio = journal_options,
    strategy = submission_strategy
  ))
}
```

---

## **📊 SESSION 2D: RESEARCH ETHICS & AI INTEGRATION**
### **⏰ Durasi: 2 Jam (16:30-18:30)**

#### **🎯 Learning Outcomes:**
- Memahami ethical use of AI dalam research
- Menerapkan data privacy dan security protocols
- Mempertimbangkan intellectual property issues
- Mengimplementasikan attribution dan transparency standards

#### **📚 MATERI INTI:**

### **Part 1: Ethical Use of AI in Research (45 menit)**

#### **A. AI Ethics Framework for Research:**

**Ethical Guidelines Matrix:**
```r
# AI ethics assessment framework
ai_ethics_framework <- data.frame(
  Principle = c("Beneficence", "Non-maleficence", "Autonomy", "Justice", "Transparency"),
  
  Definition = c(
    "AI should benefit research and society",
    "AI should not cause harm or bias",
    "Researchers maintain control and decision-making",
    "AI benefits should be distributed fairly",
    "AI use should be disclosed and explainable"
  ),
  
  Research_Application = c(
    "Use AI to improve research quality and efficiency",
    "Avoid AI that introduces bias or false conclusions",
    "Human oversight of all AI-generated content",
    "Ensure AI tools are accessible to all researchers",
    "Clearly report AI assistance in methods and acknowledgments"
  ),
  
  Red_Flags = c(
    "Using AI to fabricate data or results",
    "Accepting AI outputs without verification",
    "Replacing human judgment entirely",
    "Using AI that perpetuates existing biases",
    "Hiding AI use from reviewers or readers"
  )
)

print(ai_ethics_framework)
```

#### **B. Acceptable vs Unacceptable AI Usage:**

**Usage Guidelines:**
```r
# AI usage classification system
classify_ai_usage <- function(usage_description, context) {
  
  acceptable_uses <- c(
    "Statistical analysis guidance and code debugging",
    "Literature review assistance and synthesis",
    "Data cleaning and preprocessing automation",
    "Visualization and figure enhancement",
    "Writing clarity and grammar improvement",
    "Research question refinement",
    "Methodology selection guidance"
  )
  
  questionable_uses <- c(
    "Generating hypotheses without human input",
    "Automated data interpretation without verification",
    "Large-scale text generation for manuscripts",
    "Peer review automation",
    "Grant writing automation"
  )
  
  unacceptable_uses <- c(
    "Data fabrication or falsification",
    "Plagiarism or uncredited text generation",
    "Bias introduction or amplification", 
    "Research misconduct facilitation",
    "Intellectual property violation",
    "Ghostwriting without disclosure"
  )
  
  # Classification logic
  if(usage_description %in% acceptable_uses) {
    classification <- "Acceptable"
    requirements <- "Document usage, verify outputs"
  } else if(usage_description %in% questionable_uses) {
    classification <- "Needs careful consideration"
    requirements <- "Extensive human oversight, ethical review"
  } else if(usage_description %in% unacceptable_uses) {
    classification <- "Unacceptable"
    requirements <- "Prohibited"
  } else {
    classification <- "Requires case-by-case evaluation"
    requirements <- "Consult ethics committee"
  }
  
  return(list(
    classification = classification,
    requirements = requirements,
    context = context
  ))
}
```

#### **C. AI Disclosure Standards:**

**Transparency Reporting Template:**
```r
# AI disclosure template for manuscripts
ai_disclosure_template <- list(
  
  Methods_Section = "
  AI Assistance: This research utilized artificial intelligence tools for [specific purposes]. 
  [AI Tool Name] was used to [specific task] under human supervision. All AI-generated 
  content was reviewed, verified, and validated by the research team. The AI did not 
  make autonomous decisions about research design, data interpretation, or conclusions.
  ",
  
  Acknowledgments = "
  The authors acknowledge the use of [AI Tool Name] for [specific assistance provided]. 
  All outputs were human-reviewed and verified. The AI tool did not contribute to 
  conceptual development or interpretation of results.
  ",
  
  Conflict_of_Interest = "
  The authors declare that AI tools were used as research assistance only, with full 
  human oversight and verification of all outputs. No financial relationships exist 
  with AI tool providers that could bias this research.
  ",
  
  Data_Availability = "
  Information about AI tool usage, including prompts and verification procedures, 
  is available upon request to ensure reproducibility.
  "
)
```

### **Part 2: Data Privacy and Security (30 menit)**

#### **A. Data Protection Framework:**

**Privacy Risk Assessment:**
```r
# Data privacy risk assessment tool
assess_privacy_risk <- function(data_characteristics, ai_tools) {
  
  risk_factors <- list(
    
    # Data sensitivity level
    sensitivity_score = case_when(
      data_characteristics$contains_pii == TRUE ~ 3,
      data_characteristics$contains_sensitive_info == TRUE ~ 2,
      data_characteristics$is_anonymous == TRUE ~ 1,
      TRUE ~ 0
    ),
    
    # AI tool security level
    tool_security_score = case_when(
      ai_tools$data_retention == "permanent" ~ 3,
      ai_tools$data_retention == "temporary" ~ 2,
      ai_tools$data_retention == "none" ~ 1,
      TRUE ~ 2
    ),
    
    # Transmission security
    transmission_score = case_when(
      ai_tools$encryption == "none" ~ 3,
      ai_tools$encryption == "basic" ~ 2,
      ai_tools$encryption == "enterprise" ~ 1,
      TRUE ~ 2
    ),
    
    # Jurisdictional considerations
    jurisdiction_score = case_when(
      ai_tools$data_location == "unknown" ~ 3,
      ai_tools$data_location == "different_country" ~ 2,
      ai_tools$data_location == "same_country" ~ 1,
      TRUE ~ 2
    )
  )
  
  total_risk <- sum(unlist(risk_factors))
  
  risk_level <- case_when(
    total_risk >= 9 ~ "High Risk - Avoid or extensive safeguards needed",
    total_risk >= 6 ~ "Medium Risk - Additional protections required",
    total_risk >= 3 ~ "Low-Medium Risk - Standard protections adequate",
    TRUE ~ "Low Risk - Minimal additional protections needed"
  )
  
  return(list(
    risk_factors = risk_factors,
    total_risk = total_risk,
    risk_level = risk_level
  ))
}
```

#### **B. Data Anonymization Protocols:**

**De-identification Procedures:**
```r
# Data anonymization for AI processing
anonymize_for_ai <- function(dataset, anonymization_level = "standard") {
  
  if(anonymization_level == "basic") {
    # Remove direct identifiers only
    clean_data <- dataset %>%
      select(-any_of(c("name", "email", "phone", "address", "id_number")))
    
  } else if(anonymization_level == "standard") {
    # Remove direct and quasi-identifiers
    clean_data <- dataset %>%
      select(-any_of(c("name", "email", "phone", "address", "id_number",
                      "zip_code", "birth_date", "exact_age"))) %>%
      mutate(
        # Generalize age to ranges
        age_group = case_when(
          age < 25 ~ "Under 25",
          age < 35 ~ "25-34", 
          age < 45 ~ "35-44",
          age < 55 ~ "45-54",
          TRUE ~ "55+"
        ),
        # Remove exact dates
        year_only = year(date_variable)
      ) %>%
      select(-age, -date_variable)
    
  } else if(anonymization_level == "high") {
    # K-anonymity approach
    clean_data <- dataset %>%
      # Implement k-anonymity with k=5
      group_by(age_group, gender, education_level) %>%
      filter(n() >= 5) %>%
      ungroup() %>%
      # Add noise to numerical variables
      mutate(
        score_noisy = score + rnorm(n(), 0, 0.1),
        # Top and bottom code extreme values
        income_coded = case_when(
          income < quantile(income, 0.05, na.rm = TRUE) ~ "Bottom 5%",
          income > quantile(income, 0.95, na.rm = TRUE) ~ "Top 5%",
          TRUE ~ as.character(round(income, -3))  # Round to nearest 1000
        )
      )
  }
  
  return(clean_data)
}
```

### **Part 3: Intellectual Property Considerations (30 menit)**

#### **A. IP Rights Framework:**

**Intellectual Property Assessment:**
```r
# IP considerations for AI-assisted research
ip_assessment <- data.frame(
  Content_Type = c("Research Ideas", "Data Analysis", "Code/Scripts", 
                  "Written Content", "Figures/Visualizations"),
  
  Human_Contribution = c("Conceptualization, design", "Interpretation, validation",
                        "Logic, implementation", "Ideas, structure, revision",
                        "Design, interpretation"),
  
  AI_Contribution = c("Refinement suggestions", "Code generation, debugging",
                     "Code optimization", "Grammar, clarity", "Visual enhancement"),
  
  Ownership = c("Human (with AI assistance noted)", "Human (with AI assistance noted)",
               "Human (with AI assistance noted)", "Human (with AI assistance noted)",
               "Human (with AI assistance noted)"),
  
  Attribution_Required = c("Yes - in methods", "Yes - in methods", 
                          "Yes - in acknowledgments", "Yes - in acknowledgments",
                          "Yes - in figure captions")
)

print(ip_assessment)
```

#### **B. Collaboration vs Assistance Distinction:**

**AI Role Classification:**
```r
# Distinguish AI roles for proper attribution
classify_ai_role <- function(ai_contribution, human_oversight) {
  
  role_types <- list(
    
    Tool = list(
      description = "AI as computational instrument",
      examples = c("Statistical software", "Spell checker", "Grammar tool"),
      attribution = "Acknowledge in methods or footnotes",
      authorship = "Not eligible for authorship"
    ),
    
    Assistant = list(
      description = "AI provides suggestions reviewed by humans",
      examples = c("Code debugging", "Literature search", "Writing improvement"),
      attribution = "Acknowledge in methods and/or acknowledgments",
      authorship = "Not eligible for authorship"
    ),
    
    Collaborator = list(
      description = "AI makes autonomous decisions (hypothetical future)",
      examples = c("Independent data interpretation", "Autonomous hypothesis generation"),
      attribution = "Would require full disclosure and ethical review",
      authorship = "Currently not applicable - human oversight required"
    )
  )
  
  # Current classification (all should be Tool or Assistant)
  if(human_oversight == "complete") {
    return(role_types$Assistant)
  } else if(human_oversight == "minimal") {
    return("Requires enhanced human oversight")
  } else {
    return(role_types$Tool)
  }
}
```

### **Part 4: Attribution and Transparency Standards (15 menit)**

#### **A. Comprehensive Attribution Framework:**

**Multi-Level Attribution System:**
```r
# Complete attribution documentation
document_ai_usage <- function(research_project) {
  
  attribution_record <- list(
    
    # Project level documentation
    project_overview = list(
      ai_tools_used = character(),
      primary_purposes = character(),
      human_oversight_level = character(),
      verification_procedures = character()
    ),
    
    # Task-specific documentation
    task_documentation = data.frame(
      Task = character(),
      AI_Tool = character(),
      Specific_Use = character(),
      Human_Verification = character(),
      Output_Modification = character(),
      Quality_Check = character()
    ),
    
    # Publication documentation
    manuscript_attribution = list(
      methods_disclosure = character(),
      acknowledgment_text = character(),
      supplementary_info = character(),
      reproducibility_info = character()
    ),
    
    # Ethical compliance
    ethics_documentation = list(
      irb_notification = logical(),
      data_privacy_measures = character(),
      bias_mitigation = character(),
      transparency_measures = character()
    )
  )
  
  return(attribution_record)
}
```

#### **B. Quality Assurance Protocols:**

**AI Output Verification System:**
```r
# Quality assurance for AI-assisted research
qa_protocol <- function(ai_output, verification_type) {
  
  verification_procedures <- list(
    
    statistical_code = list(
      checks = c("Logic review", "Output validation", "Alternative method comparison"),
      required_expertise = "Statistical knowledge",
      documentation = "Code comments and validation results"
    ),
    
    literature_synthesis = list(
      checks = c("Source verification", "Accuracy checking", "Bias assessment"),
      required_expertise = "Domain knowledge", 
      documentation = "Reference verification log"
    ),
    
    data_analysis = list(
      checks = c("Assumption testing", "Sensitivity analysis", "Expert review"),
      required_expertise = "Methodological expertise",
      documentation = "Analysis decision log"
    ),
    
    writing_assistance = list(
      checks = c("Accuracy review", "Voice consistency", "Plagiarism check"),
      required_expertise = "Subject matter expertise",
      documentation = "Revision tracking"
    )
  )
  
  return(verification_procedures[[verification_type]])
}
```

---

## **🎯 PRACTICAL EXERCISES & ASSESSMENTS**

### **💡 Exercise 1: Multilevel Analysis Design (60 menit)**
**Hands-on R Implementation**

**Task:** Design and implement multilevel analysis
1. Assess nested structure in provided dataset
2. Compare single-level vs multilevel results
3. Interpret ICC and variance components
4. Report findings using APA standards

### **💡 Exercise 2: Journal Selection Matrix (45 menit)**
**Strategic Planning Exercise**

**Task:** Develop publication strategy for research project
1. Create journal evaluation matrix
2. Assess research-journal fit for 3 target journals
3. Develop submission timeline and strategy
4. Prepare reviewer response strategies

### **💡 Exercise 3: AI Ethics Case Studies (30 menit)**
**Ethical Decision Making**

**Task:** Evaluate AI usage scenarios
1. Classify AI usage as acceptable/questionable/unacceptable
2. Develop appropriate disclosure statements
3. Design verification procedures
4. Create attribution documentation

### **💡 Exercise 4: Reproducibility Implementation (45 menit)**
**Documentation Practice**

**Task:** Create reproducible research package
1. Document complete analysis pipeline
2. Prepare data and code for sharing
3. Write transparency statements
4. Develop verification procedures

---

## **📝 ASSESSMENT RUBRIC**

### **Conceptual Understanding (40%)**
- Multilevel modeling concepts
- Publication standards knowledge
- Ethics framework application
- Research integrity principles

### **Technical Implementation (35%)**
- R code quality and accuracy
- Statistical reporting compliance
- Documentation completeness
- Reproducibility standards

### **Strategic Thinking (25%)**
- Journal selection rationale
- Timeline optimization
- Risk assessment quality
- Ethical decision making

---

## **📚 RECOMMENDED READINGS**

### **Essential References:**
1. Raudenbush, S. W., & Bryk, A. S. (2002). *Hierarchical linear models: Applications and data analysis methods* (2nd ed.). Sage.

2. American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). American Psychological Association.

3. Moher, D., et al. (2010). CONSORT 2010 statement: Updated guidelines for reporting parallel group randomised trials. *BMJ, 340*, c332.

### **AI Ethics Resources:**
1. Montreal Declaration for Responsible AI (2018)
2. IEEE Standards for Ethical Design of Autonomous Systems
3. Nature Guidelines on AI in Research (2023)

### **Statistical Resources:**
1. Nakagawa, S., & Schielzeth, H. (2013). A general and simple method for obtaining R² from generalized linear mixed‐effects models. *Methods in Ecology and Evolution, 4*(2), 133-142.

2. Lakens, D. (2022). Sample size justification. *Collabra: Psychology, 8*(1), 33267.

---

## **🏁 DAY 2 WRAP-UP**

### **Key Takeaways:**
1. **Multilevel Thinking:** Recognize and appropriately model nested data structures
2. **Publication Strategy:** Systematic approach to journal selection and submission
3. **Ethical AI Use:** Responsible integration with proper attribution and transparency
4. **Reproducibility:** Complete documentation for verification and replication

### **Tomorrow's Preview:**
- Hands-on descriptive analysis mastery
- Advanced visualization techniques
- Publication-quality table creation
- Statistical assumption checking

### **Homework Assignment:**
1. Evaluate your data for multilevel structure using provided R scripts
2. Complete journal selection matrix for your research
3. Draft AI usage disclosure statement
4. Prepare dataset for tomorrow's advanced analysis session

**READY FOR DAY 3: HANDS-ON TECHNICAL MASTERY!** 🚀