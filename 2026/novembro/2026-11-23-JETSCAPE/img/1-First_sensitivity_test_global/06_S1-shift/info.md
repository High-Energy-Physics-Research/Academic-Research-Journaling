# 06_S1-shift

- Artigo: Mankolli et al., PRC 114, 014905 (2026), figura nossa: deslocamento do posterior vs σ²_emu/σ²_exp
- Especificação: `../../DESIGN_SYSTEM.md`, seção 2 · 06
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
- Substitui a versão anterior (painel RBF "chain not available yet"); backup em `mcmc/04_paper_figures/backup_before_5078/06_S1-shift/`.
