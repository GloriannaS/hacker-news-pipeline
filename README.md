# Hacker News Pipeline

An end-to-end functional data pipeline that ingests, cleans, transforms, and analyzes thousands of 2014 Hacker News submission records to extract top tech industry trends.

Built with Python using a modular, custom decorator-driven pipeline framework (`Pipeline`), this project demonstrates stream processing, in-memory IO optimization, lazy evaluation with generators, and natural language text normalization.

---

## 📌 Project Overview

Think of this pipeline as a digital assembly line:

1. **Ingest** raw JSON submission payloads.


2. **Filter** out noise and low-engagement posts.


3. **Transform** structured data into an in-memory CSV format.


4. **Normalize** headline text strings.


5. **Aggregate** keyword frequencies into actionable trend intelligence.



By chaining decoupled task functions through dependency injection (`depends_on`), each stage of the data transformation remains modular, memory-efficient, and easy to maintain or debug.

---

## 🛠️ Pipeline Architecture & Workflow

The pipeline consists of seven sequential task nodes:

| Stage | Task Function | Description |
| --- | --- | --- |
| **1. Load Data** | `file_to_json` | Ingests `hn_stories_2014.json` and extracts raw story dictionaries into memory.
| **2. Filter Signal** | `filter_stories` | Retains high-engagement stories (>50 points, >1 comment) while removing "Ask HN" posts.
| **3. Tabular Convert** | `json_to_csv` | Parses timestamps into `datetime` objects and writes records to an in-memory `io.StringIO` CSV buffer.
| **4. Extract Titles** | `extract_titles` | Reads the in-memory CSV stream and yields post titles using Python generators.
| **5. Text Normalization** | `clean_title` | Converts titles to lowercase and strips non-alphanumeric characters for clean token matching.
| **6. Word Frequencies** | `build_keyword_dictionary` | Tokenizes titles, filters out generic `stop_words`, and calculates word occurrences.
| **7. Extract Trends** | `top_keywords` | Sorts frequency dictionaries to output the top 100 dominant tech keywords.

---

## 📊 Key Findings (2014 Snapshot)

Analyzing the pipeline output provides a compelling look into developer interests and tech news from 2014:

* **Product Launches:** `"new"` leads all keywords (7 occurrences), showcasing Hacker News' focus on major release announcements.


* **Tech Giants & Languages:** Heavy focus on corporate juggernauts (`"google"`, `"apple"`, `"amazon"`) alongside core technologies (`"python"`, `"web"`, `"data"`, `"software"`, `"git"`).


* **Era-Specific Topics:** Captures historical moments such as security concerns (`"truecrypt"`), AWS infrastructure (`"useast1"`), mobile OS shifts (`"android"`), and early autonomous vehicle chatter (`"selfdriving"`).



---

## 💡 Key Technical Features

* **Functional DAG Architecture:** Uses custom `@pipeline.task(depends_on=...)` decorators to enforce dependency graphs across execution nodes.


* **Memory-Efficient I/O:** Utilizes `io.StringIO` to buffer CSV objects in memory rather than writing unnecessary temporary files to disk.


* **Stream Processing:** Employs Python generators (`yield`) to stream title strings downstream lazily.


* **Robust Text Sanitization:** Standardized tokenization logic via character-level filtering (`c.isalnum() or c.isspace()`) and stop-word elimination.

---

