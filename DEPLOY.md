# Deploy do trilhapt.com — GitHub + Netlify + Namecheap

Este processo fica configurado uma vez. Depois disso, qualquer alteração que
enviares para o GitHub (`git push`) atualiza o site sozinho, sem precisares
de fazer mais nada.

---

## 1. Criar o repositório no GitHub

1. Vai a [github.com/new](https://github.com/new)
2. Nome do repositório: `trilhapt.com` (ou o que preferires)
3. Deixa como **privado** ou **público** — tanto funciona com a Netlify
4. Não marques "Add a README" (já temos ficheiros para enviar)
5. Cria o repositório

No teu computador, dentro desta pasta (`trilhapt.com/`), corre:

```bash
git init
git add .
git commit -m "Primeira versão da landing page"
git branch -M main
git remote add origin https://github.com/O_TEU_USERNAME/trilhapt.com.git
git push -u origin main
```

(substitui `O_TEU_USERNAME` pelo teu utilizador do GitHub)

---

## 2. Ligar o repositório à Netlify

1. Vai a [app.netlify.com](https://app.netlify.com) e cria conta gratuita (podes usar login direto com GitHub — mais rápido)
2. **Add new site → Import an existing project**
3. Escolhe **GitHub** e autoriza o acesso ao repositório `trilhapt.com`
4. Configurações de build:
   - **Build command**: deixa em branco (não há build, é HTML puro)
   - **Publish directory**: `.`
5. **Deploy site**

Em menos de um minuto tens um URL do tipo `https://algo-aleatorio.netlify.app`
já no ar. A partir daqui, **qualquer `git push` para `main` faz redeploy automático** — é isto que resolve o "deploy automatizado" que pediste.

---

## 3. Ligar o domínio trilhapt.com na Netlify

1. No painel do site na Netlify: **Site configuration → Domain management → Add a domain**
2. Escreve `trilhapt.com` → **Verify** → **Add domain**
3. A Netlify vai mostrar-te os registos DNS exatos a configurar. Os valores
   típicos (confirma sempre os que a Netlify te mostrar no momento, porque
   podem mudar) são:

   | Tipo | Host | Valor |
   |------|------|-------|
   | A | `@` | `75.2.60.5` |
   | CNAME | `www` | `[o-nome-do-teu-site].netlify.app` |

---

## 4. Configurar o DNS na Namecheap

1. Entra em [namecheap.com](https://namecheap.com) → **Domain List** → **Manage** (no domínio `trilhapt.com`)
2. Vai ao separador **Advanced DNS**
3. **Apaga** os registos que a Namecheap põe por defeito quando compras o domínio (normalmente um "CNAME Record" ou "URL Redirect Record" para `@` e `www`, ligados a uma página de parqueamento) — se não os apagares, vão entrar em conflito com os novos
4. Adiciona os dois registos que a Netlify te mostrou no passo 3:
   - **A Record** → Host `@` → Value `75.2.60.5` → TTL Automatic
   - **CNAME Record** → Host `www` → Value `[o-nome-do-teu-site].netlify.app` → TTL Automatic
5. Grava

A propagação pode demorar entre alguns minutos e 24-48h (normalmente é rápido, 15-30 min).

---

## 5. HTTPS automático

Depois do DNS propagar, volta à Netlify → **Domain management** → deverá
aparecer **"Netlify DNS" / SSL certificate: Let's Encrypt** a ser emitido
automaticamente. Não precisas de fazer nada — é gratuito e automático.

---

## Fluxo depois disto montado

```
editas o index.html localmente
        ↓
git add . && git commit -m "descrição da alteração" && git push
        ↓
Netlify deteta o push e faz build+deploy automaticamente
        ↓
trilhapt.com atualizado em ~30-60 segundos
```

## Antes de ires para produção, não te esqueças de

- [ ] Substituir `GA_MEASUREMENT_ID` no `index.html` pelo teu ID real do Google Analytics 4 (ver `GOOGLE_ANALYTICS.md`)
- [ ] Confirmar que `WHATS_NUMBER` no `index.html` está correto (já está: 351928264249)
- [ ] Desligar o modo de manutenção no dia do lançamento (ver abaixo)

---

## Modo de manutenção

Enquanto preparas o lançamento, o site mostra uma página simples de "em
breve" em vez do conteúdo completo — assim já podes ter o domínio e o deploy
a funcionar sem mostrares um produto incompleto.

**Está ativo por padrão.** Para desligar no dia do lançamento, procura no
`index.html`:

```js
const MAINTENANCE_MODE = true;
```

Muda para `false`, faz commit e push:

```bash
git add index.html
git commit -m "Lançamento oficial"
git push
```

A Netlify faz o redeploy automático em segundos e o site completo fica
visível. Para voltar ao modo de manutenção (ex: para uma manutenção
pontual), basta mudar de volta para `true` e voltar a fazer push.

