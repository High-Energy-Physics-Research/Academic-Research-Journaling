# posterior_predictive — origem dos gráficos

Run: `run_005_long_global` (Au-Au 200 GeV). Bandas preditivas posteriores (painel superior: dados + bandas;
inferior: modelo/dados) para Baseline (diag), CS global (ρ=1) e RBF global (ℓ=0.5).

## Script

`~/Remote-HPC/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/analysis/scripts/plot_selected_observables_posterior_predictive_long_global.py`
(HPC: `/storage/avilamrs/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/analysis/scripts/...`)

⚠️ Este script está na pasta `results/`, **não** no repositório `hic-bayes` (não está versionado).
Ele importa `_autodiscover.py`, da mesma pasta, e módulos do repositório `hic-bayes`
(`pipelines.run_universality_analysis`, `hic_bayes.emulator_jet`).

## Dados lidos

- Cadeias: `~/Remote-HPC/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/chains/run_005_long_global/`
  - `AuAu200_baseline_long_Grad/` → rótulo interno `baseline`
  - `AuAu200_cs_global_long_Grad/` → rótulo interno `rho_0.9` (legenda: CS global ρ=1)
  - `AuAu200_rbf_global_long_Grad/` → rótulo interno `ro_10.0` (legenda: RBF global ℓ=0.5)
  - usa `chain.hdf5` se o `h5py` estiver instalado, senão `chain.csv`
- Emulador: `~/Remote-HPC/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/emulador/emulator-Au-Au-200-idf-0-npc-10.dill`
- Dados experimentais: `~/Remote-HPC/code-space/my-work/hic-bayes/data/AllDetectorsAuAu200/Au-Au-200/{experiment.csv,exp_error.csv}`
- Simulação (PCA): `~/Remote-HPC/code-space/my-work/hic-bayes/model/AllDetectorsAuAu200/Au-Au-200/simulation.csv`

## Arquivos → comando

Os comandos abaixo foram reconstruídos a partir do `argparse`; não há log do comando exato.
As saídas não estão em `results/.../plots/run_005_long_global/`, então foi usado `--output` apontando para esta pasta (ou as figuras foram copiadas depois).
Seja `PP=~/Remote-HPC/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/analysis/scripts/plot_selected_observables_posterior_predictive_long_global.py`
e `OUT=".../1-First_sensitivity_test_global/posterior_predictive"`.

1. `posterior_predictive_selected_9panels_legends_all_axes.{pdf,png}` (23/11, 11:44)
   ```bash
   python $PP --run run_005_long_global --system AuAu200 --observable-set selected --output "$OUT"
   ```
2. `posterior_predictive_AuAu200_all_page01..04.{pdf,png}` (23/11, 11:47)
   ```bash
   python $PP --run run_005_long_global --system AuAu200 --observable-set all --output "$OUT"
   ```
   Valores-padrão: `--burnin 100 --thin 10 --n-predictive-samples 250 --panels-per-page 9 --ncols 3 --seed 12345`.
3. `posterior_predictive_AuAu200_all_observables.pdf`: junção das 4 páginas com **pypdf** (metadado `Producer: pypdf`).
   **Nenhum script salvo faz essa junção.** Equivalente:
   ```bash
   python -c "from pypdf import PdfWriter; w=PdfWriter(); [w.append(f'posterior_predictive_AuAu200_all_page{i:02d}.pdf') for i in range(1,5)]; w.write('posterior_predictive_AuAu200_all_observables.pdf')"
   ```

Como as cadeias foram geradas: ver `../marginals/info.md` (`submit_long_global_chains.sh --run-id run_005_long_global`).
