# AI-Driven Threat and Mitigation Report Generation Process

## 1. Overview

This repository contains a structured process and a set of templates for using a Large Language Model (LLM) to generate a comprehensive Threat and Mitigation Report for embedded systems. The methodology is grounded in the MITRE EMB3D framework and is designed to accelerate the creation of detailed, accurate, and standardized security documentation early in the product development lifecycle.

The core principle is to treat the LLM as a "Cybersecurity Analyst Co-pilot." By providing the AI with high-quality source material (engineering documents, a threat framework) and a detailed prompt, it can perform the heavy lifting of analysis, mapping, and report generation. The human expert then reviews, refines, and validates the output.

## 2. The Workflow

The process follows these key phases:

1.  **System Analysis:** The AI ingests all provided engineering documents to build a deep understanding of the product's architecture, components, and features.
2.  **Asset Identification:** The AI identifies the system's critical assets—the key things that must be protected.
3.  **Threat Modeling:** Using the MITRE EMB3D framework (provided as a STIX JSON file), the AI maps the system's properties to known threats.
4.  **Mitigation Mapping:** The AI links the identified threats to their corresponding mitigations as defined in the framework.
5.  **Report Generation:** The AI populates a detailed report template with all the synthesized information, including justifications and component-specific details.
6.  **Human Review & Refinement:** The human subject matter expert reviews the generated report for accuracy, completeness, and context, making corrections and refinements as needed.

## 3. Prerequisites

Before starting, you will need:
* **Access to a capable Large Language Model** (e.g., Gemini, ChatGPT-4).
* **A complete set of product engineering documents** (e.g., `.docx`, `.pptx`, specifications, schematics). The more detailed the documents, the better the output.
* **The MITRE EMB3D Framework** as a STIX JSON file. The version used in this process is `emb3d-stix-2.0.1.json`.
* The `ai-prompt.md` and `report-template.md` files from this repository.

## 4. Step-by-Step Instructions

**Step 1: Gather Your Inputs**
* Create a working directory for your new product.
* Copy the `ai-prompt.md`, `report-template.md`, and `dfd-template.md` files from this repository into your directory.
* Collect all relevant engineering documents for your product and place them in the same directory.
* Download the latest `emb3d-stix-2.0.1.json` file from the official source or use the one in this repository.

**Step 2: Prepare the AI Prompt**
* Open `ai-prompt.md`.
* Review the prompt and replace any generic placeholders like `[Product Name]` with the actual name of your product. While the prompt is designed to be generic, you can add more specific context if needed.

**Step 3: Initiate the AI Session**
* Start a new conversation with your chosen LLM.
* Begin by uploading all the necessary files:
    * All engineering documents.
    * The `emb3d-stix-2.0.1.json` file.
    * The `report-template.md` file.
* Once the files are uploaded and acknowledged by the AI, copy the entire contents of your prepared `ai-prompt.md` and paste it into the chat.

**Step 4: Review and Refine**
* The AI will generate a complete, filled-in report. Treat this as a high-quality first draft.
* Carefully review every section, especially the threat mappings and justifications in Section 7 and the comprehensive model in Section 10.
* Engage in a conversational feedback loop with the AI to correct any misinterpretations, add missing context, or reformat sections as needed. Remember, the human expert is the final validator.

## 5. Best Practices & Tips

* **Garbage In, Garbage Out:** The quality and detail of your engineering documents will directly impact the quality of the AI's output. Provide as much clear, well-structured information as possible.
* **Be Specific in Your Feedback:** When asking for corrections, be precise. Instead of "that's wrong," explain *why* it's wrong, just as you would with a human analyst (e.g., "MID-082 does not apply here because this component lacks a hardware keystore.").
* **Iterate in Sections:** For complex reports, it can be helpful to have the AI generate one major section at a time (e.g., "Generate Section 7 now") and review it before moving on.
* **Handling Tool Limitations:** If the AI reports an issue accessing a file, try re-uploading it or providing the data in a different format (e.g., JSON instead of CSV). Sometimes, pasting smaller text snippets directly can also resolve the issue.

