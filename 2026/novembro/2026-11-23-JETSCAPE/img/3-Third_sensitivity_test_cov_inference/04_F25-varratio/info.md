# 04_F25-varratio

- Artigo: Mankolli et al., PRC 114, 014905 (2026), Fig. 25: Σ_emu/Σ_exp na diagonal (idêntica entre propostas)
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 04
- Tag de proposta no nome do arquivo: `covinf`
- Modelos comparados: baseline (diag), CS sector (referência), learned PHOBOS cov. (laranja `#FF7F00`, tracejado)
- Status: gerado
- Não depende da cadeia: fonte `mcmc/02_12_shift_vs_cov_ratio/bin_level.npz`.

## Origem
- Script: `3dglauber-bayes-example/mcmc/scripts/paper_figures/fig_varratio.py` (+ `common.py`, MODEL `learned_phobos`; proposta `covinf`)
- Dados:
  - baseline: `mcmc/01_01_baseline_reproduction/chain-idf-0.hdf`
  - CS sector: `mcmc/02_04_cs_sector/chain-idf-0-cs-observable-region-rho-1-a77902df73a5.hdf`
  - learned PHOBOS (job Condor 5079): `mcmc/02_14_learned_phobos_normalization/chain-idf-0-learned-phobos-7ec3735f96a8.hdf`
- burnin / thin / amostras: 20 000 de burn-in; 20 000 de produção / 50 / 80 000
- Comando (no servidor, de `mcmc/scripts/paper_figures`, python `/storage/avilamrs/code-space/my-work/jetscape-emulator/.venv/bin/python`):
  `python fig_varratio.py --proposal covinf --outdir ../../04_paper_figures/out/covinf/04_F25-varratio`
- Gerado em 25/11/2026 e copiado de `mcmc/04_paper_figures/out/covinf/04_F25-varratio/`
