# HARI 1: RESEARCH DESIGN & METHODOLOGY ASSESSMENT
## Materi Pelatihan Analisis Data Empiris Berbantuan AI

---

## **📋 LEARNING OBJECTIVES**

Setelah menyelesaikan Hari 1, peserta akan mampu:
1. **Mengidentifikasi** kelemahan metodologi yang menyebabkan penolakan manuscript
2. **Mengevaluasi** kualitas data dan melakukan persiapan data yang robust
3. **Menerapkan** statistical thinking yang benar dalam research design
4. **Menggunakan** AI untuk diagnosis dan perbaikan metodologi penelitian

---

## **📊 SESSION 1A: PARADIGM SHIFT IN RESEARCH ANALYSIS**
### **⏰ Durasi: 2 Jam (09:00-11:00)**

#### **🎯 Learning Outcomes:**
- Memahami penyebab utama penolakan manuscript di jurnal bereputasi
- Membedakan standar publikasi Q1, Q2, dan Q3
- Mengenal peran AI dalam meningkatkan kualitas penelitian

#### **📚 MATERI INTI:**

### **Part 1: Why 80% Manuscripts Get Rejected (30 menit)**

#### **A. Top 10 Rejection Reasons:**

1. **Weak Methodology (35% kasus)**
   ```
   ❌ Common Problems:
   - Sample size tidak adequate (power analysis missing)
   - Statistical tests inappropriate untuk data type
   - Confounding variables tidak dikontrol
   - Missing data handling inadequate
   
   ✅ AI-Enhanced Solutions:
   - Power analysis menggunakan G*Power + AI interpretation
   - Statistical test selection guidance via AI
   - Confounder identification assistance
   - Missing data pattern analysis
   ```

2. **Poor Statistical Reporting (25% kasus)**
   ```
   ❌ Common Problems:
   - P-hacking dan multiple testing tanpa correction
   - Effect sizes tidak dilaporkan
   - Confidence intervals missing
   - Assumption violations tidak dicheck
   
   ✅ Robust Alternatives:
   - Preregistered analysis plans
   - Effect size calculations + interpretation
   - Complete statistical reporting
   - Diagnostic checking protocols
   ```

3. **Insufficient Data Analysis (20% kasus)**
   ```
   ❌ Shallow Analysis:
   - Descriptive statistics only
   - Single-level analysis untuk nested data
   - No exploratory data analysis
   - Missing pattern identification
   
   ✅ Deep Analysis:
   - Multi-level statistical modeling
   - Comprehensive exploratory analysis
   - Pattern recognition techniques
   - Robust diagnostic procedures
   ```

#### **B. Impact Factor vs Quality Matrix:**

| Journal Tier | Impact Factor | Methodology Requirements | AI Integration Level |
|--------------|---------------|-------------------------|---------------------|
| **Q1** | >3.0 | Rigorous + Innovation | Advanced AI assistance |
| **Q2** | 1.5-3.0 | Solid + Some innovation | Moderate AI use |
| **Q3** | 0.5-1.5 | Basic requirements | Basic AI integration |
| **Q4** | <0.5 | Minimal standards | Optional AI use |

#### **💡 Interactive Exercise 1A (15 menit):**
**"Manuscript Diagnosis Challenge"**

Peserta diberikan 3 abstracts dari rejected papers dan harus:
1. Identify methodology weaknesses
2. Suggest AI-assisted improvements
3. Predict likely journal tier after improvement

**Contoh Abstract untuk Analisis:**
```
Title: "The Effect of Online Learning on Student Performance"

Abstract: "This study examined 100 students from one university. 
We compared grades before and after online learning implementation. 
Results showed significant improvement (p<0.05). Online learning 
is effective for improving student performance."

TASK: Identify 5+ major weaknesses and suggest improvements.
```

### **Part 2: Publication Standards Analysis (45 menit)**

#### **A. Q1 Journal Requirements Deep Dive:**

**Methodology Standards:**
```
1. Research Design:
   ✅ Clear theoretical framework
   ✅ Hypothesis pre-registration preferred
   ✅ Power analysis documented
   ✅ Control variables justified

2. Statistical Analysis:
   ✅ Assumption checking mandatory
   ✅ Effect sizes with interpretation
   ✅ Multiple testing corrections
   ✅ Sensitivity analyses included

3. Reporting Standards:
   ✅ CONSORT/STROBE compliance
   ✅ Complete statistical reporting
   ✅ Reproducible analysis code
   ✅ Raw data availability
```

