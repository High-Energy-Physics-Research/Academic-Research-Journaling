# Como as fontes não gaussianas (skew) foram obtidas — proposta 4

**Resumo:** as fontes **não foram inferidas de dados experimentais**. O STAR publica, por bin, o valor
central e um erro. Não publica réplicas nem variações sistemáticas. O que existe é um **toy model**:
1. postulamos três fontes sistemáticas assimétricas (forma, tamanho e skew);
2. geramos réplicas sintéticas com elas;
3. o Tensor PCA recuperou as fontes a partir dessas réplicas.

As cadeias 02_10 e 02_11 usam as fontes **recuperadas**, que são praticamente as postuladas. O teste
valida o **método**: recuperar fontes assimétricas a partir do terceiro cumulante e propagá-las até a
posterior. Ele **não mede** o skew real do experimento.

Código: `3dglauber-bayes-example/study_nongaussian_systematics/toy_model/sources.py` (02_10) e
`sources_detector_split.py` (02_11). Seed 2026, 5000 réplicas.

## O que vem do experimento e o que foi escolhido

| Ingrediente | Origem |
|---|---|
| Observável: Au-Au 200 GeV, STAR v2{2}(η), 20–70%, 21 bins (TPC \|η\|<1, FTPC 2.7<\|η\|<3.9) | **dado real** |
| Valor central μ_i | **dado real**: `HIC_experimental_data/Au-Au-200/STAR/v22_eta_cen_20_70_STAR.dat` |
| σ_total,i (a variância usada na calibração, coluna `err` do mesmo arquivo) | **arquivo da calibração**. É cerca de 4× o erro original do STAR (`*_original_error.dat`, σ ≈ 0.0012 contra σ_total ≈ 0.0045) |
| **Forma** das fontes (direções v̂_a) | **escolhida**: normalização ∝ μ, assimetria frente/trás ∝ sign(η) só na FTPC, borda gaussiana em \|η\|=3.9 (largura 0.45) |
| **Tamanho** das fontes (s_a) | **escolhido**: normalização 5% de μ; assimetria 0.55·σ_total; borda 0.60·σ_total no pico. Juntas, ocupam 15–72% da variância σ_total² do bin |
| **Skew** das fontes | **escolhido**: λ_a ~ Gamma com skew 2.0, 1.5 e 2.5 (k = 4/skew²) |
| σ_stat (o que sobra) | derivado: σ_stat² = σ_total² − Σ_a s_a² v̂_ai². A variância total do bin continua igual à da calibração |
| Réplicas | **sintéticas**: y_r = Σ_a λ_a v̂_a + N(0, σ_stat²), com 5000 réplicas |

## A inferência (o que é de fato inferido)

A partir **só das réplicas sintéticas**:
1. κ̂₃ = terceiro cumulante empírico das réplicas, um tensor 21×21×21.
2. Decomposição simétrica de κ̂₃ (power iteration com reinícios e deflação, depois mínimos quadrados) → direções v̂_a.
3. Escalas s_a: ajuste dos elementos fora da diagonal da covariância das réplicas.
4. Skew_a = peso do tensor / s_a³.
5. σ_stat inferido = σ_total² − sistemático inferido.

Resultado em `data/inferred_sources.json` (02_10) e em `data/detector_split/inferred_sources.json` (02_11).

| Fonte | Skew postulado | Skew recuperado | Erro de direção: Tensor PCA | Erro de direção: PCA (covariância) |
|---|---|---|---|---|
| **02_10** normalização global | 2.0 | 2.02 | 2.9° | 7.1° |
| **02_10** assimetria FTPC | 1.5 | 1.61 | 2.4° | 6.5° |
| **02_10** borda FTPC | 2.5 | 2.34 | 6.2° | **89.3°** |
| **02_11** normalização TPC | 2.0 | 1.94 | 3.0° | 2.4° |
| **02_11** normalização FTPC | 2.0 | 1.91 | 6.4° | **80.5°** |
| **02_11** assimetria FTPC | 1.5 | 1.42 | 3.8° | 6.3° |
| **02_11** borda FTPC | 2.5 | 2.36 | 10.5° | **89.6°** |

## Como entra na posterior (02_10, 02_11)

