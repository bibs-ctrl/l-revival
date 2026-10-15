# Reviews de produtos no site localrevival.co (Local Revival) — extração Judge.me, temas, claims e sinais de autenticidade

Convenções: "dump" = `/home/user/l-revival/research_notes/Pain points sabonetes Local Revival/localrevival_reviews_raw.json` (e `.csv`, mesmas 3.107 linhas; campos: review_id, produto, nota, data UTC, autor, verified_buyer, badge de origem, título, texto literal, fotos, resposta do dono). IDs de review citados abaixo = primeiros 6-8 caracteres do `review_id` no dump. "T&H 16oz" = Tallow & Honey Body Wash - 16oz (produto principal). Contagens por tema = busca por palavra-chave/regex sobre título+texto (aproximadas; precisão amostral ~85-95% nos temas positivos, ver Gaps); contagens de críticas em reviews de 1-4 estrelas = leitura manual das 268 reviews. Datas das reviews: 2024-09-06 a 2026-10-08; coleta feita em 2026-10-15.

[Dump JSON](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json) · [Dump CSV](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.csv)

---

## 0. Catálogo, plataforma de reviews e cobertura da extração

### Takeaway
O site usa **Judge.me** (Shopify). Há 23 produtos em /products.json; só 14 páginas têm reviews. Extraí 3.107 reviews únicas (T&H 16oz: 2.837 de 2.862 reportadas = 99,1%; demais produtos ~100%). Nota média global 4,874; T&H 16oz 4,871 (widget mostra 4,87).