#### **B. AI Integration Ethics:**

**Acceptable AI Usage:**
```
✅ Statistical analysis guidance
✅ Code debugging and optimization
✅ Literature review assistance
✅ Writing clarity improvement
✅ Data pattern identification

❌ Prohibited AI Usage:
❌ Data fabrication or manipulation
❌ Plagiarism or ghost writing
❌ Bias introduction or hiding
❌ Methodology substitution
❌ Results misinterpretation
```

#### **💡 Interactive Exercise 1B (30 menit):**
**"Journal Standards Comparison"**

Peserta dalam kelompok menganalisis reviewer comments dari:
- 1 Q1 journal rejection
- 1 Q2 journal acceptance with major revision
- 1 Q3 journal acceptance

**Analysis Framework:**
1. What methodology concerns were raised?
2. How do standards differ across tiers?
3. Which issues could AI have prevented?
4. What improvements would elevate the work?

### **Part 3: Case Study Analysis (30 menit)**

#### **Before-After Transformation Example:**

**CASE: Educational Technology Research**

**❌ WEAK VERSION (Rejected 3x):**
```
Sample: 50 students from one class
Analysis: t-test comparing pre-post scores
Results: "Significant improvement (p=0.032)"
Conclusion: "Technology improves learning"
```

**✅ ROBUST VERSION (Accepted Q1):**
```
Sample: 240 students from 8 schools (power analysis: 80% power)
Design: Cluster randomized controlled trial
Analysis: Multilevel modeling with random effects for schools
Controls: Prior achievement, SES, teacher experience
Results: Effect size d=0.42 (95% CI: 0.18-0.66), 
         significant improvement (β=0.34, SE=0.12, p=0.006)
Conclusion: "Technology shows moderate positive effect with 
            practical significance for educational outcomes"
```

**Key Improvements:**
1. **Sample Size:** Power analysis-based recruitment
2. **Design:** Clustered randomization with controls
3. **Analysis:** Appropriate multilevel modeling
4. **Reporting:** Effect sizes with confidence intervals
5. **Interpretation:** Practical significance discussion

---

## **📊 SESSION 1B: DATA QUALITY & PREPARATION MASTERY**
### **⏰ Durasi: 2 Jam (11:15-13:15)**

#### **🎯 Learning Outcomes:**
- Melakukan data integrity assessment menggunakan AI
- Menguasai teknik handling missing data yang sophisticated
- Mengidentifikasi dan menangani outliers dengan tepat
- Memahami transformasi variabel untuk analisis optimal

#### **📚 MATERI INTI:**

### **Part 1: Data Integrity Assessment (30 menit)**

#### **A. Comprehensive Data Audit Framework:**

**1. Structural Integrity Check:**
```r
# AI-Assisted Data Audit Protocol
library(VIM)
library(mice)
library(naniar)

# 1. Basic structure assessment
dim(data)                  # Dimensions check
str(data)                  # Variable types
summary(data)              # Basic statistics

# 2. Missing data patterns
vis_miss(data)             # Missing pattern visualization
gg_miss_upset(data)        # Upset plot for missing combinations
```

**2. Logical Consistency Validation:**
```r
# AI-guided consistency checks
# Age vs birth date consistency
data$age_calculated <- year(Sys.Date()) - year(data$birth_date)
data$age_discrepancy <- abs(data$age - data$age_calculated) > 2

# Score range validation
data$invalid_scores <- data$test_score < 0 | data$test_score > 100

# Cross-variable logical checks
data$logical_errors <- (data$education_years > data$age - 5) |
                       (data$experience_years > data$age - 18)
```

#### **B. AI-Powered Data Quality Metrics:**

**Quality Score Calculation:**
```r
calculate_data_quality <- function(data) {
  completeness <- 1 - (sum(is.na(data)) / (nrow(data) * ncol(data)))
  consistency <- 1 - (sum(data$logical_errors, na.rm=T) / nrow(data))
  validity <- 1 - (sum(data$invalid_scores, na.rm=T) / nrow(data))
  
  overall_quality <- (completeness + consistency + validity) / 3
  
  return(list(
    completeness = completeness,
    consistency = consistency,
    validity = validity,
    overall_quality = overall_quality
  ))
}
```

