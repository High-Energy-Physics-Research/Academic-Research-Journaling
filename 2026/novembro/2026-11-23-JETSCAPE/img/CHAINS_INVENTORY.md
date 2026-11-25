# Inventário de cadeias: 3dglauber-bayes-example (24/11/2026)

Raiz: `~/Remote-HPC/code-space/my-work/jetscape-emulator/3dglauber-bayes-example` (HPC: `/storage/avilamrs/code-space/my-work/...`).
Lido diretamente dos HDF (formato + atributos) e dos manifestos JSON. Todas usam os emuladores
`emulator/emulator-{Au-Au,d-Au}-200-idf-0-npc-10.dill` e os 917 bins (791 Au-Au + 126 d-Au).

## Cadeias existentes

| Experimento (`mcmc/…`) | Modelo de cov. experimental | walkers × produção | burn-in | Parâm. | Uso |
|---|---|---|---|---|---|
| `01_01_baseline_reproduction/chain-idf-0.hdf` | diagonal (baseline) | 200 × 50 000 | **não gravado** (sem dataset `burnin` nem atributos) | 20 | **baseline de todas as propostas** |
| `02_01_global_cs/chain-idf-0-cs-system-rho-1.hdf` | CS ρ=1, escopo `system` (todos os bins de cada sistema correlacionados; sem correlação Au-Au↔d-Au) | 200 × 50 000 | 20 000 | 20 | proposta 1: CS global |
| `02_04_cs_sector/chain-idf-0-cs-observable-region-rho-1-a77902df73a5.hdf` | CS ρ=1, escopo observável + região de rapidez | 200 × 50 000 | 20 000 | 20 | proposta 2: CS sector |
| `02_05_rbf_sector/chain-idf-0-rbf-observable-a873ae558cc9.hdf` | RBF em coordenadas físicas (pT 0.5 GeV/c, η 1.0, centralidade 10 p.p.), escopo setor | 200 × 50 000 | 20 000 | 20 | proposta 2: RBF sector |
| `02_10_nongaussian_nuisance/chain-idf-0-nongauss-nuisance-6058bef13723.hdf` | diagonal + 3 nuisances não gaussianas (STAR v2(η) 20-70%) | 200 × **20 000** | 20 000 | 23 | proposta 3a: global nuisance |
| `02_11_nongaussian_nuisance_detector_split/chain-idf-0-nongauss-nuisance-detector-split.hdf` | diagonal + 4 nuisances (TPC/FTPC separados) | 200 × **20 000** | 20 000 | 24 | proposta 3b: detector split |
| `01_00_reference_posterior/posterior-chain-default.hdf` | posterior de referência do artigo | 200 × 20 000 | — | 20 | só validação da baseline |
| `01_04_mcmc_convergence/chain-idf-0.hdf` | teste de convergência | 200 × 1 (+20 000 burn-in) | 20 000 | 20 | só a Fig. 24 (fora do escopo atual) |

Outros insumos já prontos:
- `02_12_shift_vs_cov_ratio/bin_level.npz`: σ²_emu/σ²_exp e pesos de Fisher por bin → **F25 e S1**.
- `02_08_sector_input_correlations/*_input_matrices.npz`: correlação experimental CS/RBF sector → **F26 (proposta 2)**.
- `02_09_all_bins_input_correlations`: ilustração CS/RBF com índice global (ℓ=0.5); **não** corresponde a nenhuma cadeia → não usar.
- `covariancia_emulador.py`: covariância do emulador num ponto da cadeia → **F26 (coluna do emulador)**.
- `*/plots/observables`, `02_06/baseline_predictions`: preditivas com 1000 amostras → **F05** (o design pede 10 000; regenerar).

## Situação por proposta × grupo

✔ = dados prontos · ⟳ = dados existem, falta gerar no novo padrão · ✘ = falta rodar

| Proposta | 01 F28 | 02 F14 | 03 F05 | 04 F25 | 05 F26 | 06 S1 |
|---|---|---|---|---|---|---|
| 1 global (baseline, CS global, **RBF global**) | CS ✔ / RBF ⏳ job 5078 | CS ✔ / RBF ⏳ | CS ✔ / RBF ⏳ | ✔ | CS ✔ / RBF ⏳ | CS ✔ / RBF ⏳ |
| 2 sector (baseline, CS sector, RBF sector) | ⟳ | ⟳ | ⟳ | ⟳ | ⟳ | ⟳ |
| 3 cov momentum (a nuisance, b split, c quantum) | a,b ⟳ / c ✘ | a,b ⟳ / c ✘ | a,b ⟳ / c ✘ | ⟳ | a definir | a,b ⟳ / c ✘ |
| 4 GP heuristics | ✘ | ✘ | ✘ | ⟳ | ✘ | ✘ |

## Rodando (25/11/2026)

| Job | Experimento | Modelo | Configuração |
|---|---|---|---|
| 5078 (qgp1) | `02_02_global_rbf/chain-idf-0-rbf-global-c33aba121315.hdf` | RBF global, comprimentos comuns (pT 0.5, η 1, centralidade 10) | 200 × 20 000 + 20 000 burn-in |
| 5079 (qgp3) | `02_14_learned_phobos_normalization/chain-idf-0-learned-phobos-7ec3735f96a8.hdf` | **degrau 3**: covariância do PHOBOS dN/dη aprendida da fatia 2a (offset + inclinação + diagonal; diagonal da calibração mantida); resto diagonal | 200 × 20 000 + 20 000 burn-in |

## O que precisa rodar

1. **RBF global (proposta 1)**: `mcmc/02_02_global_rbf/` está vazia. `bayes_mcmc_rbf_model.py` é o RBF original por
   observável, não global, e nunca foi rodado. É preciso implementar uma variante com o mesmo escopo do CS global
   (`system`) e rodar com a configuração das outras cadeias:
   `--nwalkers 200 --nburnsteps 20000 --ncpus 20 --status 1000 50000`.
   Falta decidir o kernel (ver abaixo).
2. **Proposta 3c (quantum source inference)**: nenhuma cadeia.
3. **Proposta 4 (GP heuristics)**: nenhuma cadeia; modelos a definir.

## Problemas de consistência encontrados

- **O S1 atual mistura frameworks**: em `mcmc/scripts/shift_vs_cov_ratio.py`, os deslocamentos de CS/RBF *global*
  vêm do `hic-bayes` (`run_004`, só Au-Au, **175 passos**), e os de *sector* vêm do 3dglauber. Pela regra nova, as
  duas partes devem vir das cadeias do 3dglauber.
- **"CS global" não é o mesmo nos dois frameworks**: no 3dglauber (`02_01`) o escopo é por sistema (Au-Au e d-Au,
  cada um fechado); no `hic-bayes` (`run_005`) era só Au-Au. As figuras novas usam a do 3dglauber.
- **Baseline sem burn-in registrado**: `01_01` só guarda a produção. Os READMEs usam `--nburnsteps 20000`;
  confirmar no log do HPC.
- **Proposta 3 tem 20 000 passos de produção, contra 50 000** nas demais: para a mesma figura, igualar o número de
  amostras retidas (80 000) por thinning.
- **Assinatura da covariância não confere** em `02_04`/`02_05` (`covariance_signature_matches = False`): a
  covariância reconstruída localmente não é bit a bit a usada na cadeia. Não impede as figuras de posterior, mas
  afeta a F26 (proposta 2).
