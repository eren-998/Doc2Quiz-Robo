# 🤖 Doc2Quiz Agent

[![Platform](https://img.shields.io/badge/Platform-Mulerun-brightgreen?style=for-the-badge)](https://mulerun.com/)
[![n8n](https://img.shields.io/badge/Workflow-n8n-blueviolet?style=for-the-badge)](https://n8n.io/)
[![AI Model](https://img.shields.io/badge/Model-Google%20Gemini-blue?style=for-the-badge)](https://deepmind.google/technologies/gemini/)
[![API](https://img.shields.io/badge/API-ConvertAPI-orange?style=for-the-badge)](https://www.convertapi.com/)

This repository contains the complete n8n workflow source for the **Doc2Quiz Agent**, an automated 'Document-to-Test' generator deployed on [Mulerun](https://mulerun.com/).

> **➡️ Interact with the live agent:** <https://mulerun.com/@rudntamug659yai/doc2quiz-robo>

---

## 🤖 Overview

The Doc2Quiz Agent is designed to transform passive reading into active learning. It ingests any user-provided document or text and converts it into **two distinct, high-value learning tools**:

1.  A **beautiful, interactive HTML quiz** for active recall and self-assessment.
2.  A **clean, print-ready PDF** for offline study or sharing.

The agent's core competency is its ability to generate high-quality, relevant questions by *strictly* adhering to user-defined parameters (`Scope`, `Difficulty`, `Quantity`), ensuring a fair and focused test of knowledge.

---

## ✨ Core Functionality

* **Dual Output (HTML + PDF):** 🕹️ + 📄 Generates an interactive web quiz *and* a portable PDF from a single run.
* **Critical Scope-Lock:** 🧭 The AI *only* uses the section you define (e.g., "Page 5", "Introduction section"). No off-topic questions, guaranteed.
* **Input Flexibility:** Supports all major file formats (PDF, TXT, DOCX, DOC, MD, CSV) plus a **direct text paste option**.
* **Smart Difficulty Engine:** Creates questions that precisely match your 'Easy' (recall), 'Medium' (inference), or 'Hard' (synthesis) requirements.
* **Feasibility Checks:** 🛟 Politely warns you if the specified scope is too short to generate the requested number of questions or too simple for 'Hard' difficulty.
* **Interactive Dashboard:** 📊 The HTML quiz finishes with a full report, including a score circle, total correct/incorrect, percentage, and a complete question-by-question review.

---

## 🛠️ Tech Stack & Workflow Architecture

This agent is built entirely within **n8n** and orchestrates several services:

1.  **Input & Branching (Form Trigger & IF):** An n8n `FormTrigger` node captures the user's inputs (`file`, `Scope`, etc.). An `IF` node then routes the workflow based on whether the user provided a `file` or `pasted text`.
2.  **AI Analysis Engine (Google Gemini):** The core `GoogleGemini` node (or `Agent` node for text) analyzes the source content. Its system prompt orders it to act as an "Expert Document Analyst" and generate a structured Markdown quiz strictly following the user's `Scope`, `Difficulty`, and `Quantity` rules.
3.  **PDF Generation (ConvertAPI):** The workflow converts the AI's Markdown output into basic HTML using a `Code` node. This HTML is then sent via an `HttpRequest` to the **ConvertAPI** endpoint, which returns a URL for a clean, shareable PDF.
4.  **Interactive HTML Generation (AI Agent):** A *second* `Agent` node (with the "MCQ to HTML Quiz Generator" prompt) is triggered. It receives two key inputs: the original Markdown quiz from Step 2 and the new PDF URL from Step 3.
5.  **Report Aggregation & Output:** This second agent builds the final, complex, interactive HTML page, complete with CSS styling, quiz logic, and a "Download PDF" button that links to the URL generated in Step 3. This complete HTML file is the final output served to the user.

---

## 🚀 How to Use (Kaise Istemal Karein)

Agent ko istemal karna bahut saral (simple) hai. Yeh 6-step process follow karein:

1.  **Content Provide Karein:** Ya toh apni file (PDF, DOCX, TXT, etc.) **`file`** input mein upload karein, ya seedha **`Paste Your Text`** field mein apna content paste karein.
2.  **Difficulty Set Karein:** **`DIFFICULTY LEVEL`** dropdown se apni zaroorat ka level chunein (Easy, Medium, ya Hard).
3.  **Quantity Define Karein:** **`QUANTITY OF MCQs`** field mein batayein ki aapko kitne sawaal (questions) chahiye.
    > *Examples: "10", "25", "50"*
4.  **Scope Lock Karein:** AI ko batayein ki content ka *sirf* kaunsa hissa istemal karna hai. Yeh **`Scope`** field mein likhein. Yeh sabse zaroori step hai.
    > *Examples: "all", "Page 1", "Introduction section", "Chapter 3"*
5.  **Agent Run Karein:** "Run" (ya "Submit") button par click karein aur agent ko apna kaam karne dein.
6.  **Outputs Receive Karein:** Agent aapki request process karega aur ek interactive `.html` file output mein dega.
    * Us `.html` file ko apne browser mein kholein aur apna quiz shuru karein.
    * Quiz ke final result screen par aapko "Download PDF" ka button mil jaayega, jisse aap offline version download kar sakte hain.

---

## 📋 Input Schema (Example Prompts)

**• file:** (File) Upload your document.
> *Supported: PDF, TXT, DOCX, DOC, MD, CSV*

**• Paste Your Text:** (String) (Optional) Paste text here if not uploading a file.
> *Example: "The quick brown fox jumps..."*

**• DIFFICULTY LEVEL:** (Dropdown) Select the question difficulty.
> *Examples: "EASY", "MEDIUM", "HARD"*

**• QUANTITY OF MCQs (Maximum 50):** (Number) The number of questions to generate.
> *Examples: "10", "25", "50"*

**• Scope:** (String) The *exact* part of the document to use.
> *Examples: "all", "Page 1", "Introduction section", "The section about marketing"*

---

## 📂 Repository Structure

* `Doc2Quiz_n8n_Workflow.json`: The complete, importable n8n workflow file (I've renamed your `Doc2Quiz (2).json` for clarity).
* `README.md`: This documentation.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