### **Part 2: Missing Data Handling Strategies (45 menit)**

#### **A. Missing Data Mechanisms:**

**1. Missing Completely at Random (MCAR):**
```r
# Test for MCAR using Little's test
library(naniar)
mcar_test(data)

# If MCAR: Listwise deletion acceptable for <5% missing
# If not MCAR: Advanced imputation required
```

**2. Missing at Random (MAR) vs Missing Not at Random (MNAR):**
```r
# MAR assumption testing
library(mice)
# Create missing data indicators
data$missing_income <- is.na(data$income)
data$missing_education <- is.na(data$education)

# Test if missingness relates to observed variables
logistic_model <- glm(missing_income ~ age + gender + employment, 
                     data = data, family = "binomial")
summary(logistic_model)
```

#### **B. Advanced Imputation Techniques:**

**1. Multiple Imputation (Preferred Method):**
```r
library(mice)

# Set up imputation model
init <- mice(data, maxit=0)
predM <- init$predictorMatrix
meth <- init$method

# Customize imputation methods
meth["income"] <- "pmm"      # Predictive mean matching
meth["education"] <- "polr"   # Ordered logistic regression
meth["gender"] <- "logreg"    # Logistic regression

# Run multiple imputation
imputed_data <- mice(data, method=meth, 
                    predictorMatrix=predM, 
                    m=5, maxit=10, seed=123)

# Pool results for analysis
pooled_results <- pool(with(imputed_data, 
                           lm(outcome ~ predictor1 + predictor2)))
summary(pooled_results)
```

**2. AI-Enhanced Imputation:**
```r
# Use random forest for complex imputation
library(missForest)
data_imputed <- missForest(data, maxiter=10, ntree=100)

# Check imputation performance
data_imputed$OOBerror  # Out-of-bag error estimates
```

#### **💡 Hands-on Exercise 2A (30 menit):**
**"Missing Data Challenge"**

Peserta diberikan dataset dengan berbagai missing data patterns:
1. Diagnose missing data mechanism
2. Choose appropriate imputation strategy
3. Implement using R with AI guidance
4. Validate imputation quality

### **Part 3: Outlier Detection & Treatment (30 menit)**

#### **A. Multivariate Outlier Detection:**

**1. Statistical Distance Methods:**
```r
library(MVN)
library(robustbase)

# Mahalanobis distance
maha_dist <- mahalanobis(data[,numeric_vars], 
                        center=colMeans(data[,numeric_vars]), 
                        cov=cov(data[,numeric_vars]))

# Chi-square threshold for outlier detection
threshold <- qchisq(0.975, df=length(numeric_vars))
outliers_maha <- which(maha_dist > threshold)

# Robust outlier detection
outliers_robust <- which(robustbase::adjOutlyingness(data[,numeric_vars]) > 0.95)
```

**2. AI-Assisted Pattern Recognition:**
```r
library(isotree)

# Isolation forest for anomaly detection
iso_forest <- isolation.forest(data[,numeric_vars])
anomaly_scores <- predict(iso_forest, data[,numeric_vars])

# Identify outliers based on anomaly scores
outlier_threshold <- quantile(anomaly_scores, 0.95)
outliers_iso <- which(anomaly_scores > outlier_threshold)
```

#### **B. Treatment Strategies:**

**Decision Framework:**
```r
outlier_treatment <- function(data, outliers, method="investigate") {
  switch(method,
    "investigate" = {
      # Manual investigation approach
      cat("Outliers detected at positions:", outliers, "\n")
      cat("Investigate these cases manually\n")
      return(data)
    },
    "winsorize" = {
      # Winsorization approach
      library(DescTools)
      data_numeric <- data[,numeric_vars]
      data_winsorized <- data
      data_winsorized[,numeric_vars] <- Winsorize(data_numeric, probs=c(0.05, 0.95))
      return(data_winsorized)
    },
    "transform" = {
      # Log transformation for skewed data
      data_transformed <- data
      data_transformed[,numeric_vars] <- log1p(abs(data[,numeric_vars]))
      return(data_transformed)
    }
  )
}
```

