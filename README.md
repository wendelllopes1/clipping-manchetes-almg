# 📰 Detector de Manchetes em PDF (Clipping ALMG)

Este repositório contém um app **Streamlit** que tenta identificar **manchetes** em arquivos PDF de clipping
usando **heurísticas de layout** (tamanho da fonte, negrito e posição na página) via **PyMuPDF**.

> **Ideal para PDFs digitais** (com texto embutido). Para **PDFs escaneados** (imagem), é preciso aplicar **OCR** antes.

## Como usar (sem instalar nada no seu computador)

1. Crie um repositório no GitHub (por exemplo: `clipping-manchetes`).
2. Envie os seguintes arquivos para o repositório:
   - `app.py`
   - `requirements.txt`
   - `packages.txt` (opcional; já incluído aqui, pode ficar vazio)
   - `README.md`
3. Acesse **Streamlit Community Cloud** (https://streamlit.io/cloud), clique em **Deploy an app** e conecte seu GitHub.
4. Escolha o repositório e o arquivo principal `app.py` e conclua o deploy.
5. Abra o app, faça **upload** do seu PDF e ajuste as **heurísticas** na barra lateral.

## Como funciona (resumo técnico)

- Lemos o PDF com **PyMuPDF** e extraímos os *spans* de texto (cada trecho com fonte/tamanho).
- Calculamos um **percentil global** de tamanho de fonte (ex.: 90º).
- Selecionamos candidatos que:
  - têm **tamanho de fonte** ≥ *cutoff* OU são **negrito** (quando ativado);
  - têm **comprimento** entre limites configuráveis (ex.: 6–120 caracteres);
  - têm **proporção de MAIÚSCULAS** mínima (ex.: ≥ 0,4).
- Ordenamos por **página**, **prioridade**, **posição vertical** (topo da página) e **tamanho**.
- Removemos duplicatas próximas.
- Exibimos em tabela e oferecemos **download em CSV**.

## Limitações e próximos passos

- **PDF escaneado** precisa de **OCR**:
  - Alternativas: Tesseract, Google Document AI, ABBYY FineReader, PaddleOCR.
  - Depois do OCR, reexporte como PDF com **texto embutido** e use o app.
- Os critérios são **heurísticos**. Em alguns arquivos você pode precisar:
  - reduzir o percentil de fonte,
  - diminuir o limite de maiúsculas,
  - desativar a exclusão de rodapés,
  - ajustar `min_len`/`max_len`.
- Evoluções possíveis:
  - Agrupar spans em **linhas/blocos** por X/Y para títulos multi-linha.
  - Modelos de **layout** (Layout Parser / DocTR) para títulos com mais precisão.
  - Regras específicas para **clippings da ALMG** (padrões de diagramação).

## Desenvolvimento local (opcional)

Se quiser rodar localmente:
```bash
pip install -r requirements.txt
streamlit run app.py
