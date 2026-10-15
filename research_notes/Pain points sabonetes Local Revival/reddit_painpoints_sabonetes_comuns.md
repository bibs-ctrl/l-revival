# Pain points de sabonetes comuns (body wash e barra) no Reddit

STATUS: BLOQUEADO. Nenhum thread do Reddit pôde ser lido neste ambiente. Este arquivo NÃO contém citações, links de threads, contagens de upvotes ou datas, porque nada foi acessado. Nenhuma citação foi inventada. O banco de pain points (mínimo de 25 threads) NÃO foi construído.

## Quais reclamações se repetem mais sobre body washes e barras comuns (frequência relativa)

### Takeaway
Não foi possível quantificar a frequência: zero threads do Reddit foram lidos.

### Cited Findings
- Nenhum finding do Reddit. Todas as vias de acesso testadas falharam (ver Gaps).

### Inferences
- Nenhuma. Qualquer ranking de reclamações (pele seca, fragrância, sulfatos etc.) seria conhecimento prévio meu, não evidência do Reddit, e por isso foi omitido.

### Gaps
Vias tentadas (todas em 2026-10-15) e resultado:
- `curl` em old.reddit.com (search.json e HTML): HTTP 403, página "Blocked ... network policy".
- `curl` em www.reddit.com (`.json`, `top.json`, `.rss`) e oauth.reddit.com: 403, e 429 no `.rss`.
- `npx -y defuddle parse` em old.reddit.com e www.reddit.com: "No content could be extracted".
- WebFetch em reddit.com: "unable to fetch from www.reddit.com".
- WebSearch com `allowed_domains: reddit.com`: erro 400, domínio inacessível ao user agent.
- WebSearch sem filtro (4 queries, `site:reddit.com`, nomes de subs, Dove/eczema): nenhum resultado de reddit.com; só páginas de varejo e editoriais, que as restrições da tarefa excluem.
- Espelhos e arquivos: r.jina.ai (403, domínio bloqueado até 15:37 UTC de hoje), redlib.catsarch.com (429), arctic-shift (422, timeout), pullpush.io (rate limit, sem acesso de scraping), allorigins (522), Wayback (sem snapshots de r/eczema; CDX com connection reset).

## Quais ingredientes são mais citados como vilões e por quê

### Takeaway
Sem dados do Reddit.

### Cited Findings
- Nenhum.

### Inferences
- Nenhuma.

### Gaps
- Citações sobre SLS/SLES, fragrância, parabenos, triclosan, pH alcalino de sabão em barra: não coletadas.

## O que as pessoas pedem explicitamente ("I wish...", "looking for...", "recommendations")

### Takeaway
Sem dados do Reddit.

### Cited Findings
- Nenhum.

### Inferences
- Nenhuma.

### Gaps
- Pedidos de ingredientes simples, sem fragrância/sulfato, cheiro suave, preço justo, transparência e sustentabilidade: não coletados.
- Threads em português (r/brasil, SkincareBrazil): não coletados.

## Quais alternativas as pessoas recomendam e por quê

### Takeaway
Sem dados do Reddit.

### Cited Findings
- Nenhum.

### Inferences
- Nenhuma.

### Gaps
- Alternativas citadas (por exemplo, syndets, produtos para pele sensível) e motivos: não coletadas.

## Próximos passos sugeridos para destravar
- Exportar manualmente threads (texto copiado, links) ou rodar a coleta em ambiente com acesso ao Reddit (API oficial com credenciais OAuth, ou PRAW), e reenviar para análise.
- Alternativa: liberar o domínio reddit.com na allowlist da rede do ambiente e/ou fornecer credenciais de API do Reddit.