- **Verossimilhança:** y_exp = μ(θ) + Σ_a λ_a v̂_a + ε, com ε ~ N(0, diag σ_stat²).
- **Prior de λ_a:** Gamma deslocada e escalada com os (s_a, skew_a) recuperados.
- **Amostragem:** os λ_a são amostrados junto com θ (23 ou 24 parâmetros). A marginalização sobre λ é feita pelo MCMC.

## O que se pode e o que não se pode concluir

- **Pode:** com uma assimetria plausível, do tamanho do sistemático declarado, a posterior de θ quase não muda (F14, F05). O Tensor PCA identifica fontes que a PCA não separa.
- **Não pode:** afirmar que o skew do STAR é 2.0 / 1.5 / 2.5, nem que essas são as fontes reais.
- **Para virar inferência a partir de dados:** são necessárias réplicas **reais**, isto é, as variações sistemáticas do experimento (cortes, métodos de não-fluxo, calibração da FTPC), obtidas da colaboração ou reconstruídas das tabelas suplementares. O código de inferência é o mesmo: só troca `replicas.npz`.

## Registro original da decisão
- Roteiro com todas as etapas: `IFUSP-Master-Program/2026-11-Poster_WORKING_3RD_QC_SCHOOL/roteiro/nongaussian_systematics_study.tex`
  - slide 5: "Sizes and skewness are toy choices inside the published budget, not STAR's systematic breakdown";
  - backup "honest limits": "Toy sources. Shapes, sizes and skewness are chosen; STAR does not publish the breakdown".
- Conversa exportada: `.../conversa/conversa_completa.md`
  - linhas 163 e 268: o HEPData não publica terceiros cumulantes;
  - linha 1412: os tamanhos e as assimetrias foram escolhas para o toy.

## Inferência com dados reais (degrau 2): existe, mas não entrou nas cadeias
Feita em 16–17/11/2026, numa cópia local que **não está no HPC**:
`IFUSP-Heavy-Ions/3dglauber-bayes-exemple/3dglauber-bayes-example/study_nongaussian_systematics/`.
Arquivos: `MAPA.tex`, `MAPA_regiao_eta.tex`, `cross_detector_overlaps.md` (136 comparações, cerca de 695 bins),
`replicas_survey.md`, `fatia2a/`, `regiao_eta/`.

Ideia: o sistemático de um detector se repete em todas as suas medidas. A diferença entre dois detectores
medindo a mesma coisa funciona como réplica real: y_A − y_B = sys_A − sys_B + ruído. Isso vale para várias
energias e sistemas (Au+Au e Cu+Cu).

| Fatia | Dados | 2º momento | 3º momento |
|---|---|---|---|
| **2a** normalização de dN/dη em η≈0 | PHOBOS, PHENIX, sPHENIX e BRAHMS; 10 conjuntos de 19.6 a 200 GeV | **medido**: ρ_ef = 0.95–1.00 entre classes de centralidade; covariância ≈ offset + inclinação em centralidade + 0.5–2% diagonal (apoia o CS ρ=1) | skew dos resíduos **−0.08 ± 0.24**, compatível com zero; por diferenças, só a assimetria *relativa* é identificável; N efetivo ≈ 7 |
| **R1** parte ímpar em η (east/west) | os 11 arquivos do PHOBOS da calibração | forma medida: degrau de cerca de 2% na fronteira Octagon/Rings; 10–14% da incerteza citada | **não mensurável**: há uma única realização da fonte |
| R2 simetria do STAR v2(η) | — | estava "agora", não concluída | — |
| R3–R5, 2d | — | a fazer | — |

**Degrau 3** (as fontes aprendidas viram nuisances na calibração): **não foi feito**.
> Atualização 25/11/2026: o degrau 3 com o **2º momento** aprendido (covariância do PHOBOS dN/dη, fatia 2a) foi rodado
> (job 5079) e virou a proposta 3, `../3-Third_sensitivity_test_cov_inference/`. Nenhuma posterior usa um skew
inferido de dados reais. Os dados reais disponíveis não sustentam um skew diferente de zero.
Ponto em aberto da 2a: o JETSCAPE parece ter usado os sistemáticos do PHOBOS em 90% C.L. como se fossem 1σ, inflando-os em cerca de 1.6×.
