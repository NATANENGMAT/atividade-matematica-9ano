# CETI Elisa Bessa Freire — Atividade de Matemática (9º Ano)
## Grandezas Diretamente e Inversamente Proporcionais & Regra de Três

Aplicação web escolar moderna, responsiva (*Mobile-First*), desenvolvida em **Tailwind CSS** e integrada ao **Supabase (PostgreSQL & Storage)** para realização e envio da atividade avaliativa trimestral de matemática dos alunos dos **9º Anos (9º 1, 9º 2 e 9º 4)**.

---

## 📌 Sumário
1. [Visão Geral e Funcionalidades](#-visão-geral-e-funcionalidades)
2. [Estrutura dos Arquivos](#-estrutura-dos-arquivos)
3. [Passo a Passo: Configuração do Supabase](#-passo-a-passo-configuração-do-supabase)
4. [Como Publicar Gratuitamente no GitHub Pages](#-como-publicar-gratuitamente-no-github-pages)
5. [Como Testar a Aplicação (Modo Teste do Professor)](#-como-testar-a-aplicação-modo-teste-do-professor)
6. [Gabarito Oficial e Resoluções Passo a Passo](#-gabarito-oficial-e-resoluções-passo-a-passo)
7. [Consulta de Notas e Exportação de Relatórios](#-consulta-de-notas-e-exportação-de-relatórios)

---

## 🚀 Visão Geral e Funcionalidades

- **Design Mobile-First:** Otimizado para telas de smartphones de alunos (Android e iPhone) com interface limpa, botões grandes e leitura confortável.
- **Identificação com Dropdowns em Cascata:**
  - Seleção da Turma (`9º 1`, `9º 2`, `9º 4`).
  - Preenchimento automático do nome oficial do estudante a partir da lista fornecida pela escola.
- **20 Questões no Formato Oficial:**
  - **Parte 1 (Questões 1 a 10):** Classificação das Grandezas (Múltipla Escolha A, B, C, D, E).
  - **Parte 2 (Questões 11 a 20):** Situações-problema com Regra de Três Simples.
- **Anexo Obrigatório de Foto da Folha de Cálculos:**
  - Acionamento direto da câmera do smartphone (`capture="environment"`) ou galeria.
  - Pré-visualização com recurso de zoom em tela cheia.
  - **Compressão automática no navegador (HTML5 Canvas):** Fotos pesadas (8 MB a 15 MB) são compactadas para ~800 KB preservando a nitidez total dos cálculos a caneta/lápis, evitando falhas em conexões 3G/4G.
- **Validações e Segurança:**
  - **Prazo Estrito:** Válido estritamente no dia **01/10/2026, das 08:00 às 23:59**. Fora desse intervalo o envio fica bloqueado, com contagem regressiva e aviso ao aluno.
  - **Envio Único por Aluno:** O sistema impede envios duplicados, conferindo em tempo real se o estudante já enviou. O banco possui restrição `UNIQUE(turma, aluno)`.
  - **Comprovante de Entrega Oficial:** Gera código de autenticação/protocolo único (ex: `CETI-20261001-91-ALICE-8A2F1`), data/hora, resumo de respostas e botão para imprimir/salvar em PDF.

---

## 📁 Estrutura dos Arquivos

```
atividade-matematica-ceti/
│
├── index.html            # Aplicação Web completa (HTML5 + Tailwind CSS + JS + Supabase Client)
├── supabase-schema.sql   # Script SQL com tabelas, RLS, Storage Bucket e View de notas automáticas
└── README.md             # Guia completo de configuração e publicação
```

---

## ⚙️ Passo a Passo: Configuração do Supabase

O [Supabase](https://supabase.com) é uma plataforma em nuvem gratuita que fornece o banco de dados PostgreSQL e o serviço de armazenamento de fotos (Storage).

### 1. Criar Projeto no Supabase
1. Acesse **[supabase.com](https://supabase.com)** e faça login (pode usar sua conta GitHub ou Google).
2. Clique em **"New Project"**.
3. Defina:
   - **Name:** `ceti-elisa-bessa-matematica`
   - **Database Password:** Escolha uma senha segura e anote.
   - **Region:** `South America (São Paulo)` (para menor latência no Brasil).
4. Clique em **"Create new project"** e aguarde cerca de 1 a 2 minutos até o painel inicializar.

---

### 2. Executar o Script SQL
1. No menu lateral esquerdo do Supabase, clique no ícone **SQL Editor** (ou pressione as teclas de atalho).
2. Clique no botão **"New query"**.
3. Abra o arquivo `supabase-schema.sql` deste projeto, copie todo o seu conteúdo e cole no editor do Supabase.
4. Clique no botão verde **"Run"** (no canto inferior direito).
5. O script criará automaticamente:
   - A tabela `public.atividades_respostas` com restrição de envio único por aluno.
   - Os índices de busca por turma e aluno.
   - As regras de segurança **Row Level Security (RLS)** permitindo envio e verificação anônima pelos alunos.
   - O bucket de Storage chamado `calculos` com acesso público para visualização das imagens.
   - A view `public.vw_relatorio_notas`, que corrige automaticamente as 20 questões e calcula a nota final de 0,0 a 10,0 de cada estudante!

---

### 3. Obter as Chaves de Conexão da API
1. No menu lateral esquerdo, clique no ícone de engrenagem **Project Settings** (Configurações do Projeto).
2. Acesse a aba **API**.
3. Copie os seguintes valores:
   - **Project URL:** algo como `https://xyzabcdefg.supabase.co`
   - **Project API Keys -> anon / public:** uma sequência longa de caracteres iniciando com `eyJhbGci...`

---

### 4. Configurar as Chaves no Projeto

Você tem **duas formas** de definir as credenciais:

#### Opção A (Pela Interface - Mais Fácil):
1. Abra o arquivo `index.html` em qualquer navegador.
2. No topo direito do cabeçalho, clique no ícone de **Engrenagem** (⚙️).
3. Cole a **Project URL** e a **Anon Key** nos respectivos campos.
4. Clique em **"Salvar"**. As credenciais ficarão salvas no seu navegador.

#### Opção B (Direto no Código-Fonte):
1. Abra o arquivo `index.html` em um editor de texto.
2. Localize as linhas iniciais do script JavaScript:
```javascript
const CONFIG_PADRAO = {
  SUPABASE_URL: "https://SEU_PROJETO_AQUI.supabase.co",
  SUPABASE_ANON_KEY: "SUA_CHAVE_ANON_PUBLICA_AQUI"
};
```
3. Substitua pelos valores reais do seu projeto Supabase e salve o arquivo.

---

## 🌐 Como Publicar Gratuitamente no GitHub Pages

O GitHub Pages permite hospedar a aplicação gratuitamente com link público e seguro (`https://`) para os estudantes acessarem pelo celular.

### Método Rápido (Via Interface Web do GitHub)
1. Acesse sua conta no **[GitHub](https://github.com)**.
2. Clique no botão **"New"** para criar um novo repositório.
3. Nomeie o repositório, por exemplo: `atividade-matematica-9ano`.
4. Deixe marcado como **Public** e clique em **"Create repository"**.
5. Na tela seguinte, clique no link **"uploading an existing file"**.
6. Arraste e solte o arquivo `index.html` (e opcionalmente o `README.md`).
7. Clique no botão verde **"Commit changes"**.
8. Vá na aba **Settings** (Configurações) do repositório.
9. No menu lateral esquerdo, clique em **Pages**.
10. Em **Build and deployment -> Branch**, selecione a branch `main` e a pasta `/(root)`.
11. Clique em **Save**.
12. Em cerca de 1 minuto, o GitHub gerará o link oficial:
    `https://seu-usuario.github.io/atividade-matematica-9ano/`
13. Compartilhe esse link no grupo de WhatsApp da turma ou no Google Classroom!

---

## 🧪 Como Testar a Aplicação (Modo Teste do Professor)

Como a aplicação possui uma regra estrita de envio apenas no dia **01/10/2026 das 08:00 às 23:59**, criamos um recurso especial para que o professor possa homologar e testar o envio sem precisar esperar pelo dia ou hora do prazo:

1. **Pelo Botão no Cabeçalho:**
   - No topo da página, ao lado do nome da escola, clique no botão com ícone de raio: **"Modo Normal" / "Modo Teste"**.
   - Ao ativar, a barra superior ficará azul com a mensagem **"MODO DE TESTE ATIVO"**, permitindo simular o envio imediatamente.
2. **Pela URL:**
   - Adicione `?modo_teste=true` ao final do link da página (ex: `https://meusite.com/?modo_teste=true`).

---

## 📋 Gabarito Oficial e Resoluções Passo a Passo

### PARTE 1: Classificação das Grandezas (Questões 1 a 10)

| Questão | Enunciado Resumido | Relação Matemática | Gabarito Oficial |
| :---: | :--- | :--- | :---: |
| **Q1** | Operários em uma obra e tempo de conclusão | Mais operários $\rightarrow$ Menos tempo necessário | **B) Inversamente Proporcionais** |
| **Q2** | Kg de carne comprada e valor total pago | Mais carne comprada $\rightarrow$ Maior o valor pago | **B) Diretamente Proporcionais** |
| **Q3** | Velocidade média e tempo do trajeto | Maior velocidade $\rightarrow$ Menor o tempo de percurso | **B) Inversamente Proporcionais** |
| **Q4** | Páginas lidas e tempo dedicado à leitura | Mais tempo lendo $\rightarrow$ Mais páginas lidas | **B) Diretamente Proporcionais** |
| **Q5** | Ração consumida por dia e quantidade de cães | Mais cães $\rightarrow$ Mais ração consumida | **B) Diretamente Proporcionais** |
| **Q6** | Torneiras abertas e tempo para encher reservatório | Mais torneiras abertas $\rightarrow$ Menos tempo | **B) Inversamente Proporcionais** |
| **Q7** | Dias trabalhados e salário (diária fixa) | Mais dias trabalhados $\rightarrow$ Maior o salário | **B) Diretamente Proporcionais** |
| **Q8** | Quantidade de água e umidade do solo | Mais água irrigada $\rightarrow$ Maior a umidade do solo | **B) Diretamente Proporcionais** |
| **Q9** | Dias de viagem e mantimentos consumidos pela tropa | Mais dias viajando $\rightarrow$ Mais mantimentos gastos | **B) Diretamente Proporcionais** |
| **Q10** | Base e altura de retângulo com área constante | Se a base aumenta $\rightarrow$ a altura diminui ($b \cdot h = A$) | **B) Inversamente Proporcionais** |

