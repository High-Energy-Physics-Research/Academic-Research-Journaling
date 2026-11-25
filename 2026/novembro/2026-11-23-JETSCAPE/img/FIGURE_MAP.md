# Mapa de figuras (versão simplificada, 24/11/2026)

Artigo de referência: Mankolli et al., PRC 114, 014905 (2026), `7kjw-vfgp.pdf`.
Estilo, cores, eixos e nomes: `DESIGN_SYSTEM.md`.

Cada proposta (`1-…global`, `2-…sector`, `3-…cov_inference`, `4-…cov_momentum`, `5-GP_heuristics`) tem as mesmas 7 pastas:
(renumeração de 25/11/2026: a antiga 3 `cov_momentum` virou a 4, a antiga 4 `GP_heuristics` virou a 5; a 3 nova é a covariância
aprendida de dados, job 5079.)

| Pasta | Artigo | Conteúdo | Comparação |
|---|---|---|---|
| `01_F28-corner` | Figs. 28+29 (uma só) | Corner 20×20 | baseline vs cada modelo |
| `02_F14-viscosity` | Fig. 14 | η/s(T), ζ/s(T) | baseline + todos os modelos juntos (no lugar de Au-Au vs d-Au) |
| `03_F05-observables` | Fig. 5 | Posterior dos observáveis, todos os bins | baseline vs cada modelo |
| `04_F25-varratio` | Fig. 25 | Σ_emu/Σ_exp na diagonal | igual para todos (referência) |
| `05_F26-corrmatrix` | Fig. 26 | Correlação do emulador vs correlação experimental | baseline (identidade), CS, RBF |
| `06_S1-shift` | — (nossa) | Deslocamento do posterior vs σ²_emu/σ²_exp | CS e RBF |
| `07_S2-expcov` | — (nossa) | Covariância e correlação experimental, todos os 917 bins | baseline, CS, RBF |

Pastas antigas mantidas (sem mover): `1/marginals` (→ 01), `1/posterior_predictive` (→ 03), `1/shear_bulk_viscosity` (→ 02),
`2/posterior_predictive_{cs,rbf}_sector` (→ 03), arquivos soltos em `2/` (→ 01, 03, 06).
