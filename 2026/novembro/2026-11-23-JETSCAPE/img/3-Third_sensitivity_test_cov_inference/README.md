# Proposta 3: inferência da covariância experimental a partir de dados

Primeira cadeia em que a estrutura de correlação experimental é **aprendida de dados reais**, e não suposta
(propostas 1 e 2) nem postulada num toy model (proposta 4, `4-Fourth_sensitivity_test_third_cov_momentum`).

- **Job Condor 5079** (qgp3, 25/11/2026, término normal): `mcmc/02_14_learned_phobos_normalization/chain-idf-0-learned-phobos-7ec3735f96a8.hdf`,
  200 walkers × 20 000 de produção + 20 000 de burn-in, 20 parâmetros.
- **Fonte:** `study_nongaussian_systematics/fatia2a` (comparações entre PHOBOS, PHENIX, sPHENIX e BRAHMS em η≈0); é o
  "degrau 3" descrito em `../4-Fourth_sensitivity_test_third_cov_momentum/INFERENCE.md`.
- **Bloco alterado:** só Au-Au PHOBOS dN/dη (11 classes × 54 η = 594 bins); o resto continua diagonal (= baseline).
  Diagonal inalterada: só a correlação é aprendida.
- **Forma (em log):** Σ = a_c a_c' (offset comum, ρ=1, plano em η) + s² g_c g_c' (inclinação em centralidade) + diag(resto),
  com u = 0.0148 (parte não correlacionada, three-cornered hat) e s = 0.025 por 100% de centralidade.
  Correlação fora da diagonal no bloco: média 0.51, mínimo 0.07 (η frontal), máximo 0.94.
- **Hipóteses declaradas:** offset plano em η (fatia 2b não feita); variância extra no η frontal não correlacionada;
  variância da calibração mantida (possível 90% C.L. usado como 1σ: ponto em aberto).
- Detalhes: `mcmc/02_14_learned_phobos_normalization/README.md`.

## Figuras (tag `covinf`, cor laranja `#FF7F00`)
Comparação: baseline (diag) | CS sector (referência) | learned PHOBOS. Script: `mcmc/04_paper_figures/run_5078_5079.sh`.

## Resultado em uma linha
A covariância aprendida quase não move o posterior: todos os deslocamentos de mediana ficam abaixo de 1σ da baseline
(o maior é α_shadowing, ≈1σ), bem menos que o CS sector (até ≈1.8σ). Ver `06_S1-shift`.