---

### PARTE 2: Cálculo de Regra de Três (Questões 11 a 20)

#### Questão 11:
- **Problema:** 9 pedreiros constroem uma casa em 20 dias. Quantos pedreiros para construir em 12 dias?
- **Classificação:** Inversamente proporcional (menos dias exigem mais pedreiros).
- **Cálculo:**
  $$9 \times 20 = x \times 12 \implies 180 = 12x \implies x = \frac{180}{12} = 15 \text{ pedreiros}$$
- **Gabarito Oficial:** **C) 15 pedreiros**

#### Questão 12:
- **Problema:** 3 máquinas produzem 1.800 livros. Quantas máquinas para produzir 5.400 livros no mesmo período?
- **Classificação:** Diretamente proporcional (mais livros exigem mais máquinas).
- **Cálculo:**
  $$\frac{3}{1800} = \frac{x}{5400} \implies x = \frac{3 \times 5400}{1800} = 3 \times 3 = 9 \text{ máquinas}$$
- **Gabarito Oficial:** **E) 9 máquinas**

#### Questão 13:
- **Problema:** Ônibus a 90 km/h faz percurso em 4 h. Quanto tempo levaria a 120 km/h?
- **Classificação:** Inversamente proporcional (maior velocidade reduz o tempo).
- **Cálculo:**
  $$90 \times 4 = 120 \times t \implies 360 = 120t \implies t = \frac{360}{120} = 3 \text{ horas}$$
