# AGEscore Brasil — Notas de Arquitetura e Deploy

> Projeto INDEPENDENTE do NeuroClin. Está fisicamente dentro de `neuroclin-v2/agescore/`
> apenas por organização de pastas — não tem relação com o sistema de laudos NeuroClin.

## Domínio e hospedagem (verificado em 09/06/2026 via DNS no registro.br)

- **`agescorebrasil.com.br`** → registro A `185.199.108.153` → **GitHub Pages**
  - Servido a partir deste repositório: `pedrodonizetti347/agescore` (branch `main`)
  - Arquivo `CNAME` na raiz confirma o domínio
  - Página principal (`index.html`) = login com 4 perfis (Admin, ADM2, Coordenador, Aplicador)
- **`participante.agescorebrasil.com.br`** → registro A `185.158.133.1` → **Lovable** (app separado, NÃO é este repo)
  - Existe TXT `_lovable.participante.agescorebrasil.com.br` confirmando
- **`www.agescorebrasil.com.br`** → CNAME para `agescorebrasil.com.br`

## Como publicar alterações no site principal

1. Editar `index.html` (e/ou `participar.html`) diretamente neste repo (`agescore/`)
2. `git add` + `git commit` + `git push origin main`
3. GitHub Pages republica automaticamente em ~1-2 min

❌ **NÃO usar `firebase deploy --only hosting --project agescorebrasil`** — esse projeto Firebase
NÃO é o que serve `agescorebrasil.com.br` (a tentativa anterior, em 05/06/2026, fez deploy para
o lugar errado e só uma das 3 correções pediu foi parar no ar).

## Histórico de divergência (09/06/2026)

Em 09/06/2026 foram encontradas 3 versões diferentes do `index.html`:
- HEAD do git (commit f3ef65f, 02/06/2026)
- Working tree deste repo, com mudanças não commitadas desde 01/06/2026 (estrutura antiga:
  Embutidos com `fixedScore`, Ovos sem "(mexido/omelete)")
- `C:\Users\Pedro Donizetti\Desktop\NEUROCLIN\AGESCOREBRASIL\index.html` — versão mais
  recente (05/06/2026), já com Embutidos `mixedScore`/`cs[]` e Ovos com "(mexido/omelete)"
  — esta é a que bateu com o que está realmente no ar.

**Lição:** antes de editar, sempre conferir `git log -1` + `git status` neste repo E comparar
com a cópia em `Desktop\NEUROCLIN\AGESCOREBRASIL\index.html`, que costuma ser onde o Dr. Pedro
testa/prepara as versões mais novas. Em caso de dúvida, perguntar — não assumir qual versão é a
"real".

## Estrutura do questionário (referência rápida)

- `Q1C` (Q1 — Proteínas): categorias `carnes`, `aves`, `suinos`, `ovos`, `embutidos`, `peixes`
  - `ovos.methods` = `['Coz./Micro','Grelhado (mexido e omelete)','Frito']` (corrigido em 09/06/2026)
  - `embutidos` usa `mixedScore:true` + `cs:[coz/micro, frito_em_oleo]` por alimento
    - Salsicha `cs:[2.2,3.4]`, Bacon `cs:[9.9,13.7]`, Linguiça seca `cs:[null,6.6]`
- `Q2C` (Q2 — Laticínios, Óleos e Outros): categorias `laticinios`, `oleos`, `fastfood`,
  `preparacoes`, `diversos`
  - Tapioca/crepioca foi removida da categoria `preparacoes` em 09/06/2026
- Aba "Resumo e Exportação": `updateResumoTab()` deve usar os IDs `res-tot` e `res-day`
  (corrigido em 09/06/2026 — antes usava `res-total`/`res-daily`, que não existiam no HTML)
