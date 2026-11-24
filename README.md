# Painel de Coletas — Mercado Livre · RVD Saúde

Página única, estática. Não tem build, não tem dependência para instalar.

## Arquivos

- `index.html` — o painel inteiro (HTML, CSS, JS e logo embutidos)
- `vercel.json` — configuração do deploy (sem cache, para a lista atualizar na hora)

## Publicar na Vercel

### Opção 1 — arrastar a pasta (mais rápido, sem instalar nada)

1. Entre em https://vercel.com/new
2. Clique em **Deploy** → aba de upload e arraste esta pasta inteira
3. Pronto: a Vercel devolve a URL pública

### Opção 2 — pela linha de comando

```bash
npm i -g vercel          # instala o CLI (só na primeira vez)
cd painel-rvd            # entre na pasta deste projeto
vercel login             # faça login (só na primeira vez)
vercel                   # publica uma versão de teste (preview)
vercel --prod            # publica em produção
```

### Opção 3 — pelo GitHub (atualiza sozinho a cada envio)

```bash
cd painel-rvd
git init
git add .
git commit -m "Painel de coletas RVD"
git branch -M main
git remote add origin https://github.com/SUA-CONTA/painel-coletas-rvd.git
git push -u origin main
```

Depois, em https://vercel.com/new, importe esse repositório. A cada `git push`
a Vercel publica a versão nova automaticamente.

**Configuração do projeto na Vercel:** Framework Preset = **Other**,
Build Command = vazio, Output Directory = vazio (ou `.`).

## Como atualizar a lista de pedidos

Dentro do `index.html`, procure a linha que começa com:

```js
const DADOS = {"data":"24/11/2026","coletas":[ ...
```

É um JSON com esta forma:

```json
{
  "data": "24/11/2026",
  "coletas": [
    {
      "id": "c1",
      "nome": "1ª coleta",
      "hora": "09:00",
      "data": "24/11/2026",
      "pedidos": [
        { "p": "111276", "nf": "84809", "c": "Rodrigo Rafael Barata Maia", "s": "ok" }
      ]
    }
  ]
}
```

- `p` = pedido · `nf` = nota fiscal (deixe `""` quando não tiver)
- `c` = cliente · `s` = status: `"ok"`, `"sep"` (em separação), `"erro"` ou `""`
- `id` precisa ser único por coleta (`c1`, `c2`, `c3`…)

Trocou os dados, é só publicar de novo (`vercel --prod` ou `git push`).

## Observações

- As marcações ficam salvas no navegador de cada pessoa (`localStorage`),
  usando a data da lista como chave. Lista nova = conferência zerada.
- O botão **Gerar PDF para imprimir** usa a biblioteca jsPDF, carregada do
  cdnjs. Fora do Claude, ele baixa o arquivo direto pelo navegador.
- A página funciona offline depois de carregada, menos a geração do PDF,
  que precisa do script do cdnjs.
