# 06_S1-shift

- Artigo: Mankolli et al., PRC 114, 014905 (2026), figura nossa: deslocamento do posterior vs σ²_emu/σ²_exp
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 06
- Tag de proposta no nome do arquivo: `covinf`
- Modelos comparados: baseline (diag), CS sector (referência), learned PHOBOS cov. (laranja `#FF7F00`, tracejado)
- Status: gerado

## Origem
- Script: `3dglauber-bayes-example/mcmc/scripts/paper_figures/fig_shift.py` (+ `common.py`, MODEL `learned_phobos`; proposta `covinf`)
- Dados:
  - baseline: `mcmc/01_01_baseline_reproduction/chain-idf-0.hdf`
  - CS sector: `mcmc/02_04_cs_sector/chain-idf-0-cs-observable-region-rho-1-a77902df73a5.hdf`
  - learned PHOBOS (job Condor 5079): `mcmc/02_14_learned_phobos_normalization/chain-idf-0-learned-phobos-7ec3735f96a8.hdf`
- burnin / thin / amostras: 20 000 de burn-in; 20 000 de produção / 50 / 80 000
- Comando (no servidor, de `mcmc/scripts/paper_figures`, python `/storage/avilamrs/code-space/my-work/jetscape-emulator/.venv/bin/python`):
  `python fig_shift.py --proposal covinf --outdir ../../04_paper_figures/out/covinf/06_S1-shift`
- Gerado em 25/11/2026 e copiado de `mcmc/04_paper_figures/out/covinf/06_S1-shift/`
