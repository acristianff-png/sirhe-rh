# SiRHE — Sistema de RH EMAD/EMAP
> Guia completo de instalação e configuração

---

## 🏗️ Arquitetura

```
GitHub Pages (frontend)  ←→  Google Apps Script (API)  ←→  Google Sheets (banco de dados)
     index.html                    Code.gs                    7 abas estruturadas
```

**Custo: R$ 0,00** — tudo hospedado em infraestrutura Google e GitHub.

---

## PASSO 1 — Criar o Google Sheets

1. Acesse [sheets.google.com](https://sheets.google.com) com sua conta institucional
2. Crie uma **planilha em branco**
3. Renomeie para: `SiRHE - Banco de Dados`
4. Copie o **ID da planilha** da URL:
   ```
   https://docs.google.com/spreadsheets/d/  ESTE_É_O_ID  /edit
   ```

---

## PASSO 2 — Configurar o Apps Script

1. Na planilha, acesse **Extensões → Apps Script**
2. Apague todo o código existente
3. Cole o conteúdo do arquivo `Code.gs`
4. Na linha 5, substitua o ID:
   ```javascript
   const SPREADSHEET_ID = 'COLE_SEU_ID_AQUI';
   ```
5. **Salve** (Ctrl+S) e nomeie o projeto como `SiRHE`

### Criar as abas automaticamente

6. No editor, clique no menu **▶ Executar**
7. Selecione a função: `setupSheets`
8. Autorize as permissões quando solicitado
9. Verifique no console: `✅ Setup concluído!`
10. Sua planilha agora tem 7 abas: PROFISSIONAIS, EQUIPES, VINCULOS, FERIAS, OCORRENCIAS, GESTORES, SESSOES

**Login padrão criado automaticamente:**
- E-mail: `admin@emad.gov.br`
- Senha: `Admin@123`

---

## PASSO 3 — Publicar o Apps Script como Web App

1. No editor Apps Script, clique em **Implantar → Nova implantação**
2. Clique no ícone ⚙️ e selecione **App da Web**
3. Configure:
   - **Descrição:** SiRHE API v1
   - **Executar como:** Eu mesmo
   - **Quem pode acessar:** Qualquer pessoa
4. Clique em **Implantar**
5. Copie a **URL do app da web** — parecida com:
   ```
   https://script.google.com/macros/s/XXXXXXXXXXXXXXXX/exec
   ```

---

## PASSO 4 — Configurar o Frontend

1. Abra o arquivo `index.html`
2. Na linha com `const SCRIPT_URL`, cole sua URL:
   ```javascript
   const SCRIPT_URL = 'https://script.google.com/macros/s/XXXXXXXXXXXXXXXX/exec';
   ```
3. Salve o arquivo

---

## PASSO 5 — Hospedar no GitHub Pages

1. Crie uma conta em [github.com](https://github.com) (se não tiver)
2. Crie um **novo repositório** — ex: `sirhe-rh`
3. Faça upload do arquivo `index.html`
4. Acesse **Settings → Pages**
5. Em **Source**, selecione `Deploy from a branch` → `main` → `/root`
6. Clique **Save**
7. Aguarde ~2 minutos e seu sistema estará disponível em:
   ```
   https://seu-usuario.github.io/sirhe-rh
   ```

---

## PASSO 6 — Primeiro acesso

1. Acesse a URL do GitHub Pages
2. Faça login com: `admin@emad.gov.br` / `Admin@123`
3. Vá em **Gestores** e crie os usuários reais
4. Desative ou remova o login padrão

---

## 🔄 Atualizações futuras

Para atualizar o sistema:
- **Frontend:** edite o `index.html` e faça commit no GitHub
- **Backend:** edite o `Code.gs` e crie uma **nova implantação** no Apps Script

---

## 📋 Estrutura das abas do Google Sheets

| Aba | Conteúdo |
|-----|----------|
| PROFISSIONAIS | Dados pessoais e profissionais |
| EQUIPES | Dados das equipes EMAD/EMAP |
| VINCULOS | Relação profissional ↔ equipe |
| FERIAS | Programação e controle de férias |
| OCORRENCIAS | Advertências, apontamentos, elogios |
| GESTORES | Usuários do sistema |
| SESSOES | Controle de sessões ativas |

---

## ⚠️ Regras de negócio implementadas

- ✅ Profissional em no máximo **2 equipes**
- ✅ Sem mesmo turno em equipes diferentes
- ✅ Férias bloqueadas se **conflito de categoria + equipe/irmã**
- ✅ Alertas automáticos de contratos vencendo (30 e 60 dias)
- ✅ Alertas de férias próximas (30 dias)
- ✅ Apenas gestores cadastrados acessam o sistema
- ✅ Sessões expiram em 24 horas

---

## 🆘 Solução de problemas

**"Erro de conexão"** → Verifique a URL do SCRIPT_URL no index.html

**"Não autorizado"** → Token expirado, faça login novamente

**Dados não aparecem** → Execute `setupSheets` novamente no Apps Script

**Conflito de CORS** → Garanta que o App da Web está publicado como "Qualquer pessoa"
