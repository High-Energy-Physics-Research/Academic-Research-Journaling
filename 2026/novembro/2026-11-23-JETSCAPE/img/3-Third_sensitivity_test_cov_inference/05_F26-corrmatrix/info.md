# 05_F26-corrmatrix

- Artigo: Mankolli et al., PRC 114, 014905 (2026), Fig. 26: matrizes de correlação, emulador vs experimental
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 05
- Tag de proposta no nome do arquivo: `covinf`
- Status: **não se aplica**. As 3 linhas da Fig. 26 (d-Au dN/dη 0-5% PHENIX; Au-Au v2(η) 3-15% PHOBOS; Au-Au v2(pT) 20-30% PHENIX)
  não contêm o bloco alterado (Au-Au PHOBOS dN/dη): para o modelo aprendido elas são idênticas à baseline (identidade).
  A matriz aprendida está em `../07_S2-expcov`.

## Atualização de 27/11/2026
- A versão de 3 observáveis continua **não se aplica** (motivo acima).
- Gerada a versão com **todos os observáveis do bloco alterado**: as 11 classes de Au-Au PHOBOS dN/dη, uma linha por
  classe, colunas emulador | exp. baseline (diag) | exp. CS sector | exp. learned PHOBOS, 5 por página:
  `F26-corrmatrix__covinf__baseline-cs-learnphob__p01` … `__p03`. Script `fig_corrmatrix_all.py`.
  Eixos crescentes (`origin="lower"`, DESIGN_SYSTEM 1.5).
