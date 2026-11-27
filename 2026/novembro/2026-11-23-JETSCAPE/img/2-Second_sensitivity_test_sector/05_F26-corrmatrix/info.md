# 05_F26-corrmatrix

- Artigo: Mankolli et al., PRC 114, 014905 (2026), Fig. 26: matrizes de correlação: emulador vs experimental de cada modelo
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 05
- Tag de proposta no nome do arquivo: `sector`
- Modelos comparados: baseline (diag), CS sector, RBF sector
- Status: pendente (passo 3)

## Origem (preencher ao gerar)
- Script (3dglauber-bayes-example/...):
- Dados (3dglauber-bayes-example/mcmc/...):
- burnin / thin / amostras:
- Comando:

## Atualização de 27/11/2026
- Eixos crescentes (`origin="lower"`, DESIGN_SYSTEM 1.5): figura de 3 observáveis regenerada.
- Nova versão com **todos os 40 observáveis**, uma linha por observável (ordem da F05), 5 por página:
  `F26-corrmatrix__<proposta>__baseline-cs-rbf__p01` … `__p08`. Script `fig_corrmatrix_all.py`.
