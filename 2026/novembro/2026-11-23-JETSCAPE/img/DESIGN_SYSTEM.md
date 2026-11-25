# Design system: figuras de comparação baseline × modelos de correlação

Referência visual: Mankolli et al., PRC 114, 014905 (2026), `7kjw-vfgp.pdf`.
Objetivo: qualquer figura de uma proposta (1–4) pode ser posta **lado a lado** com a mesma figura de outra proposta
e comparada diretamente. Por isso, **tudo o que não é o resultado fica fixo**: ordem dos painéis, tamanho, eixos,
limites, cores, legendas, intervalos de credibilidade e número de amostras.

Dados e scripts: sempre em `~/Remote-HPC/code-space/my-work/jetscape-emulator/3dglauber-bayes-example`
(cadeias em `mcmc/<experimento>/`). Os scripts devem importar um único módulo de estilo (passo 3), não copiar constantes.

---

## 1. Regras globais

### 1.1 Papéis e cores (iguais em todas as propostas)

| Papel | Cor | Uso | Legenda |
|---|---|---|---|
| Prior | cinza `#BFBFBF`, α=0.8, preenchido | bandas de prior | `90% Prior` |
| Baseline (cov. diagonal) | verde `#2CA02C`; banda α=0.45 com hachura `++++` (como a Fig. 14) | referência em toda figura | `90% C.I. baseline (diag)` |
| Modelo 1 (CS: global, sector…) | vermelho `#E41A1C`, tracejado `(0,(5,3))`, lw=2.5 | | `90% C.I. CS <escopo>` |
| Modelo 2 (RBF: global, sector…) | azul `#1F3FD8`, tracejado `(0,(5,3))`, lw=2.5 | | `90% C.I. RBF <escopo>` |
| Modelo 3 (se houver: propostas 3, 4) | roxo `#7B2D8E`, ponto-traço `(0,(5,2,1,2))` | | `90% C.I. <nome>` |
| Dados experimentais | preto, marcador `o` 3.5 pt, barra de erro 0.8 pt | Fig. 5 | — |

Regra: vermelho = CS, azul = RBF **em qualquer escopo** (global, sector), como a Fig. 14 usa vermelho e azul para as
duas variantes. Assim, a proposta 1 e a 2 ficam diretamente comparáveis.

### 1.2 Estatística (fixa)
- Intervalo de credibilidade: **90%**, central (percentis 5 e 95). Mediana = percentil 50.
- Amostras: mesmo `burnin`, mesmo `thin` e mesmo número de amostras retidas para todos os modelos de uma mesma figura
  (registrar no `info.md`). Bandas preditivas: **N = 10 000** amostras do posterior, semente fixa `12345`.
- KDE 1D: gaussiana refletida nas bordas do prior, largura 0.7×Scott. 2D: histograma 40×40 suavizado (σ=1.2 bin).

### 1.3 Tipografia e saída
- Fonte: DejaVu Sans (padrão do matplotlib, como no artigo); matemática `mathtext.fontset = dejavusans`.
- Tamanhos: rótulos de eixo 14, ticks 11, título de painel 12, legenda 11.
- Logotipo JETSCAPE: **não usar** (o trabalho não é da colaboração).
- Saída: PDF vetorial + PNG 300 dpi, fundo branco, `bbox_inches='tight', pad_inches=0.05`.
- Título geral (`suptitle`): não usar; a descrição fica no `info.md`. Rótulo de proposta/modelo: só na legenda.

### 1.4 Regra de nome
```
<ID>__<proposta>__<comparação>[__<detalhe>].<pdf|png>
```
- `<ID>`: `F28-corner`, `F14-viscosity`, `F05-observables`, `F25-varratio`, `F26-corrmatrix`, `S1-shift`
- `<proposta>`: `global` (1), `sector` (2), `covmom` (3), `gpheur` (4)
- `<comparação>`: `baseline-cs-rbf` (todos juntos) ou `baseline-vs-cs` / `baseline-vs-rbf` (pares)
- `<detalhe>`: página `p01`…, ou sistema `AuAu200`/`dAu200`

Exemplo: `F14-viscosity__sector__baseline-cs-rbf.pdf` e `F14-viscosity__global__baseline-cs-rbf.pdf` são diretamente
comparáveis.