- **Gabarito Oficial:** **B) 3h**

#### Questão 14:
- **Problema:** Para pavimentar 300m, 12 operários levam 15 dias. Quantos dias para 20 operários pavimentarem os mesmos 300m?
- **Classificação:** Inversamente proporcional (mais operários levam menos dias).
- **Cálculo:**
  $$12 \times 15 = 20 \times d \implies 180 = 20d \implies d = \frac{180}{20} = 9 \text{ dias}$$
- **Gabarito Oficial:** **C) 9 dias**

#### Questão 15:
- **Problema:** 5 máquinas produzem 4.500 panfletos em 3h. Quantos panfletos com 8 máquinas no mesmo período?
- **Classificação:** Diretamente proporcional (mais máquinas produzem mais panfletos).
- **Cálculo:**
  $$\frac{4500}{5} = 900 \text{ panfletos por máquina} \implies 8 \times 900 = 7.200 \text{ panfletos}$$
- **Gabarito Oficial:** **B) 7.200 panfletos**

#### Questão 16:
- **Problema:** 6 torneiras enchem um reservatório em 10h. Em quanto tempo com 15 torneiras?
- **Classificação:** Inversamente proporcional (mais torneiras reduzem o tempo).
- **Cálculo:**
  $$6 \times 10 = 15 \times t \implies 60 = 15t \implies t = \frac{60}{15} = 4 \text{ horas}$$