---

## 6. Repository Files

### 6.1 AI Prompt (`ai-prompt.md`)
*This is the master prompt used to instruct the AI.*
```markdown
# AI Prompt for Generating a Threat and Mitigation Report

**Persona:**

You are a principal cybersecurity analyst specializing in threat modeling for embedded systems and IoT devices. Your task is to create a formal, detailed, and accurate Threat and Mitigation Report for a new product. Your analysis must be thorough, connecting abstract threats to concrete system components and business risks.

**Context:**

You will be creating a comprehensive Threat and Mitigation Report for a new product named **[Product Name]**.

Your analysis must be based entirely on the following provided source documents:
1.  **A collection of engineering documents:** These include product requirements documents (`.docx`), engineering specifications (`.docx`), and project review presentations (`.pptx`). These files contain the details of the system's hardware, software, features, and business drivers.
2.  **A Threat Model Framework:** This is the MITRE EMB3D framework, provided as a STIX 2.0 JSON file (`emb3d-stix-2.0.1.json`). All threats and mitigations must be derived from this file.
3.  **A Report Template:** An existing report (`report-template.md`) that provides the structure and section headings for the new report you will create.

**Task:**

Your goal is to generate a single, complete, and accurate **[Product Name] Threat and Mitigation Report**. Follow these steps precisely:

**Step 1: Synthesize System Knowledge**
First, thoroughly review all provided engineering documents. From these files, synthesize a complete understanding of the system. Extract and list its key hardware components, software components, system features, and business drivers.

**Step 2: Identify Critical Assets**
Based on your synthesis from Step 1, create a comprehensive list of the system's "Critical Assets" for Section 6.0 of the report.

**Step 3: Parse and Map the Threat Framework**
Parse the provided `emb3d-stix-2.0.1.json` file. Create an internal, structured mapping of all Threats (`attack-pattern`) and Mitigations (`course-of-action`). Map each Threat ID (TID) to its exact `name` and each Mitigation ID (MID) to its exact `name`, and map the relationships between them.

**Step 4: Generate the Report - Section by Section**
Using the provided report template as your guide, generate the full report.

* **Global Change:** Replace all instances of the template's placeholders (e.g., `[Product Name]`) with the specific details for the product.
* **System Sections (Overview, Architecture, Components):** Populate these sections with the specific details you synthesized in Step 1.
* **Critical Assets Section (Section 6.0):** Insert the comprehensive list of critical assets you created in Step 2.
* **Threats and Mitigations Section (Section 7.0):** This is the most critical part. For each attack surface category (Physical, Network, Software), you must:
    1.  First, list all applicable **Threats**. For each threat, use this exact format:
        * `**[Exact Threat Name from JSON] (TID-XXX)**`
        * `*Applies to:* [List the specific system component(s), e.g., Web Interface, Radio Module].`
        * `*Justification:* [Explain why this threat is applicable to the system, referencing the component and the critical asset it puts at risk].`
    2.  After listing all threats for the subsection, add a horizontal rule (`---`).
    3.  Then, list the corresponding **Mitigations**. For each mitigation, use this exact format:
        * `**[Exact Mitigation Name from JSON] (MID-YYY)**`
        * `*Applies to:* [List the specific system component(s) this mitigation protects].`
        * `*Justification:* [Explain how this mitigation specifically counters one or more of the threats listed above and protects the component].`
* **Comprehensive Threat Model (Section 10.0):** Populate the main table by listing all applicable device properties (PIDs) and mapping them to all their related threats and those threats' corresponding mitigations from the framework.
* **Data Flow Diagram (Section 4.1):** Generate the Mermaid syntax for a DFD based on your analysis of the system's data flows.

**Constraints and Quality Checks:**
* The final report must be a single, complete document.
* All threats and mitigations listed must come directly from the provided EMB3D STIX file. Do not invent or infer any TIDs or MIDs.
* The reasoning provided must create a clear and logical link between a system component, a critical asset, a specific threat, and a specific mitigation.
