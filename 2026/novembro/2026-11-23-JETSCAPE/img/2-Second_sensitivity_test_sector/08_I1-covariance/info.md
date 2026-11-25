# 08_I1-covariance

- Item de informação (não é figura do artigo): artigo que explica a covariância experimental da proposta (regra, parâmetros, matriz)
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 08
- Tag de proposta no nome do arquivo: `sector`
- **Documento: `I1-covariance__sector.pdf`** (fonte `I1-covariance__sector.tex` + `tab/` + `fig/`; compilar com `latexmk -lualatex`)
- Matrizes completas (917×917, espaço da verossimilhança): `I1-covariance__sector__cs-rbf__{cs,rbf}__matrix_cov.csv` e `__matrix.npz` (cov, corr, sigma, labels)
- Números citados: `I1-covariance__sector__cs-rbf.json`; ilustração: `I1-covariance__sector__cs-rbf.{pdf,png}`
- Status: gerado (25/11/2026)

## Origem
- Fonte canônica: `3dglauber-bayes-example/mcmc/scripts/paper_figures/recipes/I1-covariance__sector/`
- Passos (de `mcmc/scripts/paper_figures`):
  1. no servidor: `python info_covariance_assumed.py --proposal sector --outdir ../../04_paper_figures/out/sector/08_I1-covariance`
  2. local: `python3 info_covariance_assumed_tex.py --manifest ../../04_paper_figures/out/sector/08_I1-covariance/I1-covariance__sector__cs-rbf.json --recipe recipes/I1-covariance__sector`
  3. `cd recipes/I1-covariance__sector && latexmk -lualatex I1-covariance__sector.tex`; copiar `.tex`, `.pdf`, `tab/`, `fig/`, o manifesto e as matrizes para esta pasta