- **Gabarito Oficial:** **B) 4 horas**

#### Questão 17:
- **Problema:** 45 cães por 12 dias consomem 108 kg de ração. Quantos kg para 60 cães no mesmo período?
- **Classificação:** Diretamente proporcional (mais cães consomem mais ração).
- **Cálculo:**
  $$\frac{108}{45} = 2,4 \text{ kg por cão} \implies 60 \times 2,4 = 144 \text{ kg}$$
- **Gabarito Oficial:** **C) 144 kg**

#### Questão 18:
- **Problema:** Consumo de 30 litros para 350 km. Quantos litros consumirá para percorrer 560 km?
- **Classificação:** Diretamente proporcional (maior distância consome mais combustível).
- **Cálculo:**
  $$\frac{30}{350} = \frac{x}{560} \implies x = \frac{30 \times 560}{350} = \frac{16800}{350} = 48 \text{ litros}$$
- **Gabarito Oficial:** **C) 48 litros**

#### Questão 19:
- **Problema:** Guarnição de 120 soldados tem mantimentos para 30 dias. Com reforço de 30 soldados (total 150), durarão quantos dias?
- **Classificação:** Inversamente proporcional (mais soldados consomem os mantimentos em menos dias).
- **Cálculo:**
  $$120 \times 30 = 150 \times d \implies 3600 = 150d \implies d = \frac{3600}{150} = 24 \text{ dias}$$
- **Gabarito Oficial:** **C) 24 dias**

#### Questão 20:
- **Problema:** Ciclista faz trilha em 4h a 25 km/h. Se desejar fazer em 5h, qual deve ser a nova velocidade média?
- **Classificação:** Inversamente proporcional (mais tempo implica velocidade média menor).
- **Cálculo:**
  $$4 \times 25 = 5 \times v \implies 100 = 5v \implies v = \frac{100}{5} = 20 \text{ km/h}$$
- **Gabarito Oficial:** **C) 20 km/h**

---

## 📊 Consulta de Notas e Exportação de Relatórios

No painel do Supabase, o professor pode consultar os resultados em tempo real:

### 1. Ver a Tabela com Fotos dos Cálculos
- Acesse **Table Editor -> `atividades_respostas`**.
- Cada registro traz o aluno, a turma, a data/hora, o protocolo e o link clicável direto para a foto da folha de rascunho anexada!

### 2. Ver as Notas Corrigidas Automaticamente
- Acesse **SQL Editor** e execute:
```sql
SELECT turma, aluno, acertos, nota_final, data_hora_envio, foto_calculos_url
FROM public.vw_relatorio_notas
ORDER BY turma ASC, aluno ASC;
```

### 3. Exportar para Excel / CSV
- Na tela do **Table Editor**, clique no botão **"Export as CSV"** no canto superior direito para baixar a planilha completa com as respostas e notas dos alunos.

---

### 🏫 CETI Elisa Bessa Freire — SEDUC Amazonas
*Atividade desenvolvida com padrões web modernos para valorizar a educação pública e facilitar o acompanhamento pedagógico.*
