# marginals — origem dos gráficos

Run: `run_005_long_global` (Au-Au 200 GeV; 200 walkers, 20k burn-in + 20k produção/walker, thin=10).
Modelos: Baseline (diag) · CS global (ρ=1) · RBF global (ℓ=0.5).

Todos os arquivos desta pasta são idênticos (byte a byte) às cópias em `results/` listadas abaixo.

## Caminhos (local `~/Remote-HPC` ↔ HPC `/storage/avilamrs`)

| | Local | HPC |
|---|---|---|
| Repositório | `~/Remote-HPC/code-space/my-work/hic-bayes` | `/storage/avilamrs/code-space/my-work/hic-bayes` |
| Scripts | `<repo>/studies/AllDetectorsAuAu200/uncertainty_correlation/scripts/` | idem |
| Cadeias (dados) | `~/Remote-HPC/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/chains/run_005_long_global/` | `/storage/avilamrs/results/hic-bayes/AllDetectorsAuAu200/uncertainty_correlation/chains/run_005_long_global/` |
| Saídas originais | `.../uncertainty_correlation/analysis/plots/run_005_long_global/presentation_long_global/` e `.../shift_vs_cov_ratio/` | idem |

Cadeias lidas (`chain.csv`, 400 000 amostras cada) e configs em `chains/run_005_long_global/configs/`:
- `AuAu200_baseline_long_Grad/chain.csv`
- `AuAu200_cs_global_long_Grad/chain.csv`
- `AuAu200_rbf_global_long_Grad/chain.csv`

Dependência externa usada pelos scripts (rótulos/estilo do corner):
`~/Remote-HPC/code-space/my-work/jetscape-emulator/3dglauber-bayes-example/mcmc/scripts/plot_posterior_corner.py`

## Script → arquivos → comando

Executar a partir da raiz do repositório `hic-bayes`. Seja `S=studies/AllDetectorsAuAu200/uncertainty_correlation/scripts`
e `P=".../2026-11-23-JETSCAPE/img/1-First_sensitivity_test_global"`.

> Os comandos foram reconstruídos a partir dos `argparse` dos scripts; nenhum log guardou a linha exata.
> O `manifest.json` confirma que `plot_auau_long_global_marginals.py` foi rodado com `--presentation-dir` apontando para
> `1-First_sensitivity_test_global/` (os arquivos foram depois movidos para `marginals/`).

1. **`$S/plot_auau_long_global_marginals.py`**
   - `long_global_cs_posterior_marginals_comparison.{pdf,png}`
   - `long_global_rbf_posterior_marginals_comparison.{pdf,png}`
   - `long_global_cs_vs_baseline_posterior_corner.{pdf,png}`
   - `long_global_rbf_vs_baseline_posterior_corner.{pdf,png}`
   - `long_global_cs_rbf_median_shifts_baseline_sigma.csv`
   - `long_global_convergence_split_rhat.csv`
   - (+ `manifest.json` em `presentation_long_global/`)
   ```bash
   python $S/plot_auau_long_global_marginals.py --presentation-dir "$P"
   ```

2. **`$S/plot_long_global_all_marginals.py`**
   - `long_global_baseline_cs_rbf_posterior_marginals.{pdf,png}`
   ```bash
   python $S/plot_long_global_all_marginals.py --presentation-dir "$P"
   ```

3. **`$S/plot_long_global_corner3.py`**
   - `long_global_baseline_cs_rbf_posterior_corner.{pdf,png}`
   ```bash
   python $S/plot_long_global_corner3.py --presentation-dir "$P"   # defaults: --bins 40 --smooth 1.2
   ```

4. **`$S/plot_long_global_shift_vs_cov_ratio.py`**
   - `long_global_shift_vs_cov_ratio.{csv,pdf,png}` (+ `summary.json` em `shift_vs_cov_ratio/`)
   - Dado extra: `~/Remote-HPC/code-space/my-work/jetscape-emulator/3dglauber-bayes-example/mcmc/02_12_shift_vs_cov_ratio/bin_level.npz`
   ```bash
   python $S/plot_long_global_shift_vs_cov_ratio.py --presentation-dir "$P"
   ```

## Como as cadeias foram geradas

`<repo>/studies/AllDetectorsAuAu200/uncertainty_correlation/submit_long_global_chains.sh`
(→ `scripts/condor_long_global_chains.sub`), no nó de submissão do HPC:
```bash
./submit_long_global_chains.sh --run-id run_005_long_global
```
Detalhes: `study.md`, seção "Long Au-Au chains: baseline, global CS and genuinely global RBF".