### **Part 4: Variable Transformation & Normalization (15 menit)**

#### **A. Normality Assessment:**
```r
library(nortest)

# Multiple normality tests
normality_check <- function(x) {
  list(
    shapiro = shapiro.test(x)$p.value,
    anderson = ad.test(x)$p.value,
    kolmogorov = ks.test(x, "pnorm", mean(x), sd(x))$p.value
  )
}

# Apply to all numeric variables
norm_results <- lapply(data[,numeric_vars], normality_check)
```

#### **B. Transformation Strategies:**
```r
library(car)
library(forecast)

# Box-Cox transformation
lambda <- BoxCox.lambda(data$variable)
data$variable_transformed <- BoxCox(data$variable, lambda)

# Yeo-Johnson for data with zeros/negatives
data$variable_yj <- yjPower(data$variable, lambda=0.5)
```

---

## **📊 SESSION 1C: STATISTICAL THINKING REVOLUTION**
### **⏰ Durasi: 2 Jam (14:15-16:15)**

#### **🎯 Learning Outcomes:**
- Memahami dan menghindari p-hacking practices
- Menghitung dan menginterpretasi effect sizes
- Memahami pentingnya confidence intervals
- Menguasai statistical power dan sample size planning

#### **📚 MATERI INTI:**

### **Part 1: Beyond P-Hacking (45 menit)**

#### **A. Understanding P-Hacking:**

**Common P-Hacking Practices:**
```
❌ BAD PRACTICES:
1. Multiple testing without correction
2. Selective reporting of significant results
3. Data dredging/fishing expeditions
4. Post-hoc hypothesis generation
5. Selective outlier removal
6. Optional stopping rules

✅ GOOD PRACTICES:
1. Pre-registered analysis plans
2. Multiple testing corrections (Bonferroni, FDR)
3. Effect size reporting
4. Complete results reporting
5. Sensitivity analyses
6. Replication attempts
```

#### **B. Multiple Testing Corrections:**

**Implementation in R:**
```r
# Multiple comparison corrections
p_values <- c(0.01, 0.03, 0.045, 0.08, 0.12)

# Bonferroni correction (conservative)
p_bonferroni <- p.adjust(p_values, method="bonferroni")

# False Discovery Rate (FDR) - Benjamini-Hochberg
p_fdr <- p.adjust(p_values, method="fdr")

# Holm method (step-down)
p_holm <- p.adjust(p_values, method="holm")

correction_comparison <- data.frame(
  original = p_values,
  bonferroni = p_bonferroni,
  fdr = p_fdr,
  holm = p_holm
)
```

#### **C. Pre-registration Best Practices:**

**Analysis Plan Template:**
```
1. RESEARCH QUESTIONS:
   - Primary hypothesis (confirmatory)
   - Secondary hypotheses (exploratory)

2. SAMPLE SIZE JUSTIFICATION:
   - Power analysis details
   - Effect size assumptions
   - Alpha level and power

3. DATA ANALYSIS PLAN:
   - Primary analysis method
   - Handling of missing data
   - Outlier treatment strategy
   - Assumption checking procedures

4. VARIABLES:
   - Primary outcome measures
   - Secondary outcomes
   - Control variables
   - Moderator/mediator variables

5. STATISTICAL ANALYSIS:
   - Descriptive statistics plan
   - Inferential tests specified
   - Multiple testing corrections
   - Sensitivity analyses planned
```

### **Part 2: Effect Sizes & Practical Significance (30 menit)**

#### **A. Effect Size Categories:**

**1. Standardized Mean Differences:**
```r
library(effsize)

# Cohen's d for t-tests
d_effect <- cohen.d(group1, group2)

# Interpretation guidelines:
# Small: d = 0.2
# Medium: d = 0.5  
# Large: d = 0.8

# Hedges' g (corrected for small samples)
g_effect <- hedges.g(group1, group2)
```

**2. Variance Explained:**
```r
library(effectsize)

# Eta-squared for ANOVA
eta_sq <- eta_squared(aov_model)

# Partial eta-squared
partial_eta_sq <- eta_squared(aov_model, partial=TRUE)

# Omega-squared (less biased)
omega_sq <- omega_squared(aov_model)
```

