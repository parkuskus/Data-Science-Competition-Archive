Overview
📊 Data Analysis Competition (DAC) 2025
🔎 Overview
The Data Analysis Competition (DAC) 2025 is a university-level data analysis competition across Southeast Asia.
Held as part of the *Statistics Fair Week (Pekan Raya Statistika 2025), this event provides a platform for students to showcase their skills in *data analysis, critical thinking, and problem-solving with real-world applications in Business Economics.

With the theme "Decoding Consumer Behaviour: Statistical Approaches for Digital Market Insights," the competition invites both Statistics and Non-Statistics students to participate in a two-stage challenge—preliminary and finals. The top 10 teams will advance to the final round, held offline at the Department of Statistics, Sepuluh Nopember Institute of Technology (ITS), Indonesia.

🎯 Goal
The goal of DAC 2025 is to:

Strengthen participants’ ability to analyze data rigorously and derive meaningful insights.
Encourage critical thinking and application of statistics to solve societal problems.
Enhance knowledge of statistical methods applied to digital market and consumer behaviour.
Provide participants with the opportunity to communicate findings effectively through reports and presentations.
Share the best analysis results with the public through the PRS 2025 website and social media.
Start

Aug 21, 2025
Close

Aug 31, 2025
Description
As Large Language Models (LLMs) such as GPT, Claude, Gemini, and others continue to grow in popularity, they are increasingly embedded in diverse real-world applications—from chat assistants and educational tools to productivity software and creative writing aids. With this rapid adoption, user reviews have emerged as one of the most direct and unfiltered sources of feedback for developers.

These reviews capture not only surface-level opinions (e.g., “good app” or “buggy update”) but also subtle sentiment signals that reflect deeper user experiences—such as trust, satisfaction, frustration, or skepticism toward the technology. For LLM developers, being able to systematically understand and monitor sentiment trends is critical for:

Identifying strengths and weaknesses across different versions of their applications.
Detecting user pain points that may not be visible through quantitative usage metrics.
Benchmarking against competitors in the LLM ecosystem.
Guiding future improvements in usability, transparency, and ethical considerations.
However, the challenge is non-trivial. The sheer volume of textual reviews makes manual analysis infeasible. Furthermore, sentiment expressed in reviews is often nuanced, context-dependent, and noisy. For example:

A single review may contain both positive and negative aspects, requiring a mixed or neutral classification.

Informal writing styles (slang, sarcasm, emojis, or abbreviations) introduce ambiguity for traditional text-processing pipelines.

Distribution of sentiment is highly imbalanced, as satisfied users tend to leave disproportionately more positive feedback compared to dissatisfied or neutral ones.

This task challenges participants to go beyond simple keyword matching and instead build an automated sentiment classification system that can:

Generalize across multiple LLM platforms (GPT, Claude, Gemini, Grok, Perplexity, DeepSeek).

Handle imbalanced data effectively to avoid bias toward the dominant “positive” class.

Produce reliable and fair classifications across the three sentiment categories:

0 → Negative

1 → Neutral / Mixed

2 → Positive

By developing a model that meets these requirements, participants will demonstrate not only their technical ability in natural language processing (NLP) but also their critical understanding of real-world deployment challenges—where fairness, robustness, and generalizability are as important as accuracy.

Submission
Submissions are evaluated using an F1 score defined as:


Submission File
For each CommentIdin the test set, you must predict a probability for the Sentiment variable. The file should contain a header and have the following format:

CommentId, Sentiment
gpt_1,0
gpt_2,0
gpt_3,0
etc.
Insights from User Reviews
Comparative Analysis of Negative Reviews — “What Do Users Dislike About Specific AI Apps?”
Background
While overall sentiment trends offer a broad view of user satisfaction, negative reviews often contain the most actionable insights. Users who leave negative feedback highlight pain points that may not appear in quantitative metrics or average ratings—issues like slow response times, factual inaccuracies, confusing interfaces, or privacy concerns.

