# RBF global em coordenadas físicas: método do orientador

Fonte: anotações no quadro, `IMG_9894.JPG`, `IMG_9949.JPG`, `IMG_9950.JPG` (24/11/2026).
Leitura minha; **confirmar com o orientador** os pontos marcados com (?).

## Problema
O RBF precisa de uma distância entre bins. Bins de observáveis diferentes estão em variáveis com unidades
diferentes (pT em GeV/c, η adimensional, centralidade em %). Distância de índice (`x_i = i/(N−1)`, como no
`hic-bayes` run_005) não tem sentido físico e depende da ordem dos bins.

## Ideia do quadro
1. **Todo bin vira um ponto num espaço físico comum (pT, η)**, não só na variável em que a medida é diferencial.
   - ALICE v2(pT) (circulado no quadro): diferencial em pT, medido numa janela η_min ≤ η ≤ η_max → coordenada η =
     centro da janela.
   - ATLAS v2(η): diferencial em η, integrado em p_T,min ≤ pT ≤ p_T,max → coordenada pT = centro (ou média) da janela.
   - O esboço da grade (pT na horizontal, η na vertical) mostra isso: uma fileira de pontos de v2(pT) a η fixo, uma
     coluna de v2(η) a pT fixo, e pontos isolados de outras medidas.
2. **O kernel é o produto de gaussianas, uma por eixo, cada uma com seu comprimento**:

   C_ij ≈ exp(−ΔpT²/σ_pT²) · exp(−Δη²/σ_η²)

   Cada razão ΔpT/σ_pT e Δη/σ_η é adimensional, então **a exponencial resolve o problema de unidades**: grandezas de
   unidades diferentes nunca se somam diretamente. Para medidas que só variam em pT, sobra C ≈ exp(−ΔpT²/σ_pT²)
   (última linha do quadro).
3. **Fator entre tipos de observável** (chave à direita no quadro):
   - `V ↔ V`: mesmo tipo de observável (por exemplo, v2 com v2) → kernel completo.
   - `α V`: tipos relacionados → kernel multiplicado por um fator α (?).
   - Para tipos sem relação, três opções: **nulo** (0), **neutro** (?) ou **inverso** (correlação negativa).

## Proposta de implementação (3dglauber)
- Coordenadas por bin: (pT, η, centralidade). Centralidade entra como 3º eixo com o mesmo kernel; o RBF sector já
  usa σ_cent = 10 p.p. (?).
- Comprimentos padrão iguais aos do RBF sector: σ_pT = 0.5 GeV/c, σ_η = 1.0, σ_cent = 10.
- Para cada eixo em que a medida é integrada, a coordenada é o centro da janela de aceitação, que precisa ser
  tabelado por experimento/medida (PHENIX, STAR, PHOBOS, BRAHMS).
- Correlação entre sistemas (Au-Au ↔ d-Au) = 0, como no CS global (`scope = system`).
- Matriz de fatores α por par de tipos (dN/dη, dE_T/dη, ⟨pT⟩, v_n(pT), v_n(η), v_n(cent), r_n(η)); começar com
  α = 1 dentro do mesmo tipo e 0 entre tipos (opção "nulo"), e depois testar as outras opções.
- Conferir que a matriz é positiva semidefinida. O produto de kernels gaussianos é PSD; os fatores α entre blocos
  podem quebrar isso, então verificar autovalores e projetar se preciso.
- Rodar com `--nwalkers 200 --nburnsteps 20000 20000` em `mcmc/02_02_global_rbf/`.

## Perguntas para o orientador
1. A coordenada do eixo integrado é o centro da janela ou a média ponderada pelo espectro?
2. Centralidade entra como eixo do kernel?
3. O que significam exatamente "αV" e "neutro", e quais pares de observáveis seriam "inversos"?

## Decisões e implementação (24/11/2026)
- **Faixa comum**: um comprimento por eixo, em unidades físicas: σ_pT = 0.5 GeV/c, σ_η = 1, σ_cent = 10 p.p.
  A faixa própria (fração da janela de cada medida) não define a distância entre bins de medidas diferentes e,
  com σ variando por par, pode quebrar a positividade. Se for testada depois, usar o kernel de Gibbs.
- **Fator entre tipos de observável: neutro (α = 1).** Au-Au e d-Au continuam sem correlação entre si.
- **Coordenada de um eixo integrado = ponto médio da janela de aceitação.** Todas as janelas em η são simétricas,
  então dão η = 0. As medidas da STAR integradas em pT usam [0.15, 2.0] → 1.075 GeV/c. As medidas sobre
  "todo pT" (PHOBOS dN/dη e v2(η), PHENIX d-Au dN/dη e v2(η), os 4 ⟨pT⟩) usam a convenção [0, 1.5] → 0.75 GeV/c.
- As janelas foram tiradas dos textos completos das publicações (refs. na tabela
  `3dglauber-bayes-example/global_rbf_acceptance.json`).
- Validação (`--check-covariance`): matriz positiva semidefinida nos dois sistemas. Au-Au: correlação média fora da
  diagonal 0.047, 3.4% dos pares acima de 0.5. d-Au: 0.19 e 18%.
- Cadeia: `mcmc/02_02_global_rbf/`, job Condor 5078 (qgp1), 200 walkers, 20 000 de burn-in + 20 000 de produção.
