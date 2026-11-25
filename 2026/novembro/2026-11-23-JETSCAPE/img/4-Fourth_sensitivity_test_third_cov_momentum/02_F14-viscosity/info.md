# 02_F14-viscosity

- Artigo: Mankolli et al., PRC 114, 014905 (2026), Fig. 14: η/s(T) e ζ/s(T)
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 02
- Tag de proposta no nome do arquivo: `covmom`
- Modelos comparados: baseline (diag), CS sector, nuisance skew global (02_10), nuisance skew TPC/FTPC (02_11)
- Status: gerado

## Origem
- Script: `3dglauber-bayes-example/mcmc/scripts/paper_figures/fig_viscosity.py` (+ `common.py`, MODELS `nuis_global`, `nuis_split`; proposta `covmom`)
- Dados:
  - baseline: `mcmc/01_01_baseline_reproduction/chain-idf-0.hdf`
  - CS sector: `mcmc/02_04_cs_sector/chain-idf-0-cs-observable-region-rho-1-a77902df73a5.hdf`
  - nuisance skew (norm. global, 23 parâm.): `mcmc/02_10_nongaussian_nuisance/chain-idf-0-nongauss-nuisance-6058bef13723.hdf`
  - nuisance skew (TPC/FTPC, 24 parâm.): `mcmc/02_11_nongaussian_nuisance_detector_split/chain-idf-0-nongauss-nuisance-detector-split.hdf`
  - fontes (v̂, s, skew) do Tensor PCA das réplicas sintéticas: `study_nongaussian_systematics/data/{,detector_split/}inferred_sources.npz`
- burnin / thin / amostras: 20 000 de produção (as cadeias 02_10/02_11 só têm 20 000) / 50 / 80 000; só as 20 primeiras colunas (θ) para as bandas
- Comando (no servidor, de `mcmc/scripts/paper_figures`, python `/storage/avilamrs/code-space/my-work/jetscape-emulator/.venv/bin/python`):
  `python fig_viscosity.py --proposal covmom --outdir ../../04_paper_figures/out/covmom/02_F14-viscosity`
- Gerado em 25/11/2026 e copiado de `mcmc/04_paper_figures/out/covmom/02_F14-viscosity/`

> **Atenção:** os skews das nuisances vêm de um toy model (fontes postuladas → réplicas sintéticas → Tensor PCA), não de dados experimentais. Ver `../INFERENCE.md`.