For LLM developers, understanding these complaints is essential to:

Prioritize bug fixes and feature improvements.

Enhance user experience and trust in the application.

Benchmark against competitors by identifying recurring issues unique to each platform.

However, analyzing negative reviews poses unique challenges:

Reviews may be short, informal, or context-dependent, making automated theme extraction difficult.

The vocabulary across different LLM platforms can vary, so direct keyword matching may miss nuanced complaints.

Versioning differences may change the nature of complaints, requiring careful comparison across app releases.

In this task, participants are challenged to build a comparative analysis framework that can:

Extract and consolidate negative reviews from multiple LLM platforms (GPT, Claude, Gemini, Grok, Perplexity, DeepSeek).

Identify recurring themes or keywords using techniques such as TF-IDF, LDA topic modeling, or similar methods.

Compare complaints across platforms and across app versions to uncover trends, shifts, or persistent issues.

By completing this task, participants will demonstrate their ability to perform advanced text analysis, uncover hidden insights from unstructured data, and provide actionable intelligence to developers.

Sentiment Analysis Across App Versions — “Do Certain LLM Versions Correlate With More Positive or Negative Sentiment?”
Background
Large Language Model (LLM) applications, such as GPT, Claude, and Gemini, are frequently updated to improve performance, add features, or fix bugs. While new versions often bring enhancements, they may also introduce unintended issues that affect user satisfaction.

User reviews provide a direct window into how version updates impact sentiment, allowing developers to monitor trends, detect regression problems, and assess the reception of new features.

Key motivations for this analysis include:

Understanding version-specific performance: Some updates may improve functionality, while others may degrade user experience.

Prioritizing improvements: Identifying versions that receive unusually high negative feedback helps developers focus on critical pain points.

Benchmarking over time: Tracking sentiment across versions provides a longitudinal view of product quality and user satisfaction.

Challenges participants may face in this task:

Inconsistent version formats (e.g., “v2.1” vs “2.1.0”) complicate aggregation.

Imbalanced sentiment distribution can bias analyses if not properly handled.

Reviews may contain mixed or nuanced opinions, requiring careful classification before version-level aggregation.

In this task, participants are challenged to:

Aggregate sentiment data across multiple LLM versions.

Analyze whether specific versions are associated with significant shifts in sentiment using statistical methods (e.g., Chi-Square Test, ANOVA).

Visualize trends and identify versions that require further investigation based on review content or sudden sentiment changes.

By completing this task, participants will demonstrate their ability to link model outputs with product lifecycle insights, applying data analytics to monitor and improve real-world software deployments.

Answer Sheets and Submission Links
The Answer Sheets can be accessed through the following link:
Answer Sheet Late or multiple submissions will not be accepted. Thank you for your cooperation, and we wish you the best of luck in the competition.

The submission portal for the Answer Sheets, Source Code, and CSV file can be accessed through the following link:
Submit Your Answer Please ensure that all files are complete and accurate before submitting.

Dataset Description
The dataset contains user comments on several AI-related applications (e.g., GPT, Claude, Gemini, Perplexity, Grok, DeepSeek). The primary goal is to classify the sentiment of each comment into three categories. This dataset can be used to train and evaluate sentiment analysis models in English/Indonesian user-generated text.

Files
train.zip - Train.zip → the training set (January 2024 – July 2025), containing Comment, At, AppVersion, Sentiment, and source.
test.zip - the test set (same period), containing CommentId, Comment, At, AppVersion, and source
sample_submission.csv - a sample submission file in the correct format


sample_submission.csv(604.09 kB)

2 of 2 columns


CommentId

Sentiment
45971

unique values
Label	Count
0.00 - 0.00	45,971
0
0
gpt_1
0
gpt_2
0
gpt_3
0
gpt_4
0
gpt_5
0
gpt_6
0
gpt_7
0
gpt_8
0
gpt_9
0
gpt_10
0
gpt_11
0
gpt_12
0
gpt_13
0
gpt_14
0
gpt_15
0