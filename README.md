# Assistente do Curso Técnico em Inteligência Artificial (IFNMG/CEADI)

Agente de conversa para alunos que responde dúvidas sobre o curso com base no Projeto Pedagógico do Curso (PPC). Implementado em um notebook do Google Colab, usando o modelo Gemini com ferramentas.

[Read in English](README.en.md)

## Funcionalidades

- **Guardrail de entrada:** bloqueia mensagens com tentativas de injeção de prompt antes que cheguem ao agente. Hoje usa regras com expressões regulares. A camada por modelo (`guardion/ModernGuard-1`) está prevista, mas não implementada.
- **Skills:** instruções em arquivos `SKILL.md`. O agente escolhe a Skill pela descrição e segue os passos dela:
  - `perguntas_simples`: duração, modalidade, carga horária e ingresso, a partir de um resumo do curso.
  - `duvidas_ppc`: dúvidas sobre regras do PPC, com busca no documento.
  - `quadro_de_disciplinas`: disciplinas e cargas horárias de um módulo, em tabela.
- **RAG agêntico:** o agente decide quando buscar no PPC e pode reformular a consulta. A busca é semântica, com `sentence-transformers`, sobre trechos do PPC.
- **Memória de curto prazo:** o histórico da conversa fica no objeto `chat`.
- **Memória de longo prazo:** nome e módulo do aluno são salvos em `memoria_longa.json` e usados em conversas novas.

## Como executar

1. Abra o notebook `TP_Assistente_organizado.ipynb` no Google Colab.
2. Envie o PDF do PPC para o Colab com o nome `ppc.pdf`, pelo painel de arquivos.
3. Crie uma chave de API no [Google AI Studio](https://aistudio.google.com) e guarde-a em **Segredos** (ícone de chave na barra lateral), com o nome `GOOGLE_API_KEY`.
4. Confira se o modelo definido em `MODELO` está disponível na sua chave. Para listar os modelos:
   ```python
   for m in client.models.list():
       print(m.name)
   ```
5. Clique em **Ambiente de execução > Executar tudo**.

A demonstração roda a conversa 1 (mensagens 1 a 5, incluindo uma tentativa de injeção bloqueada) e a conversa 2 (pergunta sobre disciplinas, usando a memória de longo prazo).

## Estrutura

- `TP_Assistente_organizado.ipynb`: código completo, na ordem de execução.
- `skills/`: criada pelo notebook, com as Skills em `SKILL.md` e o resumo do curso.
- `memoria_longa.json`: criado ao executar, com os perfis dos alunos.

## Tecnologias

- Python
- Google Gemini (`google-genai`)
- `sentence-transformers` com o modelo `paraphrase-multilingual-MiniLM-L12-v2`
- `pypdf` e `numpy`
- Google Colab

## Limitações

O projeto é um estudo. Entre as limitações principais:

- A memória de longo prazo não tem autenticação e identifica o aluno só pelo nome.
- O guardrail por regras pode deixar passar variações de injeção de prompt.
- A busca depende da divisão do PPC em blocos de caracteres, que não respeita as seções, e de consultas bem formuladas.
- Não há avaliação formal da taxa de acerto das respostas.
- O serviço do modelo pode responder com erro em horários de alta demanda.

## Aviso

Este é um projeto de estudo e não é um sistema oficial do IFNMG. As respostas devem ser conferidas no PPC.
