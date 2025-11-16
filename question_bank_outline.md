# Question Bank Outline & Design  

## Overview
This project implements a structured question bank for storing and organizing survey questions, response options, and metadata. The goal is to create a clean, normalized system that supports easy analysis, transparency, and scalability. The question bank is built entirely in Python using `pandas`, following relational database principles, and can be created in SQL as well. I opted for Python to make sure the code was reproducible without any server connection issues for the sake of this assessment. 

---

## Data Model

The question bank uses two interlinked tables, structured similarly to a relational schema.

### 1. `questions` Table
Stores the main attributes of each survey question:

| Column | Description |
|--------|-------------|
| `question_id` | Unique identifier for the question |
| `question_text` | Exact wording shown to respondents |
| `question_type` | Automatically inferred type (e.g., “single choice,” “multi-select”) |
| `field_date` | Date the question was fielded |

A helper function infers `question_type` based on patterns in the wording (e.g., “select all,” “approve/disapprove”).

---

### 2. `response_options` Table
Stores one row per answer choice:

| Column | Description |
|--------|-------------|
| `question_id` | Foreign key linking to the `questions` table |
| `option_text` | The response option text |
| `option_order` | The order the option appeared in the survey |

Response options come from a single pipe-separated field and are expanded into structured rows.

---

## Rationale for This Structure

- **Normalized:** Questions and options are split into two tables to prevent duplication.  
- **Flexible:** Can handle new surveys or question formats without schema redesign.  
- **Analysis-friendly:** Works smoothly with pandas joins and survey data pipelines.  
- **Portable:** No external database required; the entire question bank is represented as two DataFrames.

---

## ERD (Entity-Relationship Diagram)

+-----------------+ 1-to-many +----------------------+
| questions |-------------------------->| response_options |
+-----------------+ +----------------------+
| question_id PK | | option_id (optional) |
| question_text | | question_id FK |
| question_type | | option_text |
| field_date | | option_order |
+-----------------+ +----------------------+
