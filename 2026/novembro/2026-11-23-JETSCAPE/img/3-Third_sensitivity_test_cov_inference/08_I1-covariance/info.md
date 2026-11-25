# 08_I1-covariance

- Item de informação (não é figura do artigo): artigo que explica a inferência da covariância experimental da proposta
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 08
- Tag de proposta no nome do arquivo: `covinf`
- **Documento: `I1-covariance__covinf.pdf`** (fonte `I1-covariance__covinf.tex` + `tab/` + `fig/`; compilar com `latexmk -lualatex`)
- Resultado numérico completo: `I1-covariance__covinf__learnphob__matrix_cov.csv` (594×594, log) e `__matrix.npz` (cov, corr, sigma, labels)
- Números citados: `I1-covariance__covinf__learnphob.json`; ilustração: `I1-covariance__covinf__learnphob.{pdf,png}`
- Status: gerado (25/11/2026)

## Origem
- Fonte canônica: `3dglauber-bayes-example/mcmc/scripts/paper_figures/recipes/I1-covariance__covinf/`
- Passos (de `mcmc/scripts/paper_figures`):
  1. no servidor: `python info_covariance.py --proposal covinf --outdir ../../04_paper_figures/out/covinf/08_I1-covariance`
  2. local: `python3 info_covariance_tex.py --manifest ../../04_paper_figures/out/covinf/08_I1-covariance/I1-covariance__covinf__learnphob.json --recipe recipes/I1-covariance__covinf`
  3. `cd recipes/I1-covariance__covinf && latexmk -lualatex I1-covariance__covinf.tex`; copiar `.tex`, `.pdf`, `tab/`, `fig/` e os arquivos da matriz para esta pasta
