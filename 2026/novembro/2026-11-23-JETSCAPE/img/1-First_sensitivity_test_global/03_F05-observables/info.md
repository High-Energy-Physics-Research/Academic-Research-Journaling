# 03_F05-observables

- Artigo: Mankolli et al., PRC 114, 014905 (2026), Fig. 5: posterior dos observáveis, todos os bins
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 03
- Tag de proposta no nome do arquivo: `global`
- Modelos comparados: baseline (diag), CS global (ρ=1), RBF global (ℓ=0.5)
- Status: pendente (passo 3)

## Origem (preencher ao gerar)
- Script (3dglauber-bayes-example/...):
- Dados (3dglauber-bayes-example/mcmc/...):
- burnin / thin / amostras:
- Comando:

## RBF global: job 5078 (gerado em 25/11/2026)
- Cadeia: `mcmc/02_02_global_rbf/chain-idf-0-rbf-global-c33aba121315.hdf` (200 walkers, burn-in 20 000 + produção 20 000; 80 000 amostras, thin 50)
- Script de execução: `mcmc/04_paper_figures/run_5078_5079.sh` (no servidor, venv `/storage/avilamrs/code-space/my-work/jetscape-emulator/.venv`); saída em `mcmc/04_paper_figures/out/global_rbf/` e copiada para cá.
- Limites de y **reaproveitados** de `F05-observables__global__baseline-vs-cs__p01.json` (`--ylims-from`), para a página ficar idêntica em escala às já existentes. A banda do RBF passa desses limites em 3 painéis (d-Au dN/dη 0-5%, 5-10%, 10-20%), listados no manifesto em `panels_where_model_band_exceeds_ylims`.
