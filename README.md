Adaptive Multi-Persona AI Assistant (AMPAA)

1. Project Overview

The Adaptive Multi-Persona AI Assistant (AMPAA) is a next-generation conversational system designed to serve the distinct needs of academic and professional users across three specialized domains: Lab Research & Reporting, Study Assistance, and Office Administration.
The core innovation is the Personalization Engine, which uses a Hybrid Feedback Loop (Explicit and Implicit) and a custom-trained ML Preference Classifier to allow each user to isolate and dynamically adapt the agent's behavior and style without affecting other users (multi-tenancy).

2. Technical Solution and Objectives

A. Core Architecture: Hybrid Adaptive Model
The system utilizes a single, powerful Large Language Model (LLM) (e.g., using Ollama on a dedicated server) that switches between three personas based on the user's initial prompt. The LLM's behavior is controlled by a dynamic System Prompt that is personalized for each user.

B. High-Value Project Components
| Project Requirement | Implementation Detail | CE Value Demonstrated |
|---|---|---|
| Code | Python-based application using a framework (e.g., LangChain/CrewAI) to orchestrate the feedback loop and prompt injection. | Software Engineering, Distributed Systems |
| ML Training | Training a small ML Preference Classifier on synthetic/logged user feedback to map free-form text corrections (implicit feedback) to structured behavior labels. | Machine Learning (Classification), Data Science |
| Database | PostgreSQL or similar, utilizing Row-Level Security (RLS) and a Personalization Table to achieve strict multi-tenancy and data persistence. | Database Management, Multi-Tenancy |
| Cloud Storage | Cloud platform (e.g., AWS S3) used for storing user-specific documents (lab reports, emails, notes), enforced via tenant-specific folder paths and encryption. | Cloud/Edge Services, Security & Privacy |
| RL/Optimization | The system operates as a form of Reinforcement Learning from Human Feedback (RLHF), where user corrections serve as the dynamic reward signals for adaptation. | Reinforcement Learning, Optimization |

3. Detailed Adaptive Mechanism

The project's core intelligence relies on the following loop:
 * Interaction: A user receives a response from the agent.
 * Hybrid Feedback Capture:
   * Explicit: The user clicks "Thumbs Down" or "Too Formal."
   * Implicit: The user provides a follow-up correction ("Can you make that a bulleted list?").
 * ML Classification: The user's text correction is fed to the trained ML Preference Classifier.
   * Example Input: "Make that a bulleted list."
   * Example Output (Structured Label): ADJUST_TO_BULLETS
 * Database Update: The structured label is saved to the user's isolated record in the Personalization Table.
 * Dynamic Prompt Injection: For the user's next request, the orchestrator code queries the database and injects the user's full list of preferences (e.g., "Always use bullet points; use a casual tone for study questions") into the LLM's Master System Prompt.

4. Multi-Tenancy and Security Architecture

To ensure each user's data and preferences remain isolated, we will implement the following security measures:
 * User Isolation: All personalization data, conversation logs, and file access must be strictly keyed by a unique user_id. Queries to the Database will utilize Row-Level Security (RLS) to enforce isolation at the application layer.
 * Encrypted Storage: All user-uploaded documents (e.g., lab data files) in Cloud Storage will be encrypted using unique, tenant-specific encryption keys (derived from a master key or a Cloud KMS).
 * LLM Privacy: Running the LLM locally on a dedicated server instance using Ollama ensures that the sensitive prompt/response data never leaves the system's control to a third-party API provider.

5. Required Tools and Technologies
| Category | Tools to be Used |
|---|---|
| Programming | Python, Python Libraries (e.g., requests, SQLAlchemy) |
| AI/ML Frameworks | Scikit-learn or TensorFlow/Keras (for the Classifier), Ollama Python Client, LangChain/CrewAI (for orchestration). |
| Database/Cloud | PostgreSQL (with RLS focus), AWS/GCP (for hosting the application and Cloud Storage). |
| LLM Execution | Ollama (to run Llama 3 or Gemma 3 locally on the server). |


