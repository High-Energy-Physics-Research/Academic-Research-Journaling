# 07_S2-expcov

- Figura nossa (sem equivalente no artigo): covariância (linha de cima, log) e correlação (linha de baixo, [0,1])
  experimentais entre todos os 917 bins; colunas baseline | CS | RBF.
- Especificação: `../../DESIGN_SYSTEM.md`, seção 07.
- Script: `3dglauber-bayes-example/mcmc/scripts/paper_figures/fig_expcorr.py`
- Dados: `mcmc/02_08_sector_input_correlations/{bin_index.csv,*_input_matrices.npz}`, `global_rbf_acceptance.json`
- Comando (no servidor, `ssh qgp`, em `mcmc/scripts/paper_figures`):
  `/storage/avilamrs/code-space/my-work/jetscape-emulator/.venv/bin/python fig_expcorr.py --proposal <global|sector> --outdir ../../04_paper_figures/out/<proposta>/07_S2-expcov`
  e depois copiado para esta pasta.