**3. Correlation Effect Sizes:**
```r
# Correlation coefficient interpretation
# Small: r = 0.1
# Medium: r = 0.3
# Large: r = 0.5

# R-squared for regression
rsq <- summary(lm_model)$r.squared
rsq_adjusted <- summary(lm_model)$adj.r.squared
```

#### **B. Practical Significance Assessment:**

**Decision Framework:**
```r
practical_significance <- function(effect_size, context="education") {
  
  # Education-specific thresholds
  if(context == "education") {
    thresholds <- list(
      trivial = c(0, 0.15),
      small = c(0.15, 0.40),
      medium = c(0.40, 0.75),
      large = c(0.75, Inf)
    )
  }
  
  for(level in names(thresholds)) {
    if(effect_size >= thresholds[[level]][1] & 
       effect_size < thresholds[[level]][2]) {
      return(level)
    }
  }
}
```

### **Part 3: Confidence Intervals Mastery (30 menit)**

#### **A. CI Interpretation:**

**Common Misinterpretations:**
```
❌ WRONG: "95% probability that true value is in this interval"
✅ CORRECT: "95% of such intervals contain the true value"

❌ WRONG: "Significant if CI doesn't include 0"
✅ CORRECT: "Evidence against null if CI doesn't include null value"
```

#### **B. Bootstrap Confidence Intervals:**
```r
library(boot)

# Bootstrap function for mean
boot_mean <- function(data, indices) {
  return(mean(data[indices]))
}

# Perform bootstrap
boot_results <- boot(data$variable, boot_mean, R=1000)

# Calculate confidence intervals
boot_ci <- boot.ci(boot_results, type=c("norm", "basic", "perc", "bca"))
```

### **Part 4: Statistical Power & Sample Size (15 menit)**

#### **A. Power Analysis Implementation:**
```r
library(pwr)

# Power analysis for t-test
power_t <- pwr.t.test(d=0.5, sig.level=0.05, power=0.80, type="two.sample")

# Power analysis for ANOVA
power_anova <- pwr.anova.test(k=3, f=0.25, sig.level=0.05, power=0.80)

# Power analysis for correlation
power_corr <- pwr.r.test(r=0.3, sig.level=0.05, power=0.80)
```

---

## **📊 SESSION 1D: AI-ASSISTED PROBLEM DIAGNOSIS**
### **⏰ Durasi: 2 Jam (16:30-18:30)**

#### **🎯 Learning Outcomes:**
- Menggunakan Claude AI untuk evaluasi metodologi
- Mengidentifikasi research gaps dengan bantuan AI
- Melakukan literature review yang enhanced dengan AI
- Memperbaiki research questions menggunakan AI guidance

#### **📚 MATERI INTI:**

### **Part 1: Claude AI for Methodology Assessment (30 menit)**

#### **A. AI Prompting Strategies for Research:**

**1. Methodology Evaluation Prompt:**
```
SYSTEM PROMPT: "You are an expert research methodologist and 
statistical consultant. Review the following research design 
and provide detailed feedback on potential weaknesses and 
improvements needed for publication in a Q1 journal."

USER PROMPT: "Please evaluate this research methodology:

Research Question: [Your research question]
Sample: [Sample description]
Design: [Research design]
Analysis Plan: [Statistical analysis plan]
Variables: [List of variables]

Please assess:
1. Sample size adequacy
2. Research design appropriateness
3. Statistical analysis alignment
4. Potential confounding variables
5. Threats to validity
6. Suggestions for improvement"
```

**2. Statistical Analysis Guidance:**
```
PROMPT: "I have data with the following characteristics:
- Sample size: [N]
- Variables: [List with types]
- Research questions: [Questions]
- Data distribution: [Normal/skewed/etc]

What statistical tests should I use and why? 
Please provide R code examples and interpretation guidance."
```

#### **B. AI-Assisted Assumption Checking:**

**Collaborative Approach:**
```r
# 1. Run diagnostic tests
normality_tests <- lapply(data[,numeric_vars], shapiro.test)
homogeneity_test <- bartlett.test(outcome ~ group, data=data)
independence_check <- durbinWatsonTest(lm_model)

# 2. AI interpretation prompt
"Based on these diagnostic results:
- Shapiro-Wilk tests: [results]
- Bartlett test: [result]  
- Durbin-Watson: [result]

Are the assumptions for my planned ANOVA satisfied? 
If not, what alternatives should I consider?"
```