---

## 2. Especificação por grupo

### 01 · `F28-corner`: corner completo, 20 parâmetros (Figs. 28+29 em uma só)
- Layout: triângulo inferior 20×20, ordem da Tab. I: y2, y4, y6, σ_yloss, α_rem, α_shadowing, τ_form, B_G, σ_x, σ_η,
  α_shift, (η/s)T_kink, m_low, m_high, (η/s)_kink, (ζ/s)_max, (ζ/s)T_max, w_ζ, λ_ζ, e_switch.
  Rótulos idênticos a `mcmc/scripts/plot_posterior_corner.py::LABELS`.
- Tamanho: 15 × 15 in. Um arquivo por par: `baseline-vs-cs`, `baseline-vs-rbf`.
- Limites: **intervalo do prior (Tab. I)** em todos os eixos, nunca automático. y4 em [0,4] e y6 em [0,6].
- Baseline: densidade 2D verde preenchida (estilo do artigo) + marginal 1D verde.
  Modelo: contornos 68% e 90% na cor do modelo + marginal 1D na cor do modelo.
- Diagonal: duas linhas de título, `med^{+}_{-}` da baseline (verde) e do modelo (cor do modelo).
- Linhas-guia: mediana com traço sólido fino e limites de 90% com tracejado, na cor de cada série.
  (O artigo usa vermelho/verde para essas linhas; aqui o vermelho é do CS, por isso a adaptação.)

### 02 · `F14-viscosity`: η/s(T) e ζ/s(T)
- Layout: 1×2, η/s à esquerda e ζ/s à direita (como a Fig. 14). Tamanho 11 × 4.2 in.
- Eixos fixos: T ∈ [0.13, 0.30] GeV com ticks de 0.02; η/s ∈ [0, 0.50]; ζ/s ∈ [0, 0.20]. Rótulos `T [GeV]`, `η/s`, `ζ/s`.
- Curvas: grade de 200 pontos em T, calculadas pelas Eqs. (4) e (5) do artigo para cada amostra.
- Camadas, de baixo para cima: prior 90% (cinza), baseline 90% (verde hachurado), CS 90% (bordas vermelhas
  tracejadas), RBF 90% (bordas azuis tracejadas). Um arquivo por proposta, com todos os modelos juntos.
- Legenda: **uma só**, no canto superior esquerdo do painel η/s (no ζ/s ela escondia a banda do CS), nesta ordem: Prior, baseline, CS, RBF.

### 03 · `F05-observables`: posterior dos observáveis, todos os bins
- Layout: grade 6×6, cada painel com o principal em cima (altura 3) e a razão `theory/data` embaixo (altura 1),
  como a Fig. 5. Tamanho da página 18 × 22 in.
- **Ajuste de 24/11**: a calibração do 3dglauber usa 40 observáveis (917 bins) e não tem BRAHMS, dE_T/dη,
  v3(pT), v4(pT) nem r_n. Por isso a página única `p01` tem grade **7×6** com os 40 observáveis, na ordem de grupos
  da Fig. 5: Au-Au dN/dη → d-Au dN/dη → v2(η) → v2(pT) (PHENIX, depois STAR) → v_n integrados → ⟨pT⟩. A legenda ocupa
  as 2 últimas células. A ordem é gerada pelo script e salva no manifesto (`panel_order`).
- Painel: texto no canto superior esquerdo em 3 linhas (sistema e energia em negrito; observável e centralidade em
  itálico; experimento em negrito-itálico) e `N bins` embaixo, no centro, como no artigo.
- Séries: baseline = banda verde 90% preenchida; modelo = bordas da banda 90% tracejadas na cor do modelo; dados em preto.
  Um arquivo por par (`baseline-vs-cs`, `baseline-vs-rbf`), para não sobrepor três bandas.
- Limites de y: calculados **uma vez** (união de dados ± erro e de todas as bandas de todos os modelos de todas as
  propostas, +5%) e salvos no manifesto; razão fixa em [0.5, 1.5].
- Bins fora da calibração: banda oliva `#B5B83A` no lugar do verde, como no artigo.

