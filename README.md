# Generative AI Systems: RAG and Agents

Four generative AI systems built with large language models, covering retrieval augmented generation (RAG), multimodal retrieval, LLM evaluation, and agentic workflows. Each project is a self contained notebook with its outputs saved.

| Project | What it does | Key techniques |
|---|---|---|
| [NIH proposal generator](nih-proposal-generator/) | Drafts the technical approach section of an NIH grant proposal from a researcher's past papers and a funding announcement | RAG over 110 papers, relevance filtering, LLM self evaluation, human in the loop review |
| [LangGraph email agent](langgraph-email-agent/) | Triages an inbox, summarizes critical emails, and drafts replies | LangGraph state graph, conditional routing, review and polish step |
| [CRM RAG assistant](crm-rag-assistant/) | Answers sales team questions from Salesforce product documentation | LangChain, Chroma, LLM evaluation of groundedness, relevance, and faithfulness, Gradio interface |
| [Multimodal RAG](multimodal-rag/) | Answers nutrition questions with both text and images from a reference guide | OpenCLIP image and text embeddings, query expansion, cross-encoder reranking |

## NIH proposal generator

The final project for the course, and the most complete pipeline here.

1. Parses the funding announcement (PAR-25-136, Laboratories to Optimize Digital Health) and a folder of research papers into overlapping chunks, and indexes them with FAISS.
2. Scores every paper for topical relevance to the announcement and keeps only those above a threshold.
3. Generates research ideas that bridge the retained work and the announcement's priorities, then drafts the technical approach.
4. Runs an LLM self evaluation loop against NIH review criteria and revises the draft.
5. Pauses at a human review checkpoint, applies the reviewer's guidance, and writes the final draft to text and a formatted PDF within the five page limit.
6. Prints an audit trail of every relevance score, filtering decision, evaluation round, and approval.

The pipeline runs in two modes so the effect of human review can be compared. The outputs of both runs are included as [technical_approach_auto.pdf](nih-proposal-generator/technical_approach_auto.pdf) and [technical_approach_human.pdf](nih-proposal-generator/technical_approach_human.pdf).

## LangGraph email agent

A LangGraph rewrite of a prompt only email workflow. Each email moves through a shared state with nodes for classification, summarization of critical emails, first response drafting, and review and polishing. Conditional routing sends urgent and deadline driven emails through the full path and skips routine ones. The notebook ends with an evaluation of the agent's summaries and drafts, including where it inferred deadlines that were not in the original email.

## CRM RAG assistant

Splits Salesforce documentation into overlapping chunks, stores them in Chroma, and answers questions with GPT-4o models through retrieved context. An LLM judge scores each answer for groundedness, relevance to the question, and faithfulness to the sources, and a Gradio interface serves the assistant.

## Multimodal RAG

Indexes text and images from a vitamin and mineral guide with OpenCLIP embeddings in Chroma. Each question is expanded into alternative phrasings, candidates are reranked with a cross-encoder (`ms-marco-MiniLM-L-6-v2`), and the answer is returned with the most relevant image.

## Running the notebooks

Each notebook reads OpenAI credentials from a `config.json` in its own folder. Copy [config.example.json](config.example.json) there and add your key. Real `config.json` files are ignored by Git.

The input documents are not included. These are the course's sample emails, the Salesforce guides, the nutrition guide, and the research papers used for the NIH project. To run a notebook, place your own documents where its first cells expect them.

## Built with

Python, OpenAI API (GPT-4o, GPT-4o mini), LangChain, LangGraph, FAISS, Chroma, OpenCLIP, Sentence Transformers, Gradio, pdfplumber, ReportLab

## Context

Built for EN.705.608 Applied Generative AI at Johns Hopkins University (Spring 2026).
