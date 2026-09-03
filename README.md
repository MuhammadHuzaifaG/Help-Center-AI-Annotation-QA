# Help-Center-AI-Annotation-QA

A practical dataset of customer support messages annotated for **Intent Classification**, **Sentiment Analysis**, and **PII (Personally Identifiable Information) Detection**. Built for fine-tuning natural language understanding (NLU) models and optimizing automated ticket routing workflows.

---

## 1. Project Overview

Customer support teams handle thousands of incoming queries daily across mobile apps, web forms, and social channels. Routing these tickets manually takes time and increases response latency. 

This project delivers a multi-task annotated text dataset designed to:
- Automatically route customer tickets to the correct backend team (Billing, Tech Support, Account Management).
- Detect customer sentiment to prioritize urgent or dissatisfied cases.
- Flag PII in real time to ensure compliance with data privacy standards before storing logs.

---

## 2. Dataset Description

The dataset consists of customer support queries across various service domains (e-commerce, ride-sharing, travel, and software services).

* **Format:** CSV / JSON
* **Sample Count:** 50 curated and audited records (representative subset of a 500-item dataset)
* **Languages:** English
* **Fields Included:**
  * `record_id`: Unique tracking identifier for each ticket.
  * `text`: Raw message content submitted by the user.
  * `intent`: Primary category of the query.
  * `sentiment`: Perceived emotional tone of the user.
  * `contains_pii`: Boolean flag indicating whether personal data is present.
  * `qa_status`: Audit decision (`Keep`, `Fix`, or `Flag`).

---

## 3. Labeling Taxonomy & Information

Annotators evaluated each message across three main tasks using the following schema:

### Intent Classification (Multi-class)
- **Billing:** Charges, refunds, payment failures, invoice requests, or fee disputes.
- **Technical:** Application crashes, bugs, timeout errors, performance issues, or feature glitches.
- **Account:** Password resets, email updates, login issues, security alerts, or account lockouts.
- **Inquiry:** General questions regarding features, policies, or operating hours.
- **Other:** Non-actionable messages, feedback, compliments, or general chat.

### Sentiment Analysis (Single-label)
- **Positive:** Expresses satisfaction, gratitude, or praise.
- **Neutral:** Informational queries without strong emotional charge.
- **Negative:** Frustration, dissatisfaction, complaints, or reports of broken/blocked functionality.

### PII Detection (Binary)
- **True:** Contains names, phone numbers, email addresses, street addresses, or unique transaction/account IDs.
- **False:** Contains no identifying user or account details.

---

## 4. Annotation Guidelines

To keep labels consistent across annotators, we applied these specific edge-case rules:

1. **Actionable Issue Priority:** When a message mentions multiple topics (e.g., "The app crashed while paying"), label the intent by the main goal (e.g., `Technical` if reporting the bug, `Billing` if asking about a double charge).
2. **Functional Failure = Negative Sentiment:** If an app error or system failure prevents a user from completing an action, mark sentiment as `Negative` even if the language used is polite.
3. **Transaction Identifiers as PII:** Order numbers, flight confirmation codes, and listing IDs count as `PII = True` because they link directly to an individual's activity.

---

## 5. Dataset Sample

Below is a 5-row sample extracted directly from the audited dataset:

| Record ID | Raw Text Sample | Intent | Sentiment | Contains PII | QA Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **TW-10010** | "App location tracking is 2 miles off. Call me at 415-555-0188." | Technical | Negative | True | Keep |
| **TW-10018** | "Host charged $200 cleaning fee... Account name: Robert Johnson." | Billing | Negative | True | Keep |
| **TW-10036** | "Changing primary email... for user Marcus Vance at 742 Evergreen Terrace." | Account | Neutral | True | Keep |
| **TW-10041** | "Shouting out flight attendant Sarah on DL1042 for being so kind!" | Other | Positive | False | Keep |
| **TW-10015** | "App keeps throwing network timeout errors while attempting to request ride." | Technical | Negative | False | Fix |

---

## 6. QA Audit Summary

A Quality Assurance audit was conducted on the annotated dataset to ensure high training data fidelity.

* **Dataset Accuracy Rate:** 92.0%
* **Inter-Annotator Agreement:** 88.5%
* **Key Findings:**
  - Most corrections (10%) stemmed from labelers under-reporting non-standard PII (such as order numbers) or marking functional failures as `Neutral` instead of `Negative`.
  - Guidelines were updated to explicitly define transaction IDs as PII and establish automatic `Negative` sentiment defaults for system blockers.