### 04 · `F25-varratio`: Σ_emu/Σ_exp na diagonal
- **Todos os 40 observáveis calibrados** (regra de 24/11: mostrar tudo o que já está calculado, não só os 3 do
  artigo). Arquivo `F25-varratio__<proposta>__baseline__all`: grade 7×6 na **mesma ordem de painéis da F05**.
- **Cada painel tem a sua barra de cor e escala linear própria**, como na Fig. 25, para poder ser recortado e usado
  sozinho. Título do painel: `Σ_emu/Σ_exp <sistema>` + observável, centralidade e experimento.
- Matriz com só a diagonal colorida, colormap `cividis`, fora da diagonal branco. Tamanho 22 × 24 in.
- Continua sendo gerada também a versão de 3 painéis do artigo (`F25-varratio__<proposta>__baseline`): d-Au dN/dη
  0-5% PHENIX; Au-Au v2(η) 3-15% PHOBOS; Au-Au v2(pT) 20-30% PHENIX (5.5 × 15 in).
- Fonte: `r_i` de `mcmc/02_12_shift_vs_cov_ratio/bin_level.npz` (média em 60 amostras do posterior da baseline).
- **Observação:** os modelos só mudam as correlações, não a diagonal da covariância experimental; por isso esta
  figura é **idêntica em todas as propostas**.

### 05 · `F26-corrmatrix`: matrizes de correlação
- Linhas: os mesmos 3 observáveis da F25. Colunas: `emulador` | `exp. baseline (identidade)` | `exp. CS` | `exp. RBF`.
  Tamanho 20 × 15 in.
- Colormap `cividis`, escala **fixa [0, 1]** em todas as colunas, com uma colorbar comum. (O artigo ajusta a escala
  por painel; aqui ela é fixa, para dar para comparar.)
- Faixas brancas separando regiões de rapidez/setores, como na Fig. 26.
- Título: `Cov(y_i,y_j)/σ_iσ_j (<observável>)` no topo de cada linha; nome da coluna no topo.

### 06 · `S1-shift`: deslocamento do posterior vs σ²_emu/σ²_exp (figura nossa, sem equivalente no artigo)
- Layout: 1×2, CS à esquerda e RBF à direita. Tamanho 11 × 4.5 in.
- x: `R_j = ⟨σ²_emu/σ²_exp⟩_{w_j}`, escala log, **fixa em [10⁻¹, 10³]**, usando todos os bins.
  (Uma linha extra só com os bins do escopo do modelo é opcional, como variante `__scope`.)
- y: `|mediana_modelo − mediana_baseline| / σ_baseline`, **fixo em [0, 2]**, com linha de referência tracejada cinza em 1.
- Marcadores: círculo 6 pt na cor do modelo; rótulo nos 5 maiores deslocamentos, com o nome de exibição de `LABELS`
  (não o nome interno). Spearman no título do painel, para não colidir com os rótulos.

### 07 · `S2-expcov`: covariância e correlação experimental entre todos os bins (figura nossa)
- Grade 2×3: linha de cima **covariância**, linha de baixo **correlação**; colunas baseline (diag) | CS | RBF da
  proposta. Tamanho 27 × 17.5 in. Ordem dos bins = ordem da verossimilhança (`bin_index.csv`: Au-Au 791 → d-Au 126).
- Covariância no espaço da verossimilhança (σ_i = `likelihood_std`, multiplicidades em log), escala **log com a mesma
  faixa em todos os painéis** (10⁻⁹ até o máximo); zeros em branco. Correlação `cividis` fixa em [0, 1].
- Uma barra de cor por painel (recortável). Separadores discretos: entre observáveis cinza claro quase invisível
  (lw 0.15, α 0.25); entre sistemas cinza (lw 0.6, α 0.6).
- Eixos com **contador de bins** 0, 100, …, 800, 917 ("bin index") e o nome do sistema nos eixos opostos (topo/direita).
- Fontes: sector = `mcmc/02_08_sector_input_correlations/*_input_matrices.npz`; CS global = ρ = 1 dentro de cada
  sistema; RBF global = `experimental_global_covariance.py` + `global_rbf_acceptance.json` com os comprimentos do job.
- Script `fig_expcorr.py` (roda no servidor, ver README dos scripts). Arquivo `S2-expcov__<proposta>__baseline-cs-rbf`.
