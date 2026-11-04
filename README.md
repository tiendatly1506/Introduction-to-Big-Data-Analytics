# Introduction to AI and Big Data Analytics

Two projects from my Bachelor of Business Analytics covering big data tools and machine learning fundamentals: working with a large document database in MongoDB, and applying classification trees and MapReduce to worked problems. Each project has a PDF write-up that shows the method and working, not just the answer.

| Project | Topic | File |
|---|---|---|
| 1. IMDb data analysis with MongoDB | Database design, schema validation, data cleaning, aggregation pipelines | [IMDb MongoDB Analysis.pdf](./IMDb%20MongoDB%20Analysis.pdf) |
| 2. Classification tree and MapReduce | Model evaluation, overfitting, cross-validation, distributed processing | [Classification Tree and MapReduce.pdf](./Classification%20Tree%20and%20MapReduce.pdf) |

---

## 1. IMDb Data Analysis with MongoDB

**What I did:** Loaded the IMDb titles and ratings datasets (about 1.7 million title documents) into MongoDB using Compass, designed a JSON Schema for each collection, resolved the data quality problems that came up on import, profiled the data, and wrote aggregation pipelines to answer business questions.

**Handling messy data.** The first strict schema rejected a large share of the rows. I diagnosed each failure from the import log and adjusted the schema step by step:

| Problem | Fix |
|---|---|
| Missing values stored as the text `\N` in numeric fields | Allowed `int` or `string` for year and runtime fields |
| Numeric titles (such as "1900") inferred as numbers | Allowed string, int and double on title fields |
| Titles such as "True" or timestamps parsed as booleans or dates | Extended the allowed types to include `bool` and `date` |
| Empty titles imported as `null` | Made the title optional and allowed `null`, so no records were lost |

**Data profiling highlights**
- Ratings cluster between about 6.0 and 8.5, while vote counts have an extreme long tail (5 to 295,854 votes in the sample).
- Production volume grows sharply from the late 1990s, with records going back to 1896.
- Short-form content (episodes and shorts) far outnumbers feature films, and about 97% of titles are non-adult.
- Runtime and end year are mostly missing, reflecting ongoing TV series and historical films.

**Aggregation pipelines**
- Movies released each year (`$match`, `$sortByCount`)
- Top 5 Fantasy and Adventure movies of 2021 by rating (`$match`, `$lookup`, `$unwind`, `$sort`, `$limit`, `$project`)
- Top 5 Fantasy and Adventure movies of 2021 by number of votes

**Skills and tools:** MongoDB, Compass, JSON Schema validation, data import troubleshooting, handling missing and mixed-type data, schema profiling, aggregation pipelines.

---

## 2. Classification Tree and MapReduce

**Classification tree (hiking decision)**
- Counted the training rows at each leaf of a decision tree and assigned each leaf its majority class.
- Evaluated the model with confusion matrices: 87.5% accuracy on the training data (TP 4, FP 1, TN 3, FN 0) and 80% on the test data (TP 2, FP 0, TN 2, FN 1).
- Diagnosed **overfitting**: several leaves held a single training row, and accuracy dropped on unseen data. Proposed pruning with cross-validation, or limiting tree depth and minimum split size.
- Explained how 3-fold cross-validation works on the combined 13 records.

**MapReduce word count on two machines**
- Designed the map, shuffle and reduce stages for counting words across documents while ignoring stop words, with keys, values and the grouping step spelled out.
- Balanced the reduce stage by sending 3 keys to each of the two machines.
- Distributed five documents across two machines (3 and 2) to keep the map workload as even as possible, and showed why an uneven split would be slower.

**Skills and tools:** Decision trees, confusion matrices, accuracy, overfitting and pruning, cross-validation, MapReduce design, workload balancing in distributed systems.

---