### Cited Findings
- Plataforma = Judge.me: a home carrega `cdn.shopify.com/extensions/.../judgeme-785/assets/loader.js`, classes `jdgm-*`, `jdgmSettings`, e os widgets de review vêm de `judge.me/reviews/reviews_for_widget` — [localrevival.co home](https://localrevival.co/)
- Home: "Tallow & Honey Body Wash - 16oz — 2862 reviews — $36.00"; descrição: "Unlike synthetic store bought body washes our formula is free from harsh detergents, filler ingredients and artificial fragrances." — [localrevival.co](https://localrevival.co/)
- Catálogo completo (23 produtos, `products.json`) — [products.json](https://localrevival.co/products.json?limit=250). Reviews por página (contagem e média lidas do widget da própria página de produto; extraído = linhas únicas no dump):

| Produto (URL /products/…) | Preço | Reviews (widget) | Média | Extraídas | Obs. |
|---|---|---|---|---|---|
| [tallow-honey-bodywash](https://localrevival.co/products/tallow-honey-bodywash) — T&H Body Wash 16oz (14 variações de aroma) | $36,00 | 2.862 | 4,87 | 2.837 | 12×1★, 15×2★, 49×3★, 175×4★, 2.586×5★ |
| [tallow-honey-body-wash-1-gallon-refill-pouch](https://localrevival.co/products/tallow-honey-body-wash-1-gallon-refill-pouch) — Gallon Refill | $178 (pouch plástico) / $198 (growler vidro) | 101 | 4,89 | 99 | 1×2★, 5×4★, 93×5★ |
| [tallow-honey-body-wash-limitedscents](https://localrevival.co/products/tallow-honey-body-wash-limitedscents) — 16oz Limited Release (hidden) | $36,00 | 28 | 4,96 | 28 | 1×4★, 27×5★ |
| [olive-aloe-body-wash](https://localrevival.co/products/olive-aloe-body-wash) — Olive & Aloe (sem coco, sem fragrância) | $38 (16oz); $186 pouch / $198 growler (galão) | 9 | 5,00 | 9 | `published_at` 2026-09-17 (primeira review em 2026-09-07); 9×5★ |
| [silicone-sleeve-16oz](https://localrevival.co/products/silicone-sleeve-16oz) — capa de silicone p/ garrafa | $12,00 | 41 | 5,00 | 41 | todas 5★; variantes Charcoal/Sand `available:false` |
| 8oz scent bundle (9 páginas: [almond-vanilla-8oz](https://localrevival.co/products/almond-vanilla-8oz), local-revival-8oz-1, tobacco-vanilla-8oz, lavender-bloom-8oz, blue-citrus-8oz, unscented-8oz, cedarwood-8oz, high-sierra-8oz, wild-coast-8oz) | $23,00 cada | 76 (compartidas) | 4,86 | 76 | grupo Judge.me: as mesmas 76 aparecem em todas as 9 páginas (Almond Vanilla 20, Wild Coast 12, Lavender 12, Local Revival 9, Cedarwood 6, Blue Citrus 6, Unscented 6, Tobacco Vanilla 5); High Sierra 0 próprias |
| [eucalyptus-mint-8oz-new](https://localrevival.co/products/eucalyptus-mint-8oz-new) | $23,00 | 7 | 4,29 | 7 | 5×5★, 1×4★, 1×1★ |
| [pump-gallon-refill-copy](https://localrevival.co/products/pump-gallon-refill-copy) — Pump Glass Bottles | $2,50 | 6 | 5,00 | 6 | |
| [free-subscription-sample-set](https://localrevival.co/products/free-subscription-sample-set) — Five Scent Sample Set (hidden) | $15,00 | 2 | 5,00 | 2 | |
| [local-revival-gift-card](https://localrevival.co/products/local-revival-gift-card) | $15–$100 | 2 | 5,00 | 2 | |
| champagne-coast-8oz-new-copy (Barbershop 8oz), slow-burn-seasonal-scent (Slow Burn 8oz), sample-pack-copy (Sampler Pack $12), pump-gallon-refill ($4), free-upgrade-to-quarterly-subscription ($0, hidden) | $23 / $23 / $12 / $4 / $0 | 0 | — | 0 | sem reviews |

  Fonte dos preços/variantes: [products.json](https://localrevival.co/products.json?limit=250); contagens/médias: widget Judge.me em cada página de produto (atributos `data-number-of-reviews`/`data-average-rating`).
- Endpoint público usado: `https://judge.me/reviews/reviews_for_widget?url=localrevival.co&shop_domain=localrevival.co&platform=shopify&per_page=30&page=N&product_id=<id>` (retorna JSON `{html,total_count,page}`); filtros funcionais: `filter_rating=1..5`, `sort_by=created_at|most_helpful|with_pictures`, `sort_dir=asc|desc`, `search=<palavra>`. O widget só entrega até a página 33 (~990 linhas) por consulta; para T&H 16oz combinei filter_rating, ordenação asc/desc e ~100 buscas por palavra-chave, deduplicando por review_id (cumulativo: 2.837/2.862) — [Judge.me widget T&H](https://judge.me/reviews/reviews_for_widget?url=localrevival.co&shop_domain=localrevival.co&platform=shopify&per_page=30&page=1&product_id=8950214328547)
- Contagens por nota via `filter_rating` no widget T&H: 1★=12, 2★=15, 3★=49, 4★=175, 5★=2.611 (soma 2.862); extraí 2.586 5★ (25 5★ não retornadas por nenhuma consulta) — [Judge.me widget T&H](https://judge.me/reviews/reviews_for_widget?url=localrevival.co&shop_domain=localrevival.co&platform=shopify&per_page=30&page=1&product_id=8950214328547&filter_rating=5)
- Sitemap: 5 coleções (frontpage, tallow-honey-body-wash-refills, limited-release-collection, build-your-own-bundle, accessories) e páginas contact, affiliate-sign-up, discovery-set, spring-scents, olive-aloe-teaser; não há páginas /about ou /faq (404) — [sitemap.xml](https://localrevival.co/sitemap.xml)
- Preço por onça (cálculo): 16oz $36 = $2,25/oz; 8oz $23 = $2,875/oz; galão pouch $178/128oz = $1,39/oz; site diz "$21 a bottle when you subscribe" para o galão — [Gallon Refill](https://localrevival.co/products/tallow-honey-body-wash-1-gallon-refill-pouch)

### Inferences
- 91% das reviews (2.837/3.107) são do T&H 16oz: qualquer análise de "dor" do consumidor reflete essencialmente esse produto. Olive & Aloe (9) e demais não têm massa estatística.
- Os grupos 8oz e as reviews por-item de pedidos multi-produto geram o mesmo texto repetido em várias páginas (ver seção 5); usar IDs únicos evita dupla contagem.

### Gaps
- 25 reviews do T&H 16oz (0,9%) e 2 do refill não foram recuperáveis (o widget limita a ~990 linhas/consulta e algumas linhas parecem não ser exibidas); todas as 25 faltantes são 5★ por diferença de contagem — não há como afirmar qual texto.
- Média por produto vem do widget (arredondada a 2 casas); não há distribuição por estrela publicada pelo Judge.me além dos contadores `filter_rating`.
- Barbershop, Slow Burn, Toasted Sugar, Floral Haze, Citrus Woods aparecem como aromas/variantes sazonais, mas as páginas 8oz deles têm 0 reviews.

---

## 1. Temas recorrentes (positivos e negativos) e distribuição de notas

### Takeaway
Distribuição extremamente enviesada para 5★ (91,4%); nota ≤3★ = 81 reviews (2,6%). Os positivos giram em torno de cheiro, pele macia/hidratada sem loção, "ingredientes limpos/não-tóxico" e ausência de resíduo; as críticas recorrentes são operacionais (bomba que não encaixa/não funciona, garrafa de vidro), pele ressecada em uma minoria, pouca espuma, cheiro não-agradável/fraco e preço.

### Cited Findings
**Distribuição de notas (dump, N=3.107 reviews únicas):** 5★ 2.839 (91,37%); 4★ 187 (6,02%); 3★ 52 (1,67%); 2★ 16 (0,51%); 1★ 13 (0,42%); média 4,874. T&H 16oz (N=2.837): 5★ 2.586 (91,15%) / 4★ 175 (6,17%) / 3★ 49 (1,73%) / 2★ 15 (0,53%) / 1★ 12 (0,42%); média 4,871. ≤4★ = 268 (8,6%). Nota média por trimestre (todas as reviews): 2024Q4 4,74; 2025Q1 4,79; 2025Q2 4,89; 2025Q3 4,92; 2025Q4 4,86; 2026Q1 4,82; 2026Q2 4,84; 2026Q3 4,95 — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json). Tamanho do texto: mediana 23 palavras, média 31; 650 reviews (20,9%) têm ≤10 palavras; 98 têm ≥100 — mesma fonte.

**Temas positivos (N=3.107; % do total; T&H 16oz difere ≤±1,5 pp):**

| Tema | n | % |
|---|---|---|
| Cheiro mencionado (qualquer tom) | 1.420 | 45,7 |
| Cheiro elogiado ("smells great/amazing/…") | 953 | 30,7 |
| "Limpo/fresco" (clean/fresh/squeaky clean) | 790 | 25,4 |
| Hidratada / não resseca / "moisturized" | 735 | 23,7 |
| Ingredientes naturais/limpos/sem químicos | 700 | 22,5 |
| Pele macia/lisa (soft/smooth/silky) | 642 | 20,7 |
| Garrafa de vidro/embalagem (neutro ou positivo) | 487 | 15,7 |
| Espuma/lather (qualquer tom) | 372 | 12,0 |
| Marca honesta/pequeno negócio/EUA | 353 | 11,4 |
| Recompra/assinatura/reorder/"never go back" | 351 | 11,3 |
| Coceira/vermelhidão/irritação (menção; majoritariamente alívio) | 283 | 9,1 |
| Família/crianças/bebê/marido | 257 | 8,3 |
| "Tallow" | 220 | 7,1 |
| Refil/galão/bomba | 216 | 7,0 |
| Atendimento ao cliente/envio elogiado | 177 | 5,7 |
| Comparação com sabonete comum (padrão amplo) | 169 | 5,4 |
| Preço mencionado (caro/vale a pena) | 127 | 4,1 |
| Sem resíduo/filme/"slimy"/enxágue limpo | 125 | 4,0 |
| Pele sensível | 122 | 3,9 |
| Homens/masculino/cologne | 117 | 3,8 |
| Uso em cabelo/rosto/barba | 115 | 3,7 |
| "Honey" | 110 | 3,5 |
| Eczema/psoríase/dermatite/rosácea (padrão amplo) | 109 | 3,5 |
| Spa/relax/luxo | 93 | 3,0 |
| Rende/dura (lasts/a little goes a long way) | 76 | 2,4 |
| Presente | 64 | 2,1 |
| Acne | 18 | 0,6 |

Complementos (N=3.107): palavra "eczema" 89 (2,9%); "psoriasis" 19 (0,6%); "dermatitis" 5; hives/rosácea/KP/ictiose/seb. derm. 9 (0,3%); menção a alívio de pele seca/escamosa/coceira 230 (7,4%); "não preciso mais de loção/hidratante" explícito 50 (1,6%); crianças/bebê/filhos 229 (7,4%); gravidez/lactação 13 (0,4%); elogio a atendimento/troca/reenvio 90 (2,9%); veio por anúncio/redes/indicação/amostra 90 (2,9%); sampler/discovery set 78 (2,5%); pede mais aromas/restock/edição limitada permanente 104 (3,3%); pede outros produtos (loção, shampoo, rosto, etc.) 92 (3,0%); cita sleeve 39 (1,3%) — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json).

**Críticas (leitura manual das 81 reviews 1-3★ e das 187 de 4★; categorias não exclusivas):**

| Crítica | 1-3★ (n=81) | 4★ (n=187) | 5★ (verificado à mão) | Total ≈ |
|---|---|---|---|---|
| Pele seca/apertada/"não hidrata o suficiente" depois do uso | 22 (27%) | ~13 | n/v | ≥35 |
| Bomba/dispensador não encaixa, não funciona, quebrou, ausente | 16 (20%) | ~25 | 14 | ≈55 (1,8%) |
| Cheiro desagradável / forte demais / diferente do esperado | 14 (17%) | ~27 | n/v | ≥41 |
| Cheiro fraco / some rápido | 5 (6%) | ~9 | n/v | ≥14 |
| Garrafa de vidro: quebrou, escorrega, medo, peso | 10 (12%) | ~9 | n/v | ≥19 |
| Pouca espuma / precisa de muitas bombadas | 9 (11%) | ~24 | n/v | ≥33 |
| Preço alto | 6 (7%) | ~11 | ~27 ("pricey but worth it") | ≈44 |
| Consistência fina/líquida demais (ou grossa/entope na bomba) | 3 | ~7 | n/v | ≥10 |
| Vazou/danificado/item errado/atraso | 4 | ~2 | ~9 (resolvidos pelo suporte) | ≈15 |
| Filme/resíduo/escorregadio | 3 | 2 | n/v | ≥5 |
| Reação/ardência (olhos, rosto, "KP", área íntima) | 2 | ~4 | 1 (canela) | ≥7 |
| Sem benefício p/ eczema | 1 | 1 | — | 2 |
| Review sem uso ainda ("too early", "haven't used it") | 7 | ~6 | — | — |

(n/v = categorias em 5★ não verificadas manualmente; ver Gaps. "Pump" = soma da leitura manual de 1-4★ + 14 casos em 5★ confirmados dentre 20 sinalizados pela regex.) Taxa de reclamação de bomba em 1-4★ por trimestre (regex): 2025Q1 4,5%; 2025Q2 1,6%; 2025Q3 0,2%; 2025Q4 1,4%; 2026Q1 1,7%; 2026Q2 1,1%; 2026Q3 0% (0/587) — fonte: dump. Uma review 4★ de 2026-05-22 diz "LOVE the new pumps. They're much easier to use!" (8565ba91).

**Verbatims — positivas (T&H 16oz, salvo indicado):**
- "Makes my skin feel soft and clean, No slime that's hard to rinse off or soap scum in the sink or tub. Fresh, clean smell. There is no down side!" — Kyle C., destacado na home — [localrevival.co](https://localrevival.co/)
- "Great soap, great ingredients, great team behind the product. Love that they use all natural ingredients and it comes in a glass bottle too!" — Jess M., destacado na home — [localrevival.co](https://localrevival.co/)
- "I don't use lotion anymore since I started using Local Revival." (4★, Robin McSweeny, 2025-07-17, 8d4027) — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- "I used to have to put lotion on my back all the time, now I just use Local Revival body wash and no need for the extra lotion." (4★, Chikira Castle, 2026-01-24, 2d11d8) — idem
- "I suffer from chronic hives and have tried numerous cleansing products to keep them at bay, nothing worked. Until now- Revival is the only cleanser that doesn't cause my skin to welt up!" (5★, Keri C., 2026-05-13, ac7338) — idem
- "I have eczema and most washes leave me with a low grade rash or itching. This wash really suprised me! My skin feels clear and soft." (5★, anon, 2026-03-16, c1769a) — idem
- "I have not had any psoriasis breakouts, and my husband's skin rash is gone." (5★, Angela McMahon, não verificada, 2026-04-14, ae6861) — idem
- "I tried this for my son (6 years old) who has been really struggling with eczema issues and really bad rashes. His skin almost completely cleared up after two uses, and now after a whole month his skin only has one minor spot still healing." (5★, Maggie King, 2025-11-10, 356d6d) — idem
- "I have mast cell activation allergic reactions and literally haven't found a body wash I don't react to - until this one!" (5★, Danielle Gill, 2025-04-20, 0a1cd5) — idem
- "I have Sjogren's disease and my skin is always so dry. Within the first few days of using this body wash I knew I'd be a customer for life." (5★, Amy, 2025-08-12, 7d5a19) — idem
- "So better than seeing a dermatologist :)." (5★, Debra Kopcio, 2026-05-16, e8ea55; diz ter chegado via anúncio no Facebook) — idem
- "You may think "beef tallow" and associate that with clogged drains/soap residue. Couldn't be more wrong. It rinses off of everything very cleanly!" (5★, J.E., 2025-04-24, 5407a5) — idem
- "I live in Florida and have hard water. I'm shocked how clean and soft my skin feels." (4★, Jen C., 2025-06-28, 00c9e2) — idem

**Verbatims — críticas (T&H 16oz, salvo indicado):**
- Pele seca: "I could only use it for about 4 days before i had to switch back to native due to my skin got so dry and itchy" (1★, Roy, 2025-12-15, 2286a5); "I wanted a face wash that was made with natural ingredients including tallow. I've used this for a few days and my face is dried out and feels sunburnt." (1★, anon, 2026-05-22, 49698f); "This body wash has made my skin very dry. I was hoping for a better outcome" (1★, Catherine, 2026-03-21, 9d2a9c); "I loved the balm so I was hoping to love the bodywash. I did not. My skin felt very dry and completely void of moisture." (1★, stacy, 2025-06-22, 89379c) — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- Bomba: "Both pumps don't work on the bottles, I need to open the top to then pour body wash on the cloth or my hands. please send me replacement pumps as its inconvenient to use the product not to mention dangerous." (2★, anon, 2025-11-25, 32cea4); "I love the body wash. minimal suds, nice fragrance. … But the dispenser didn't work. It wouldn't unscrew to dispense the liquid. … This happens with 50% of the dispensers I get with product." (4★, patricia caruso, 2025-01-16, 897a69); "That's an EXPENSIVE body wash to come with an inoperable pump." (3★, Olga, 2025-12-30, 1be33d); "Watch for clogged pumps … it squirted DIRECTLY into my left eye." (4★, Shelby, 2026-01-23, 9db723) — idem
- Vidro: "Literally dropped it in the shower 3 days after I received it. Product was ok but using a glass container makes no sense." (1★, Peter, 2026-03-30, 7bf790); "The bottle slipped out of my hand and broke into a million pieces in the tub. Thankfully it did not cut my foot open." (2★, Marie D., 2026-02-20, 47c9b7); "Glass bottle and kids don't mix in the shower. I transferred it to an aluminium bottle with a pump." (3★, Joy G., não verificada, 2026-01-07, 3a2d40); "Glass and showers don't mix. They should offer it at a discount or some deal at least." (4★, Cal, 2026-08-22, ba5d98, sobre a Silicone Sleeve) — idem
- Cheiro: "The smell is not good at all and it doesn't lather very well." (1★, Jessica, 2024-11-07, 72ac90); "I think it smells to much like medicine." (2★, anon, 2025-12-07, de12f4); "The scent is more cigarette ashtray than woodsy and it doesn't lather as well as i had hoped (takes 5 - 6 pumps per shower to get full body clean) HOWEVER I enjoy the way it makes my skin feel" (3★, S Bell, 2026-02-16, 4df0e0); "The lavender was overpowering." (1★, anon, 2025-12-14, a15d86); "Scent is almost non-existent. Wish I had chosen a different scent." (3★, Amy, 2024-12-28, b17031); "I only wish I could smell the scent before purchasing." (4★, David, 2026-10-08, 609783) — idem
- Espuma: "I'm familiar with Tallow and know that it doesn't foam/suds up super well, but this product barely does. I have to use so much (6-7 pumps) just to achieve a semblance of lather." (3★, Jenna, 2025-04-15, 3281d6); "It took a little getting used to. 6ft, 215lb guy took 5-8 pumps to get completely s…" (4★, Christopher Lestingi, 2026-03-15, bbd754) — idem
- Preço: "It's basically expensive lotion soap" (2★, Bill, 2024-12-16, 7bcfb9); "Overpriced for the amount you get." (3★, Andrew J Assereto, 2026-05-17, 04a540); "44 bucks a month for soap isn't really realistic for me." (4★, Christina Hall, 2026-08-28, c64d22) — idem
- Consistência/lote: "Product good… it's really fluid not thick so when you squirt it out it shots far." (3★, Todd, 2025-03-11, 9703bd); "the first bottle I received had a more thin/water-like consistency and the second bottle is more thick" (5★, cd7b05); "it used to be kinda runny but is thicker now" (5★, 27da19); "the earlier version was very liquid, this one has a better more…" (5★, e98b7a); em clima frio: "the pump clogs frequently as it solidifies in there between uses" (4★, Shelby, 9db723); "I have to heat it up to use it" (4★, Jules S., 2025-12-16, 949bdf) — idem
- Atendimento/assinatura: "I tried to pause subscription because I have plenty and not easy process. Had to remove payment to get to stop. Emailed customer support and no response." (3★, Mike Falciglia, 2025-06-17, 505ebf); "I believe you should reach out to your clients before you just auto ship. It will take me 5 years to have what i have" (4★, Murphy, 2025-09-30, 3d41b5); "Wish I would've read the details better to know that a black Friday discount left it un-returnable. It's expensive to just try." (2★, Kimberly, 2025-12-15, 93ce76) — idem
- Refil: "It's just about impossible to get the body wash into the bottle with what's provided, have probably lost a bottle's worth just trying to refill it." (4★, Jake Hylton, 2026-06-22, dd6c90, Gallon Refill) — [Gallon Refill page](https://localrevival.co/products/tallow-honey-body-wash-1-gallon-refill-pouch); "Love the product but the lid to the gallon refill is much larger than the bottle opening." (2★, Jodye Sharp, 2026-06-15, 36cb9d) — idem
- Aroma × variantes: High Sierra "very heavy on the peppermint scent which I wasn't a fan of" (3★, Lindsey, 2025-08-01, 206e9f); Almond Vanilla "leans more sweet, almost like a dessert" (5★ Jennifer, 6a86d2; 3★ h.k. 2026-01-27: "its mire of a cherry almond than almond vanila. my bf says it smells like amaretto… its SO STRONG", c01c71); Eucalyptus Mint "the smell is awful" (1★, anon, 2026-05-11, 65356f; produto 8oz) — idem
- Transparência: "I got unscented before finding out unscented is a scent and is not the same as 'fragrance free'. They do not disclose what ingredients are in the unscented scent… I bought this soap from an Instagram ad saying plastic caps lead to leeching and they do it differently with a glass bottle. Yeah, ok, so then they ship it with a literal plastic cap and a plastic pump." (3★, Ani, 2026-05-31, bc513e) — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- Ranking de aromas por menções e % ≤4★ (leitura por palavra-chave): Almond Vanilla 149 menções (média 4,93; 5% ≤4★), Unscented 84 (4,87; 7%), Lavender 49 (4,86; 8%), Local Revival 43 (4,91; 9%), Wild Coast 39 (4,92; 8%), High Sierra 36 (4,83; 14%), Cedarwood 34 (4,91; 9%), Tobacco Vanilla 33 (4,82; 12%), Blue Citrus 19 (4,89; 11%), Eucalyptus Mint 15 (4,80; 20%) — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json)

### Inferences
- Perfil de "queixa": ~2,6% de insatisfação (≤3★); as razões nas notas baixas são metade produto (pele seca 27%, cheiro 23%, pouca espuma 11%) e metade logística/design (bomba 20%, vidro 12%, preço 7%, envio/assinatura).
- Cheiro é ao mesmo tempo o maior elogio (30,7% elogiam) e a maior fonte de 4★ ("love the product, not this scent": ~36 casos em 4★). A empresa vende sem possibilidade de cheirar antes (3 reviews pedem amostra/cheiro prévio: b17031, 609783, 8bb662 "Wish didn't have to spend so much in order to decide if a fragrance is worth it").
- Pouca espuma e consistência fina são tratados pelos próprios reviewers como "normal para produto natural" ("I think it's just because it's more natural", c55278) — expectativa de espuma de sabonete convencional é um ponto de atrito, mas raramente derruba a nota abaixo de 4★.
- "Não resseca/ não precisa de loção" (alívio de pele seca) aparece em ~7% e é o benefício funcional mais forte; em contrapartida 22 das 81 notas baixas dizem o oposto (pele seca) — ou seja, o efeito é heterogêneo (vários mencionam água dura, pele muito seca, 66+ anos).

### Gaps
- Contagens por regex têm imprecisão: amostra de 14 hits por tema em 6 temas indicou ~85-95% de acerto nos positivos; categorias negativas em 5★ (cheiro, espuma, vidro, secura) **não foram validadas manualmente** (regex indicou 109 / 88 / 69 hits em 5★, mas a maioria são menções neutras/positivas, p.ex. "doesn't dry out my skin"). Por isso a tabela de críticas só dá número firme para 1-4★ e para bomba/preço/envio em 5★.
- Não há variável "aroma comprado" na review (o widget não expõe a variante); a atribuição de crítica a aroma depende do texto.
- Não tentei o endpoint JSON `api/v1/reviews` com token (requer chave privada); tudo veio do widget público.

---

## 2. Reviews que comparam com sabonetes comuns (antes/depois, "switched from…")

### Takeaway
~5,4% das reviews usam linguagem de troca/comparação; "never go back" aparece em ~2%. Marcas citadas: Dove (7), Dr. Bronner's (6), Native (2, uma delas 1★), Bath & Body Works (3), Primally Pure (1), CeraVe/Eucerin/Aquaphor (2), Lush (1), sabonetes de tallow de outras marcas/Etsy e castile/goat milk. O padrão é "Dove/Dr. Bronner's/store-bought" → resseca; Local Revival → não resseca, sem resíduo.

### Cited Findings
Contagens (dump, N=3.107; regex): "store-bought/drugstore/Target/Walmart/big box/commercial/conventional/regular soap|wash" 54 (1,7%); "never go back/never use another" 66 (2,1%); bar soap (qualquer) 23; tallow de outra marca/bar de tallow/Etsy 28; castile/"Kastle" 8; goat milk 3; Dove 7; Dr. Bronner's 6; Native 2; Bath & Body Works 3 — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json). Verbatims (todas em [T&H page](https://localrevival.co/products/tallow-honey-bodywash) salvo indicado):

*Dove / marcas de supermercado*
- "This is the first time in my 60 years of using dove soap that I switched to another soap. I will never buy the bars of dove again and will continue to use the tallow and honey body wash. Works beautifully and leaves my skin soft and not dry!" — 5★, Gibby's Mom, não verificada, 2025-07-14 (9228dc)
- "I used dove soap for 50 years and finally switched to local revival body wash! The difference is night and day! My skin has never been this soft." — 5★, Colleen, não verificada, 2025-07-17 (320d3f); outra review "Colleen", verificada, 2025-08-01: "Ive used Dove soap all my life and this is so much better. I wish I had known how toxic our soap was many years ago!" (9fa20d)
- "I have dry skin, now I know why!!! Used Dove, you know, store brands that claim greatness. Yeahhh. No. I love this bodywash !!!" — 5★, Tracy Boudreau, 2025-04-25 (6f9c2f)
- "Dropped my Dove sensitive skin soap for this and haven't looked back!" — 5★, Brenda Franklin, 2026-02-23 (a8727a)
- "He used to buy Dove brand because it was pretty moisturising and he doesn't use lotion. Well guess what, he's hooked on Local Revival now! … It doesn't dry out his skin and smells good." — 5★, Anya, não verificada, 2026-01-03 (b59d1d)
- "My boyfriend was very firm in not using anything other than "his soap" which is Dove bar soap, but I made him try it finally and now this is his go to." — 5★, Crystal R., 2026-04-23 (d23a5c)
- "I tried a bunch, without any improvement, and was both annoyed and felt like I was wasting my money when nothing really worked better than my cheap stuff from the grocery store or target. … I've now made several repeat purchases, … my skin is less dry, and the rash went away. So, I gave it four stars cause it's expensive, but it works really well." — 4★, Phil, 2025-05-31 (7a72ed)
- "The ones sold at Target/Walmart give me a headache from the fragrance now. … I don't need to use lotion after a shower anymore, and it takes longer for me to have BO after using this body wash regularly compared to the commercial stuff." — 5★, Megan, 2026-03-29 (42534f)

*Genéricas "store-bought / conventional / drugstore"*
- "I'll never go back to store bought garbage." — 5★, Roxanne M., 2025-03-31 (a7fb3c)
- "I'm never going back to drug store body wash." — 5★, Dore Charbonneau, 2024-11-30 (641da2)
- "It's only been two days but I can tell this is a better product than any store bought body wash." — 5★, Ruth A, não verificada, 2025-04-05 (e12bf5)
- "There is no comparison to store bought or any other 'brand', the difference was amazing and noticeable from the first shower." — 5★, Cindy, 2026-03-12 (694f0d)
- "100% better than any commercial product on the market" — 5★, chris k, 2025-05-18 (2e2e50)
- "Doesnt leave that slick feeling you get with other body wash under soft water." — 5★, Nicholas, 2026-02-23 (569e2d)
- "My skin doesn't feel like it has a coating on it like it did with other body washes." — 5★, anon, 2026-09-17 (c45fee)
- "After switching to this, other body washes honestly feel harsh and artificial by comparison." — 5★, Brandon Hebert, 2026-05-21 (59364c)
- "This body wash has ruined all other body washes for me. It isn't the sad, diluted mystery-liquid you get at big-box stores." — 5★, Zoe, 2025-12-06 (442e9b)
- "Off the shelf drugstore products don't work when it comes to restoring the skin barrier, especially body washes which increase irritation and make the skin worse over time." — 5★, Dawn H, não verificada, 2026-09-06 (41481e)
- "My partner's wash feels slimy compared to this one, which feels more velvety." — 5★, Dry Girl, 2025-08-11 (1ace8e)
- "It has a higher price than store bought brands; however, it takes much less product than store bought." — 5★, Kelly, 2026-03-05 (bb39c8); "I was a little apprehensive about the price point, but when you take into consideration all of the chemicals in store bought body wash - it's just worth it." — 5★, Lisa, 2025-07-01 (37fcde)
- "Don't expect it to lather like a regular body wash. … So don't expect a crazy lather that's similar to your drugstore body wash." — 5★, Hunter R, 2024-11-08 (ca0693)
- "My only issue is that it doesn't suds up like toxic bodywashes, but im pretty sure no natural brand will suds up well." — 4★, Lauren Eckard, 2025-01-02 (33b951)

*Marcas "naturais"/de nicho*
- Dr. Bronner's: "Cleans way better than Dr. Bronners and smells amazing! Also very mild and doesn't dry out my skin!" (5★, Julie Sturgeon, 2025-08-08, 668107); "I used to use dr. Bronner but it always left my skin super dry. This soap doesn't dry out my skin" (5★, Alena, 2025-08-09, b589a6); "We were using Dr bronners for years and I've loved this switch so far." (5★, Beth P, 2026-02-01, 1b7295); "I bought it to replace our regular use of Dr. Bronners. I was afraid that it would not lather, but I was wrong." (5★, Jessica Spencer, 2026-07-13, 2da93e)
- Castile: "I've tried several natural soaps and always come back to Castile for clean ingredients but it's is soooo drying." (5★, Geena, 2025-12-05, 9302e5); contra-exemplo: "the body wash is similar to a Castile soap which is too drying for my sensitive skin" (5★, Emma Dill, 2026-01-19, f7518a); "feels similar to Castile soap upon rinsing. I was hoping it'd leave my skin a little more coated with tallow." (4★, LongevityLady, 2026-03-02, 3bbb07)
- Native: "I used to use native dues to decent ingredients, but ever since they were bought out by P&G they changed alot of their ingredients." (5★, Dylan, 2025-08-10, 239e7e; afirmação do reviewer, não verificada por esta pesquisa) vs "I could only use it for about 4 days before i had to switch back to native due to my skin got so dry and itchy" (1★, Roy, 2025-12-15, 2286a5)
- Bath & Body Works: "compared to bath and body works which I used for decades, it's a clean and refreshing feeling" (5★, Dino Delacruz, 2025-10-23, 67f6bb); "I gave up the bath and body works and will never go back!" (5★, Joan Palmer, 2026-07-25, 1da688); "It doesn't suds up like my old bath and body works, but it's really not that big a difference in suds department" (5★, Body Wash lover, não verificada, 2025-12-09, c55278)
- Primally Pure: "I typically use Primally Pure body wash, but decided to give this one a try, largely because it is more affordable!" (3★, Ella McGarvey, 2026-04-23, f14ba0)
- Cremes: "My skin has gone from needing heavy creams like Aquaphor and CeraVe daily for rough, bumpy skin… to now only using them once a week" (5★, Linda, 2026-04-26, a74fad); "My skin feels hydrated better than ever without applying Eucerin after each shower" (5★, Mom of five kids, 2026-04-10, 9ced9e)
- Lush: "Gives me Lush vibes, also does a nice job lathering and the bottle is beautiful." (5★, Mary Vassallo, 2026-09-16, b0cdc2)
- Tallow bar / outros tallow líquidos: "I actually prefer this to tallow-based bar soaps too." (5★, Emily K, 2025-03-17, 28d41f); "I used to use tallow bar soap but it made my skin dry." (5★, Addison, 2025-09-24, 0a3ffb); "I had been looking for a tallow body wash for awhile and bought a few from Etsy. They were super watery and didn't moisturize my skin." (5★, Lin, 2025-11-03, ecd1ea); "I got an ad for a much cheaper product ($14/believe cheaper) so I tried it. Well it took almost 18 pumps to shower (yes I counted) because that product was SO watered down … Local Revival is much more viscous" (5★, Todd Hitchner, 2025-11-10, aececa); "I have purchased other tallow products from a different company & it smelled so bad I couldn't use it." (5★, anon, 2026-09-28, db5e99)
- Goat milk: "For the last six months I used goat milk bar soaps. These were very drying, though. This product not only lathers well, but moisturizes and smells great." (5★, Jodi Nishida, "metabolic health doctor", 2024-11-14, 825841)

### Inferences
- A narrativa comparativa dominante é de "sabonete convencional resseca → LR não resseca"; Dove é a marca mais citada nominalmente, em reviews de consumidores de longa data (50-60 anos de uso) — vocabulário útil para o pesquisador: "toxic", "garbage", "drugstore", "never go back".
- Há reviews que usam Dr. Bronner's/Castile como referência de "limpo porém seco", e LR aparece posicionado como "castile menos seco"; mas duas reviews (f7518a, 3bbb07) dizem que LR também se parece com castile em secura/pouca lubrificação — comparação não uniforme.
- A comparação de preço é de "valor": 2 reviews dizem rende mais (menos produto por uso) que marcas de loja, enquanto outras (aececa, ecd1ea) contrastam com tallow líquido mais barato e "aguado".

### Gaps
- Contagem de "comparação" é aproximada (regex amplo 5,4% vs estrito 1,7-2,1%); não classifiquei manualmente as 293 reviews com palavras-gatilho; os verbatims acima foram selecionados por leitura, não são amostra aleatória.
- Alegações sobre Native/P&G são do reviewer; não verifiquei.

---

## 3. Claims de marketing do site, ingredientes e como as reviews os refletem

### Takeaway
O site afirma "100% Organic/Natural", "USA Sourced", "Small Batch Handmade in California", "free from harsh detergents, filler ingredients and artificial fragrances", vidro reutilizável. As reviews ecoam principalmente "ingredientes limpos/sem toxinas" (6,6%), "natural" (7,8%) e vidro (9,6%); "USA/California/local" aparece em ~0,8% (busca estrita). O próprio site faz ressalvas que contradizem parcialmente a manchete ("technically can't call it organic", "100% US sourced in the next few years") e há inconsistências internas na lista de ingredientes.

### Cited Findings
**Claims (texto literal do site):**
- Home: "Our signature body wash, crafted from the finest ingredients like Olive Oil, Aloe Vera, Shea Butter, and Jojoba Oil. Unlike synthetic store bought body washes our formula is free from harsh detergents, filler ingredients and artificial fragrances." Selos: "100% Organic Ingredients / 100% Natural Ingredients / USA Sourced Ingredients / Small Batch Handmade in California". Manchetes: "From Farm to Shower. No Synthetic Detergents. No Fragrance. No Foaming Agents No Seed Oil Fillers. Only 100% Organic Ingredients. Made in the USA." e "THE CLEANEST BODY WASH IN THE WORLD" — [localrevival.co](https://localrevival.co/)
- Home FAQ: "all our products are made with 100% natural and organic certified ingredients. We do use lye in our body wash though so we technically can't call it 'organic'." / "We manufacture in house. … Each product is made in small batches." / "The glass bottles are reusable. The bodywash is biodegradable." / "Are your products suitable for sensitive skin? Yep." / "Will this heal, treat or cure my skin conditions? … not intended to diagnose, treat, cure, or prevent any disease or medical condition." — [localrevival.co](https://localrevival.co/)
- Página T&H: "We partner exclusively with small local farms and ranches to bring you 100% organic ingredients. We source everything we can from the US and it's all made right here in Southern California" … "We'd like our products to be 100% US sourced in the next few years." / "Even 'natural' alternatives are filled with cheap filler oils like sunflower and palm." / "We only use reusable glass bottles. Yeah, it may be a safety hazard but we'll take that risk over toxic plastic any day." / "The bathroom is no place for synthetic ingredients or fragrances" — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- FAQs T&H: shelf life "within 6 months of receiving"; "Does it foam? It definitely foams up but not as much as the store bought garbage."; shampoo "might strip some hair types"; "PH is between 8.05 and 8.5"; face "Yep"; eczema → só disclaimer; refund "no questions asked"; footer "Any reviews reflect individual customer experiences and opinions. Local Revival does not adopt, endorse, or make any therapeutic, medical, or health claims based on customer feedback." — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- Políticas: reembolso 100% "no questions asked", mas "We reserve the right to not accept refunds/returns if your original order is >30 days old"; envio 1-2 dias de processamento, 3-6 dias economy / 1-3 priority, sem garantia — [refund policy](https://localrevival.co/policies/refund-policy), [shipping policy](https://localrevival.co/policies/shipping-policy)

**Ingredientes listados (T&H 16oz, gallon e 8oz; "Full Ingredients"):** Cold Pressed Organic Olive Oil from California; Organic Grass-Fed/Finished Tallow from Wyoming cows; Cold Pressed Organic Jojoba Oil from Arizona; Raw Organic Honey from Utah; Organic Aloe Vera from Texas; Organic Cold Pressed Castor Oil; Unrefined Organic Shea Butter; Organic Coconut Oil; Organic Coconut Glycerine; Steam Distilled Organic Essential Oils (no essential oils in the unscented); Lye (none remains after saponification). Nota na mesma página: "*Almond Vanilla and Tobacco Vanilla use blends of vanillin, absolutes and CO2 extraction" e "Scented with Vanilla + Almond Extract"; "All Blends are made with 100% Organic Steam Distilled Essential Oils". Segunda lista na mesma página (seção "What's in it") grafa "Grass-Fed/Finished Tallow from Wyoming cows" sem "Organic" — [T&H page](https://localrevival.co/products/tallow-honey-bodywash), [Almond Vanilla 8oz](https://localrevival.co/products/almond-vanilla-8oz)
- Aromas (descrição do site): Local Revival (citrus + vanilla), Wild Coast, High Sierra (mint-forward), Cedarwood (Atlas/Texas cedar, black spruce, bergamot), Unscented ("Slightly soapy smell"), Almond Vanilla, Tobacco Vanilla, Blue Citrus (lavender + bergamot, "a bit cologny"), Lavender Bloom, Eucalyptus Mint, Barbershop, Slow Burn (patchouli, bergamot, ginger, clary sage), Toasted Sugar (cedar, cinnamon, lavender), Floral Haze, Citrus Woods — [T&H page](https://localrevival.co/products/tallow-honey-bodywash)
- Olive & Aloe: "Reverse Osmosis Water, Saponified Extra Virgin Olive Oil*, Babassu Oil*, Castor Oil*, Aloe Vera*, Shea Butter*, Jojoba Oil*, Prickly Pear Oil*, Avocado Oil*, Glycerine* (*Organic)"; "No coconut, no added fragrance… Packaged in glass"; "Yes every single one of them [organic]. We just can't put organic on the label because we haven't been USDA or Oregon Tilth certified yet."; "manufacture it in the same facility as our original though so if you're super sensitive we recommend a patch test"; "It'll lather more than castile soap but not as much as most store bought stuff." — [Olive & Aloe page](https://localrevival.co/products/olive-aloe-body-wash)
- Silicone Sleeve: "Protect your re-usable 16oz glass bottle from slips and drops. These 100% silicone sleeves are micro-plastic free…" — [Silicone Sleeve](https://localrevival.co/products/silicone-sleeve-16oz)

**Como as reviews refletem os claims (contagens no dump, N=3.107):**
- "toxic/toxin/chemical/harmful" 205 (6,6%; média 4,93); "clean ingredients/clean product/non-toxic" 205 (6,6%); "natural" 241 (7,8%); "ingredient(s)" 391 (12,6%); "organic" 46 (1,5%); microplásticos/hormônios/ftalatos/parabenos 14 (0,5%); "seed oil" 1; "detergent/sulfate/surfactant" 6; "grass-fed/finished/Wyoming" 3 — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json). Ex.: "I finally found a body wash that doesn't dry me out or give me a rash" (46caf8); "I'm happy that the ingredients won't mess with my hormones!" (61491c).
- USA/California/local/small batch/handmade/"sourced" (busca estrita) 24 (0,8%); busca ampla "honest/small business/local/support" 353 (11,4%, inclui falsos positivos como o nome da marca) — dump. Ex.: "I love the story and how clean they make it" (bb4950); "Support small business because big business doesn't gaf about you" (aececa).
- Vidro/plástico/microplástico/refil/reutilizável 298 (9,6%; média 4,79, a mais baixa entre os claims-ecos por causa das críticas ao vidro); "I enjoy knowing that the bodywash comes in a glass recyclable bottle instead of a microplastic filled container." (07c6cd, 31ceb0 quase idêntica).
- Crenças contra-claim: "I believe there are too much plant surfactants in it." (4★, Dylan crow, 2025-01-15, f3b0e8) — [T&H page](https://localrevival.co/products/tallow-honey-bodywash); "I'm curious about the lye in the soap. Is it dangerous" (4★, Jody Levinson, 2026-08-23, db2958); "when I saw the word lye I was a bit nervous. I'm trusting you that it is completely removed" (5★, b1fcf1); "wonder how accurate the labeling is b/c it doesn't include 'tobacco' or 'vanilla' in the ingredient list" (4★, B. Jenuwine, 2026-02-13, b1b513); "I do worry when honey and tallow are mixed - it needs an emulsifier." (4★, Rebecca L, 2026-02-16, 594a6a); "It smells like something halfway between thieves oil and biting into a red hot candy. The cinnamon oils gave me a bit of a reaction on my skin" (5★, Nicole, 2026-10-08, d87d3f; sazonal com canela).
- Claim "not as much foam": reviews confirmam — ver seção 1 (≈33 reclamações ≤4★).
- Claims de saúde: o site proíbe alegações; reviews de eczema/psoríase/hives/Sjögren/mastócitos etc. são espontâneas (89 "eczema"; 19 "psoriasis"); o site não respondeu a nenhuma (nenhuma resposta do dono detectada).

### Inferences
- A manchete "100% organic/USA/California" não é repetida pelos reviewers em proporção relevante (organic 1,5%, USA/California 0,8%); o que repercute é "sem toxinas/químicos/fragrância sintética" e "vidro".
- O próprio site hedge: "technically can't call it organic", "We source everything we can from the US", "100% US sourced in the next few years" e (Olive & Aloe) "haven't been USDA or Oregon Tilth certified"; e as manchetes "No Fragrance" coexistem com 14 aromas feitos com óleos essenciais/vanilina/absolutos. Há risco de percepção de incoerência (review Ani, 3★, bc513e: "unscented is a scent… not the same as 'fragrance free'").
- Tallow listado como "Organic" em uma lista e sem "Organic" em outra na mesma página — ponto de atenção para claim de "100% organic".

### Gaps
- Não verifiquei os claims contra rótulos físicos, certificações ou análise laboratorial; apenas o texto do site.
- Páginas 8oz não têm lista de ingredientes própria separada da 16oz na extração (usam o mesmo conteúdo); "Sampler Pack" e "Five Scent Sample Set" não têm descrição de ingredientes lida.
- A seção "Questions?" da home foi lida; não há páginas /about e /faq (404).

---

## 4. Fotos/vídeos, "verified buyer", respostas do dono

### Takeaway
86,5% das reviews trazem selo verified buyer; 45 reviews (1,4%) têm foto (todas 5★), nenhuma tem vídeo, nenhuma tem resposta do dono nas reviews extraídas.

### Cited Findings
- Verified buyer (`data-verified-buyer`): 2.689 true (86,5%) / 418 false (13,5%). Todas as 418 não verificadas vêm de "review_collected_from_store_visitor"; 414 estão no T&H 16oz e 4 no Gallon Refill; notas: 391×5★, 25×4★, 2×3★. Média dos verificados 4,865 vs não verificados 4,931 — [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json)
- Origem da coleta (badge de transparência Judge.me): 1.452 "collected from store visitor" (46,7%; 1.034 verificadas + 418 não), 1.226 "collected via store invitation" (39,5%; e-mail de pedido), 429 "written in shop app" (13,8%) — dump. Todas as `via_store_invitation` e `shop_app` são verificadas.
- Fotos: 45 reviews com foto (37 no T&H 16oz — o `sort_by=with_pictures` do widget retorna total=37 —, 3 Gallon Refill, 2 Silicone Sleeve, 3 do grupo 8oz); 43 têm 1 foto, 1 tem 2 fotos (0abf9e) e 1 tem 14 fotos (Shelly Trigg, 2025-10-28, e90fe8); URLs em `review-images.judgeme.com/local-revival/…` — [T&H widget com fotos](https://judge.me/reviews/reviews_for_widget?url=localrevival.co&shop_domain=localrevival.co&platform=shopify&per_page=30&page=1&product_id=8950214328547&sort_by=with_pictures). Notas das reviews com foto: 45/45 = 5★.
- Vídeos: 0; resposta do dono/loja: 0 (parser + checagem independente em 382 reviews, incluindo todas as 251 de 1-4★ do T&H 16oz, sem nenhum bloco `jdgm-rev__reply` com conteúdo); localização do reviewer: vazia em todas — dump.
- Votos "útil": 8 reviews com 1 voto, o resto 0.
- Palavras do dono presentes só indiretamente nas reviews: "the owner responded quickly and like a friend" (5★, Natasha Falkowski, 2026-03-27, 0b3588), "exchanged emails with one of their managers who was kind enough to share the process" (5★, Rose, 2025-11-14, 0fcbb1), "the person I corresponded with was prompt to resolve my issue without asking me countless questions" (5★, Olivia Boodry, 2025-03-12, 1bf275); troca/reenvio de item quebrado, bomba defeituosa ou pedido perdido: ≥14 reviews 4-5★ (regex, limite inferior) relatam resolução (ex.: "One of my bottles came with a broken pump, probably from shipping. But I emailed them and they sent one the same day.", 5★, Heath, 2025-09-19, bb4950).
- Reviews com ID 5★ que mencionam "ad/Instagram/Facebook": 90 (2,9%); ex.: "I got an ad for this stuff and on a whim" (7a72ed); "It began as an instagram ad that once again lured me in" (b4b673) — dump.

### Inferences
- Judge.me exibe o selo apenas para compras vinculadas; os 13,5% não verificados são concentrados no T&H 16oz e quase todos 5★ — vale tratá-los em análise de sensibilidade.
- Sem resposta pública do dono, o atendimento aparece só pelo relato dos clientes (positivo, 90 menções).

### Gaps
- Não baixei as imagens (não analisadas); só contei URLs.
- "Verified" do Judge.me não equivale necessariamente a compra confirmada no Shopify (depende do canal de coleta); não há como auditar.

---

## 5. Sinais verificáveis de reviews suspeitas (sem especular)

### Takeaway
Os dados mostram: (a) 5 reviews de spam idênticas (propaganda de "herbal Dr Enosetale") do autor "Ken", todas 5★ e não verificadas, que contam para a nota; (b) muitos textos curtos/genéricos de 5★; (c) autores com várias reviews no mesmo dia por produto-item de pedidos; (d) 1 par de reviews quase idênticas de dois autores com nomes parecidos. Não há concentração anômala de datas nem texto duplicado em larga escala.

### Cited Findings
- Spam: 5 reviews 5★, autor "Ken", `verified_buyer=false`, título "Good product" (4×) / "Effective" (1×), mesmo texto: "Don't die in silent there is natural herbs cure for all kinds of diseases. I used natural herbs medicine made by a powerful herbal Dr Enosetale to get rid of my mum diabetes type 2 permanently, he can also help you too. Remember that a problem shared is a pr…" Datas: 2026-03-20 (a65c28), 2026-05-24 (687798), 2026-06-22 (adeaf3), 2026-07-26 (a8d11b), 2026-08-22 (027c8c); todas listadas no T&H 16oz (ainda contadas nas 2.862) — [T&H page](https://localrevival.co/products/tallow-honey-bodywash). Sem os 5 spam: média T&H 4,8708 vs 4,8710 com eles (impacto desprezível) — cálculo no [dump](file:///home/user/l-revival/research_notes/Pain%20points%20sabonetes%20Local%20Revival/localrevival_reviews_raw.json).
- Textos duplicados: texto idêntico "love it" ×27, "amazing" ×10, "love" ×9, "great product" ×7, "absolutely love this body wash" ×6; títulos mais frequentes: "Love it!" 53, "Body wash" 42, "Great product" 41; 502 reviews sem título (16,2%); 338 "Anonymous" (10,9%); 299 reviews com ≤5 palavras (9,6%), 159 com ≤3 palavras; 612 reviews 5★ com ≤10 palavras — dump.
- Quase-duplicata: "Jennifer" (verificada, 2026-04-17, 6a86d2, 5★) e "Jennifer Gossett" (não verificada, 2026-04-14, 83ba3c, 5★, título "Almond Vanilla - Local Revival - Silicone Sleeve") — mesma estrutura e frases (pedido 4/4–4/10, sleeve, Almond Vanilla "more like a dessert", "loofah helps a lot", "three pumps", "four days", "no longer ashy", "pricey, but worth it"); a versão de 04-14 é mais longa — [T&H page](https://localrevival.co/products/tallow-honey-bodywash). Outro par com similaridade 0,57: "jennifer mccausland" (verificada, 2025-10-18) e "Anonymous" (não verificada, 2025-10-19) sobre dificuldade de encaixar a tampa/pump (37023a/047ec3) — dump.
- Concentração por dia: mediana 4 reviews/dia em 643 dias com review; picos 2026-09-08 (18; 15 autores; 17 verificadas; 13 T&H + 2 Olive & Aloe + 3 sleeve), 2026-05-14 (17), 2025-12-29 (16), 2026-02-10 (15), 2026-01-09 (15). Mix de badges nos picos = invitation/visitor/app, ou seja, não parecem lote de uma única fonte. Volume mensal cresce de 11-30/mês (até 2025-03) para 69 em 2025-04 e 114-136/mês de 2025-05 a 2025-09, 129-234/mês depois (máx. 234 em 2026-09) — dump. 
- Reviews do mesmo autor em vários produtos no mesmo dia (provável resultado de pedidos de review por item do Judge.me, pois a mesma pessoa avalia itens diferentes): "Kimberly Scott" 4 reviews em 4 aromas 8oz em 2026-10-05 (via_store_invitation, verificadas); "K" 4 reviews em 2026-05-14 via shop app (Eucalyptus Mint, Almond Vanilla, Tobacco Vanilla, Lavender Bloom); "Alexandra Ellerby" 3 reviews idênticas ("Absolutely love this product and its scent…") em 3 aromas 8oz em 2026-08-31 — dump. 384 nomes de autor (não "Anonymous") aparecem em ≥2 reviews (nome pode coincidir entre pessoas diferentes: "Susan" 13, "Michelle" 13, "Heather" 13).
- Mesma pessoa com selo diferente: "Rosi G" tem 3 reviews com foto — 2025-07-12 (não verificada), 2025-10-26 (verificada), 2026-05-15 (verificada); "Colleen" aparece com relato semelhante (Dove) 2025-07-17 (não verificada) e 2025-08-01 (verificada) — dump (não afirmo identidade, só observo o padrão).
- Anomalia nota × texto: review de 1★ com texto positivo "Product is as advertised, very pleased keep up the good work" (Tom McAllister, 2026-08-31, 0b368a); review 3★ "I love it! Get unscented!" (Jonathan Archer, 2026-01-17, 92b6bc); reviews 3★ sem uso ("Too early to give any other rating", 9246e7) — T&H page.
- Menção a incentivo: 15 reviews (0,5%) citam desconto/free/gift/giveaway/Black Friday (média 4,53, a menor entre os temas): maioria é desconto como compra; "Wish I would've read the details better to know that a black Friday discount left it un-returnable" (93ce76). Não encontrei texto do tipo "recebi grátis em troca de review" — dump.
- Reviews não-inglês/spam adicional: busca por URLs, "whatsapp", "telegram", "http", "www" retornou só o spam do Ken (e falsos positivos de "Dr. Bronner's") — dump.

### Inferences
- Os dados verificáveis não sustentam alegar fraude generalizada; o que se prova é (1) spam de um autor e (2) quase-duplicata/duplicidade de pessoas em itens de bundle, e (3) alto volume de reviews muito curtas 5★ (comum em coleta por e-mail).
- O crescimento de volume a partir de 2025-04/05 coincide com menção a anúncios no Facebook/Instagram em reviews, mas isso não prova causalidade.

### Gaps
- O widget Judge.me não expõe e-mail/IP/pedido; não é possível testar compra vs. review, nem identificar o mesmo autor com certeza.
- Não calculei similaridade entre todas as reviews curtas (<25 palavras).
- Não há dados de reviews removidas/ocultas (o widget só mostra publicadas; 25 T&H e 2 refill não retornaram).
