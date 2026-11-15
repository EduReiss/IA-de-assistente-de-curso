# Assistant for the Technical Course in Artificial Intelligence (IFNMG/CEADI)

A chat agent for students that answers questions about the course based on the Course Pedagogical Project (PPC, in Portuguese). Implemented in a Google Colab notebook, using the Gemini model with tools.

[Leia em português](README.md)

## Features

- **Input guardrail:** blocks messages with prompt injection attempts before they reach the agent. Today it uses regular expression rules. A model-based layer (`guardion/ModernGuard-1`) is planned but not implemented.
- **Skills:** instructions stored in `SKILL.md` files. The agent picks a Skill by its description and follows its steps:
  - `perguntas_simples`: duration, format, workload and admission, based on a course summary.
  - `duvidas_ppc`: questions about the PPC rules, answered by searching the document.
  - `quadro_de_disciplinas`: subjects and workload for a given module, shown as a table.
- **Agentic RAG:** the agent decides when to search the PPC and can rewrite the query. Search is semantic, using `sentence-transformers` over chunks of the PPC.
- **Short-term memory:** the conversation history is kept in the `chat` object.
- **Long-term memory:** the student's name and module are saved in `memoria_longa.json` and reused in new conversations.

## How to run

1. Open `TP_Assistente_organizado.ipynb` in Google Colab.
2. Upload the PPC PDF to Colab with the name `ppc.pdf`, using the files panel.
3. Create an API key in [Google AI Studio](https://aistudio.google.com) and store it under **Secrets** (key icon in the sidebar), with the name `GOOGLE_API_KEY`.
4. Check that the model set in `MODELO` is available for your key. To list the models:
   ```python
   for m in client.models.list():
       print(m.name)
   ```
5. Click **Runtime > Run all**.

The demo runs conversation 1 (messages 1 to 5, including a blocked injection attempt) and conversation 2 (a question about subjects, using long-term memory).

## Structure

- `TP_Assistente_organizado.ipynb`: the full code, in execution order.
- `skills/`: created by the notebook, containing the Skills as `SKILL.md` and the course summary.
- `memoria_longa.json`: created on run, storing student profiles.

## Technologies

- Python
- Google Gemini (`google-genai`)
- `sentence-transformers` with the `paraphrase-multilingual-MiniLM-L12-v2` model
- `pypdf` and `numpy`
- Google Colab

## Limitations

This is a study project. The main limitations are:

- Long-term memory has no authentication and identifies students only by name.
- The rule-based guardrail can miss variations of prompt injection.
- Search depends on splitting the PPC into fixed-size character chunks, which ignore its sections, and on well-formed queries.
- There is no formal evaluation of answer accuracy.
- The model service may return errors during periods of high demand.

## Disclaimer

This is a study project and is not an official IFNMG system. Answers should be checked against the PPC.
