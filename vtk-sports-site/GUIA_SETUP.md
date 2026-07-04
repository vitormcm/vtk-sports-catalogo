# Guia de Setup — Catálogo VTK Sports automático

Este pacote transforma o catálogo em uma página viva: o n8n verifica o estoque
da ifutz uma vez por dia e atualiza o site sozinho.

## Parte 1 — Criar o site no GitHub Pages (10 minutos)

1. Crie uma conta gratuita em https://github.com/signup (se ainda não tiver)
2. Clique em **New repository** (botão verde, ou https://github.com/new)
   - Nome sugerido: `vtk-sports-catalogo`
   - Marque **Public**
   - Clique em **Create repository**
3. Nesse repositório novo, clique em **Add file → Upload files**
   - Arraste os arquivos: `index.html`, `dados.json`, `logo.png`, e a pasta `images/`
     (com os 3 arquivos: `corinthians-2627-jogador.jpg`, `santos-2627-torcedor.jpg`,
     `flamengo-2627-jogador.jpg`)
   - Clique em **Commit changes**
4. Vá em **Settings → Pages** (menu lateral)
   - Em "Branch", selecione `main` e pasta `/ (root)` → **Save**
   - Aguarde ~1 minuto. O GitHub vai te dar uma URL tipo:
     `https://SEU-USUARIO.github.io/vtk-sports-catalogo/`
   - Essa é a URL definitiva do catálogo — pode compartilhar com quem quiser

## Parte 2 — Criar o token de acesso (Personal Access Token)

Isso é o que permite o n8n editar o arquivo `dados.json` sozinho.

1. Acesse https://github.com/settings/tokens/new
2. Em "Note", escreva `n8n-vtk-sports`
3. Em "Expiration", escolha **No expiration** (ou 1 ano)
4. Marque a permissão **repo** (a caixa principal, isso marca todas as sub-opções)
5. Clique em **Generate token**
6. **Copie o token agora** (começa com `ghp_...`) — ele só aparece uma vez

## Parte 3 — Conectar no n8n

Já criei o workflow **"VTK Sports — Atualização diária de estoque"** no seu n8n.
Falta só:

1. Abra o workflow no n8n
2. No node **"Atualizar dados.json no GitHub"**:
   - Clique em Credentials → Create New → cole o token do GitHub (tipo "GitHub API", Access Token)
   - Preencha **Repository Owner** com seu usuário do GitHub
   - Preencha **Repository Name** com `vtk-sports-catalogo` (ou o nome que você usou)
3. Clique em **Save** e depois em **Active** (canto superior direito) para ligar o workflow

Pronto. A partir daí, uma vez por dia o n8n:
- verifica as 5 páginas de produtos da ifutz.com.br
- recalcula quais camisas estão em estoque, com quais tamanhos
- reescreve o `dados.json` no GitHub
- o site (que já está no ar) mostra os dados atualizados na próxima vez que alguém abrir

## Observações importantes

- **As 3 camisas novas** (Corinthians, Santos e Flamengo 26/27) foram adicionadas manualmente
  porque não existem na ifutz — elas **não são verificadas automaticamente**. Se o estoque delas
  mudar, edite o `dados.json` direto no GitHub (é só abrir o arquivo lá e trocar o número do
  campo `"stock"` do item correspondente).
- Se a ifutz mudar a estrutura do site (layout novo, etc.), o workflow pode parar de encontrar
  os produtos corretamente — nesse caso me chame de novo para eu ajustar o código do node de
  extração.
- Quer testar sem esperar 24h? No n8n, abra o workflow e clique em **"Test workflow"** para rodar
  na hora.