### **Part 2: Research Gap Identification (30 menit)**

#### **A. Systematic Gap Analysis with AI:**

**1. Literature Synthesis Prompt:**
```
PROMPT: "I'm researching [topic]. Here are key findings from 
recent studies:

Study 1: [Brief summary]
Study 2: [Brief summary]
Study 3: [Brief summary]

Please help me identify:
1. Methodological gaps in current research
2. Unexplored research questions
3. Limitations that create opportunities
4. Contradictory findings that need resolution
5. Potential theoretical contributions"
```

**2. Innovation Opportunity Mapping:**
```
PROMPT: "Given this research landscape in [field], what are:
1. Emerging methodological approaches I should consider?
2. Technological innovations applicable to this research?
3. Cross-disciplinary perspectives that could add value?
4. Underexplored populations or contexts?
5. Novel variables or measurements worth investigating?"
```

#### **B. Competitive Analysis Framework:**

**AI-Enhanced SWOT Analysis:**
```
PROMPT: "Analyze my research proposal's competitive position:

My Study: [Description]
Existing Research: [Key competitors]

Please provide:
- Strengths: What advantages does my approach have?
- Weaknesses: What limitations should I address?
- Opportunities: What gaps can I fill?
- Threats: What competing research might overshadow mine?
- Differentiation Strategy: How can I make this study unique?"
```

### **Part 3: Literature Review Enhancement (45 menit)**

#### **A. AI-Powered Literature Synthesis:**

**1. Systematic Review Protocol:**
```
PROMPT SEQUENCE:

1. Search Strategy Development:
"Help me develop search terms for: [research topic]
Include: synonyms, related terms, Boolean operators
For databases: PubMed, PsycINFO, ERIC, Web of Science"

2. Inclusion/Exclusion Criteria:
"Refine these criteria for literature selection:
- Time period: [years]
- Study types: [methodologies]
- Population: [characteristics]
- Outcomes: [measures]
What am I missing or should modify?"

3. Quality Assessment:
"Create a quality assessment checklist for:
- Quantitative studies: [criteria]
- Qualitative studies: [criteria]
- Mixed methods: [criteria]"
```

**2. Synthesis Matrix Development:**
```r
# Create literature matrix with AI guidance
literature_matrix <- data.frame(
  Author = character(),
  Year = numeric(),
  Sample_Size = numeric(),
  Design = character(),
  Key_Variables = character(),
  Main_Findings = character(),
  Effect_Size = numeric(),
  Quality_Score = numeric(),
  Limitations = character(),
  Relevance_Score = numeric()
)

# AI prompt for synthesis
"Based on this literature matrix, help me:
1. Identify patterns across studies
2. Synthesize contradictory findings
3. Assess overall evidence quality
4. Identify theoretical frameworks
5. Develop my theoretical model"
```

#### **B. Critical Analysis with AI:**

**Bias Detection Prompts:**
```
"Review these study findings for potential biases:

Studies: [List with brief descriptions]

Please identify:
1. Selection bias patterns
2. Publication bias indicators  
3. Measurement bias issues
4. Analytical bias concerns
5. Reporting bias evidence

How should I address these in my research design?"
```

### **Part 4: Research Question Refinement (15 menit)**

#### **A. PICO Framework Enhancement:**

**AI-Assisted Question Development:**
```
PROMPT: "Help me refine my research question using PICO:

Initial Question: [Your question]

Please help develop:
- Population: Who specifically should I study?
- Intervention/Exposure: What exactly am I examining?
- Comparison: What should I compare against?
- Outcome: What specific outcomes should I measure?

Make it more specific, measurable, and feasible."
```

#### **B. Hypothesis Generation:**

**Testable Hypothesis Development:**
```
PROMPT: "Convert my research question into testable hypotheses:

Research Question: [Question]
Theoretical Framework: [Framework]
Key Variables: [List]

Provide:
1. Primary hypothesis (directional)
2. Alternative hypotheses
3. Null hypotheses
4. Potential moderating effects
5. Mediating pathways to test"
```

---

## **🎯 PRACTICAL EXERCISES & ASSESSMENTS**

### **💡 Exercise 1: Methodology Diagnosis (45 menit)**
**Individual + AI Collaboration**

**Task:** Peserta membawa draft research proposal mereka
1. Input ke Claude AI untuk evaluation
2. Identify 5 major weaknesses
3. Develop improvement plan with AI guidance
4. Present findings to peer group

### **💡 Exercise 2: Data Quality Assessment (30 menit)**
**Hands-on R Implementation**

**Task:** Using provided messy dataset
1. Conduct comprehensive data audit
2. Identify and treat missing data
3. Detect and handle outliers
4. Document quality improvement process

### **💡 Exercise 3: Effect Size Calculation Workshop (30 menit)**
**Statistical Computation Practice**

**Task:** Calculate effect sizes for different scenarios
1. Two-group comparison (Cohen's d)
2. ANOVA results (eta-squared)
3. Correlation analysis (r-squared)
4. Interpret practical significance

### **💡 Exercise 4: Literature Gap Analysis (30 menit)**
**AI-Enhanced Research Planning**

**Task:** Systematic gap identification
1. Use AI to analyze 5 recent papers in your field
2. Identify methodological limitations
3. Propose innovative research directions
4. Develop competitive advantage strategy

---
### **SESSION 1D (tambahan): AI-ASSISTED PROBLEM DIAGNOSIS**
**🕐 Durasi: 2.5 Jam (16:00-18:30) - EXTENDED +30 minutes**

#### **🆕 NEW Part 1: Master Prompt Library Workshop (45 menit)**
**"From AI User to AI Research Director"**

##### **A. Framework Introduction & Practice (20 menit):**

**Live Demonstration - Methodology Assessment:**
```
INSTRUCTOR DEMO:
"Watch me direct AI as a Research Director, not just ask questions"

WEAK APPROACH (AI User):
"Help me analyze my data"

STRONG APPROACH (AI Research Director):
SYSTEM ROLE: "You are a senior research methodologist with 20+ years 
of experience reviewing manuscripts for Q1 journals. You specialize in 
identifying methodological weaknesses that lead to rejection."

RESEARCH CONTEXT:
- Discipline: [Participant's field]
- Research Question: [Specific question]
- Study Type: [Design type]
- Sample: [Characteristics]
- Analysis Plan: [Current approach]

ASSESSMENT FRAMEWORK:
1. DESIGN EVALUATION: Is this appropriate for the research question?
2. SAMPLING ANALYSIS: Are there selection bias concerns?  
3. STATISTICAL VALIDATION: Are planned analyses suitable?
4. PUBLICATION READINESS: What would Q1 reviewers criticize?

Please provide specific, actionable recommendations for publication success.
```

##### **B. Hands-On Practice Session (25 menit):**

**Individual Workshop:**
```
TASK: "Transform Your Research with AI Direction"

STEP 1: Participant brings their current research project
STEP 2: Use provided master prompt template for methodology assessment
STEP 3: Practice iterative prompt refinement based on AI responses
STEP 4: Share one key insight gained from AI consultation

SUCCESS CRITERIA:
✅ Participant can formulate strategic research prompts
✅ Demonstrates prompt refinement based on AI feedback
✅ Identifies specific methodology improvements
✅ Shows shift from "AI user" to "AI director" mindset
```

#### **🔄 REVISED Part 2: Cross-Disciplinary Prompt Adaptation (30 menit)**

##### **A. Disciplinary Specialization Practice (15 menit):**

**Business Research AI Director:**
```
SPECIALIZED PROMPT ADDITION:
"Additionally consider for business research:
- Stakeholder impact analysis requirements
- Practical business significance thresholds  
- ROI and cost-benefit considerations
- Market research vs organizational research norms
- Regulatory compliance considerations
- Scalability for business implementation"
```

**Engineering Research AI Director:**
```
SPECIALIZED PROMPT ADDITION:
"Additionally consider for engineering research:
- Technical performance metrics and standards
- Reliability and validity in engineering contexts
- Safety and quality control requirements
- Industry standards and regulatory compliance  
- Technical innovation vs incremental improvement
- Scalability and implementation feasibility"
```

**Health Research AI Director:**
```
SPECIALIZED PROMPT ADDITION:
"Additionally consider for health research:
- Clinical significance vs statistical significance
- Patient safety and ethical considerations
- Regulatory requirements (FDA, IRB, clinical guidelines)
- Healthcare delivery and implementation science
- Cost-effectiveness and health economics
- Translation from research to practice"
```

##### **B. Cross-Disciplinary Workshop (15 menit):**

**Group Exercise:**
```
CHALLENGE: "The Adaptation Expert"
1. Form groups by discipline (Business/Engineering/Health/Social Science)
2. Each group adapts master methodology prompt for their field
3. Test adapted prompts with AI consultation
4. Present field-specific insights to other groups
5. Build cross-disciplinary prompt library together

DELIVERABLE: Disciplinary-adapted master prompt ready for use
```

#### **🆕 NEW Part 3: Iterative AI Refinement Mastery (30 menit)**

##### **A. Prompt Evolution Strategy (15 menit):**

**The Refinement Cycle:**
```
INITIAL PROMPT → AI RESPONSE → ANALYSIS → REFINED PROMPT → BETTER RESPONSE

REFINEMENT STRATEGIES:
1. SPECIFICITY INCREASE:
   - Add context details
   - Include constraints  
   - Specify output format

2. ROLE ENHANCEMENT:
   - Strengthen system role definition
   - Add expertise specifications
   - Include perspective requirements

3. FRAMEWORK DEEPENING:
   - Add assessment criteria
   - Include decision frameworks
   - Specify evaluation standards

4. OUTCOME OPTIMIZATION:
   - Define success metrics
   - Add quality checkpoints
   - Include validation requirements
```

##### **B. Live Refinement Practice (15 menit):**

**Real-Time Workshop:**
```
EXERCISE: "The Prompt Evolution Challenge"

Round 1: Use basic research question with AI
Round 2: Apply refinement strategies and compare results  
Round 3: Cross-validate improvements with peer feedback

ASSESSMENT:
- Quality improvement in AI responses
- Specificity increase in recommendations
- Actionability enhancement in outputs
- Publication relevance improvement
```

#### **🆕 NEW Part 4: AI Research Director Mindset Assessment (15 menit)**

##### **Transformation Checkpoint:**
```
SELF-ASSESSMENT FRAMEWORK:

BEFORE (AI User Mindset):
❌ "I ask AI to help with my analysis"
❌ "I hope AI gives me the right answer"  
❌ "I accept whatever AI provides"
❌ "I use AI like Google search"

AFTER (AI Research Director Mindset):
✅ "I direct AI as my research consultant"
✅ "I guide AI toward specific expertise I need"
✅ "I validate and refine AI recommendations"  
✅ "I use AI as strategic research partner"

COMPETENCY CHECKLIST:
□ Can formulate strategic research prompts
□ Understands system role importance
□ Practices iterative prompt refinement
□ Validates AI outputs with domain knowledge
□ Adapts prompts for disciplinary context
□ Focuses on publication-ready outcomes
```

---

## **📚 RECOMMENDED READINGS**

### **Essential Papers:**
1. Button, K. S. et al. (2013). Power failure: Why small sample size undermines the reliability of neuroscience. *Nature Reviews Neuroscience, 14*(5), 365-376.

2. Lakens, D. (2013). Calculating and reporting effect sizes to facilitate cumulative science: A practical primer for t-tests and ANOVAs. *Frontiers in Psychology, 4*, 863.

3. Wasserstein, R. L., & Lazar, N. A. (2016). The ASA statement on p-values: Context, process, and purpose. *The American Statistician, 70*(2), 129-133.

### **Technical Resources:**
1. **R Documentation:** Multiple imputation with mice package
2. **Statistical Power:** G*Power manual and tutorials
3. **Effect Sizes:** effectsize package documentation
4. **AI Ethics:** Guidelines for AI use in academic research

### **Online Tools:**
1. **Power Analysis:** G*Power software
2. **Effect Size Calculator:** Campbell Collaboration calculator
3. **Missing Data:** Multiple imputation guidelines
4. **AI Assistance:** Claude AI best practices guide

---

## **🏁 DAY 1 WRAP-UP**

### **Key Takeaways:**
1. **Quality Mindset:** Shift from "getting significant results" to "conducting robust research"
2. **AI Integration:** Leverage AI as analytical partner, not replacement for thinking
3. **Statistical Rigor:** Emphasis on effect sizes, confidence intervals, and practical significance
4. **Reproducibility:** Document all decisions and analysis steps

---
