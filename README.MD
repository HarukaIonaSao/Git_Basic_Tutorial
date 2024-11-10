# 📓 Guia Completo de Configuração do Git e Integração com GitHub

>  **Este guia ajuda você a configurar o Git com HTTPS e SSH e a vincular seu repositório local ao GitHub.**


<div class="link-container" style="text-align: center;">
    <a href="https://github.com/HarukaIonaSao/Git_Basic/blob/main/README.MD#passo-1-instale-o-git" style="text-decoration: none;" >Instalação</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic/blob/main/README.MD#passo-2-configure-usu%C3%A1rio-e-e-mail-no-git" style="text-decoration: none;">Configuração</a><br>  
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#autentica%C3%A7%C3%A3o-no-github-https-ou-ssh" style="text-decoration: none;">Autenticação</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#vincular-o-reposit%C3%B3rio-local-ao-github" style="text-decoration: none;">Repositório remoto</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#%EF%B8%8F-substitui%C3%A7%C3%A3o-da-branch-master" style="text-decoration: none;">Branch master</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#-passo-a-passo-subindo-c%C3%B3digo-para-o-github" style="text-decoration: none;">Subir o código para o GitHub</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#passo-4-como-gerenciar-o-reposit%C3%B3rio" style="text-decoration: none;">Alterações no código</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#4-como-realizar-o-commit" style="text-decoration: none;">Commits</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#3-usando-commits-sem%C3%A2nticos" style="text-decoration: none;">Commits Semânticos</a><br>
    <a href="https://github.com/HarukaIonaSao/Git_Basic_Tutorial/tree/main?tab=readme-ov-file#resumo-do-processo" style="text-decoration: none;">Resumo</a>
</div>

 
## Passo 1: Instale o Git

Se ainda não tem o Git, faça o download pelo [site oficial do Git](https://git-scm.com/) e siga as instruções de instalação.

Verifique a instalação com:

```bash
git --version
```

## Passo 2: Configure Usuário e E-mail no Git

⚠️ **Importante**: O e-mail usado aqui deve ser o mesmo que está associado à sua conta GitHub para garantir a autenticação e associação correta dos commits.

### Configuração Global (Para Todos os Repositórios na Máquina)

Use `--global` para que o nome e e-mail sejam aplicados a todos os repositórios da sua máquina:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@example.com"
```

### Configuração Local (Para um Repositório Específico)

Para definir nome e e-mail apenas para um repositório, navegue até o diretório desse repositório e use os mesmos comandos **sem** o `--global`:

```bash
cd caminho/do/seu/repositorio
git config user.name "Nome Específico"
git config user.email "email-especifico@exemplo.com"
```

> 🤓 **Dica**: Isso é útil para projetos que exigem configurações diferentes (como um e-mail de trabalho e outro pessoal).

Para verificar as configurações e suas origens, use:

```bash
git config --list --show-origin
```

---

## Autenticação no GitHub: HTTPS ou SSH

Você pode autenticar no GitHub de duas maneiras: **com HTTPS (token de acesso)** ou **com SSH (chave SSH)**.

---

### Opção 1: Autenticação com HTTPS (Token de Acesso Pessoal)

1. **Crie um Token de Acesso no GitHub**:
   - Vá para [Configurações do GitHub](https://github.com/settings/tokens) > **Developer settings** > **Personal access tokens** > **Tokens classic** > **Generate new token**.
   - Escolha as permissões necessárias e clique em **Generate token**. **Guarde o token** em local seguro, pois ele será exibido apenas uma vez.

2. **Clonar o Repositório com HTTPS**:
   Use o URL no formato HTTPS ao clonar um repositório do GitHub:

   ```bash
   git clone https://github.com/username/repo-name.git
   ```

3. **Autenticar com o Token**:
   - Quando solicitado por nome de usuário e senha, insira seu **nome de usuário do GitHub** e o **token de acesso** como senha.
   - Para evitar digitar o token sempre, configure o gerenciador de credenciais do Git:

     ```bash
     git config --global credential.helper cache
     ```

---

### Opção 2: Autenticação com SSH (Chave SSH)

1. **Gerar uma Chave SSH**:
   No terminal, execute o seguinte comando (substitua o e-mail pelo seu):

   ```bash
   ssh-keygen -t ed25519 -C "seu-email@example.com"
   ```

   - Pressione Enter para aceitar o local padrão para salvar a chave.
   - Opcionalmente, adicione uma senha para segurança adicional.

2. **Adicionar a Chave SSH ao `ssh-agent`**:
   No terminal, execute:

   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/.ssh/id_ed25519
   ```

3. **Adicionar a Chave SSH ao GitHub**:
   - Copie a chave pública (parte `.pub`) com:

     ```bash
     cat ~/.ssh/id_ed25519.pub
     ```

   - No GitHub, vá até [Configurações de Chaves SSH](https://github.com/settings/keys) > **SSH and GPG keys** > **New SSH key** e cole a chave pública.

4. **Clonar o Repositório com SSH**:
   Após configurar a chave SSH, clone o repositório usando o link SSH:

   ```bash
   git clone git@github.com:username/repo-name.git
   ```

5. **Testar a Conexão SSH**:
   Verifique se o GitHub reconhece sua chave SSH:

   ```bash
   ssh -T git@github.com
   ```

---

## Vincular o Repositório Local ao GitHub

Caso tenha um repositório local que deseja vincular ao GitHub, siga estes passos:

1. **Adicione o Repositório Remoto**:

   - **Com HTTPS**:

     ```bash
     git remote add origin https://github.com/username/repo-name.git
     ```

   - **Com SSH**:

     ```bash
     git remote add origin git@github.com:username/repo-name.git
     ```

2. **Enviar o Código para o GitHub**:

   Depois de adicionar o repositório remoto, você pode enviar seus commits para o GitHub:

   ```bash
   git push -u origin main
   ```

> 📝 **Nota**: Substitua `main` pelo nome da sua branch principal, caso seja diferente.

---

### ⚠️ **Substituição da Branch `master`**

A **branch `master`** foi historicamente o nome padrão para a branch principal de repositórios Git, mas nos últimos anos, devido a preocupações com a terminologia, muitas plataformas e projetos decidiram descontinuar o uso da branch `master` em favor de nomes mais neutros, como **`main`**, **`primary`** ou outros termos. A mudança está sendo promovida para refletir uma linguagem mais inclusiva e respeitosa em relação a questões sociais.

#### 🌍 **Motivos para a Mudança**
O uso do termo `master` foi associado a conotações históricas que não são mais vistas como apropriadas em um contexto moderno e inclusivo. Para promover um ambiente mais acolhedor e livre de termos potencialmente problemáticos, a indústria de software começou a substituir a branch `master` por uma alternativa mais neutra.

#### 🔄 **Impacto da Mudança**
- **GitHub e outras plataformas**: O GitHub, GitLab e outras plataformas populares começaram a adotar **`main`** como o nome padrão para a branch principal em novos repositórios. Isso significa que quando você cria um novo repositório, a branch principal automaticamente será nomeada como `main`, em vez de `master`. Para saber mais, acesse o [site](https://github.com/github/renaming) oficial ou leia esse [arquivo](https://medium.com/rd-shipit/por-que-tamb%C3%A9m-estamos-trocando-o-nome-de-nossa-branch-principal-de-master-para-main-744b9d9b1af2). 
  
- **Migração**: Para repositórios já existentes, a mudança não é obrigatória. Contudo, é altamente recomendada para que os projetos sigam as melhores práticas inclusivas e estejam alinhados com as diretrizes mais recentes. O GitHub, por exemplo, oferece uma maneira simples de renomear a branch `master` para `main` sem afetar o histórico do projeto.

#### 🛠️ **Como Renomear a Branch `master` para `main`**

Se você está mantendo um repositório que usa `master` e deseja mudar para `main`, pode fazer isso facilmente com os seguintes comandos Git:

1. **Renomeie a branch local**:
   ```bash
   git branch -m master main
   ```

2. **Atualize o repositório remoto**:
   Após renomear a branch local, você precisará fazer a atualização no repositório remoto (GitHub, GitLab, etc.):

   ```bash
   git push -u origin main
   ```

3. **Altere a branch padrão no GitHub**:
   - Vá para seu repositório no GitHub.
   - Clique em **Settings** > **Branches**.
   - Em "Default branch", altere de `master` para `main`.

4. **Excluir a antiga branch `master` no repositório remoto** (se desejado):
   Após garantir que tudo está funcionando com a nova branch, você pode excluir a `master` remota para evitar confusão:

   ```bash
   git push origin --delete master
   ```

#### 📅 **Quando Isso Acontece?**
Embora o GitHub tenha iniciado a mudança em 2020, o processo de substituição do uso de `master` como padrão ainda está em andamento. Muitos projetos ainda estão em transição, mas a tendência é que o uso de `master` diminua ao longo do tempo, com a maioria dos novos repositórios já adotando `main` ou outro nome neutro como padrão.

#### 📝 **Resumindo**
Embora a mudança não seja obrigatória para repositórios antigos, é importante adotar uma terminologia mais inclusiva, alinhada com as melhores práticas da comunidade. Se você ainda usa `master`, considere renomeá-la para `main` para garantir que seu projeto esteja alinhado com a evolução da linguagem e as práticas modernas de desenvolvimento.

---

# 🚀 **Passo a Passo: Subindo Código para o GitHub**

## Passo 1: Criar uma Conta no GitHub

1. **Acesse o site**: [https://github.com](https://github.com).
2. **Clique em "Sign up"** no canto superior direito.
3. **Preencha o formulário** com seu nome, e-mail, senha e clique em **"Create account"**.
4. Siga as instruções para concluir o processo de configuração da sua conta (verificação de e-mail, escolha do plano, etc.).

---

## Passo 2: Estrutura do Projeto

Vamos simular a estrutura de um projeto simples com uma pasta `source` e um arquivo `index.html`.

```
meu-projeto/
│
├── source/
│   └── style.css
│__ index.html
└── README.md
```

### Código de exemplo no `index.html`:

```html
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Meu Projeto</title>
</head>
<body>
    <h1>Bem-vindo ao Meu Projeto!</h1>
    <p>Este é um projeto simples que estou subindo para o GitHub.</p>
</body>
</html>
```

---

## Passo 3: Subindo o Código para o GitHub

### **Método 1: Usando o Git Bash, Terminal ou PowerShell**

1. **Abra o terminal**:
   - **Git Bash**: Clique com o botão direito na pasta do seu projeto e selecione **Git Bash Here**.
   - **Terminal (macOS/Linux)** ou **PowerShell (Windows)**: Abra a janela do terminal na pasta onde está o projeto.

2. **Inicialize o repositório Git**:
   No terminal, navegue até a pasta do seu projeto e execute o comando para inicializar o repositório:

   ```bash
   git init
   ```

3. **Adicione os arquivos ao Git**:
   Adicione todos os arquivos do projeto ao Git:

   ```bash
   git add .
   ```

4. **Crie o primeiro commit**:
   Execute o commit inicial com uma mensagem explicativa:

   ```bash
   git commit -m "Primeiro commit - subindo o projeto para o GitHub"
   ```

5. **Adicione o repositório remoto**:
   Crie um repositório no GitHub e copie a URL do repositório (por exemplo, `https://github.com/username/nome-do-repositorio.git`).

   Adicione o repositório remoto:

   ```bash
   git remote add origin https://github.com/username/nome-do-repositorio.git
   ```

6. ** Enviar o Código para o GitHub**:
   - Acesse seu GitHub e crie um novo repositório clicando no botão **"New repository"**.
   - **Dica**: O GitHub agora usa a **branch `main`** por padrão para novos repositórios, então o nome da branch principal será automaticamente **`main`**.

2. **Adicionar o repositório remoto**:
   No seu terminal, depois de criar o repositório no GitHub, adicione o repositório remoto:

   ```bash
   git remote add origin https://github.com/username/nome-do-repositorio.git
   ```

3. **Envie os arquivos para o GitHub**:
   Agora, envie seu código para o repositório remoto. Se você estiver usando a **branch `main`**, execute o comando:

   ```bash
   git push -u origin main
   ```

   Caso esteja usando **`master`**, o comando será:

   ```bash
   git push -u origin master
   ```

   > ⚠️ **Atenção**: Se o repositório já tiver sido criado com a branch `master`, você precisa renomear essa branch para `main`, como discutido anteriormente, para seguir as melhores práticas. **O GitHub e outras plataformas estão movendo-se em direção a uma terminologia mais inclusiva**, substituindo `master` por `main`.

---

### **Método 2: Usando o Gerenciador de Arquivos**

Se você preferir uma interface gráfica, pode fazer isso sem usar o terminal. Embora a maneira mais comum de interagir com Git seja por meio da linha de comando, o GitHub permite carregar arquivos diretamente pela interface web.

1. **Crie o repositório no GitHub**:
   - Vá para [GitHub](https://github.com) e faça login.
   - Clique em **+** no canto superior direito e selecione **New repository**.
   - Dê um nome ao seu repositório e clique em **Create repository**.

2. **Faça o upload dos arquivos**:
   - No seu repositório no GitHub, clique em **Upload files**.
   - Arraste e solte os arquivos do seu projeto ou use o botão **choose your files** para selecionar os arquivos.
   - Clique em **Commit changes** para enviar os arquivos.

> 💡 **Dica**: Embora essa opção seja fácil, ela não é ideal para grandes projetos ou quando você precisar gerenciar várias versões de arquivos. O uso do Git Bash ou GitHub Desktop é recomendado para projetos mais avançados.

---

## Passo 4: Como Gerenciar o Repositório

Após subir o projeto para o GitHub, você pode começar a gerenciar o código de várias maneiras:

### **Fazendo alterações e subindo novamente**

1. **Faça alterações no código** (por exemplo, edite `index.html`).
2. No terminal, adicione as alterações:

   ```bash
   git add .
   ```

3. **Comite as mudanças**:

   ```bash
   git commit -m "Alterações no index.html"
   ```

4. **Envie para o GitHub**:

   ```bash
   git push origin main
   ```

Agora, o GitHub estará atualizado com as últimas alterações feitas no código.

---

## **Resumo do Processo:**

1. **Criar conta no GitHub**: Crie um perfil no GitHub para hospedar seus repositórios.
2. **Estruturar o projeto**: Crie a estrutura de pastas e arquivos.
3. **Subir para o GitHub**:
   - Usando **Git Bash/Terminal/PowerShell** ou o **Gerenciador de Arquivos** no GitHub.
4. **Gerenciar alterações**: Use o Git para adicionar, commitar e enviar novas versões para o GitHub.

Agora você está pronto para usar o Git e o GitHub de forma eficiente para gerenciar seus projetos! 🚀

Para garantir que suas alterações no código sejam bem organizadas e compreensíveis, é importante seguir boas práticas de **commit**. Isso inclui **fazer commits de forma frequente** e **usando commits semânticos**.

---

# 💻 **Como Fazer Alterações no Código e Commitar na Frequência Correta**

## 1. **Fazendo Alterações no Código**

Sempre que você realizar alguma alteração no código, como adicionar uma nova funcionalidade, corrigir um bug ou refatorar uma parte do código, faça isso de forma **incremental**. Isso significa que você não deve esperar até que o código esteja completamente finalizado para fazer um commit. 

É recomendado **realizar commits pequenos e frequentes**. Isso permite que o histórico do seu projeto seja claro, e facilita a identificação de problemas, caso algum erro aconteça após a alteração.

Por exemplo, se você está trabalhando em uma página de login, você pode:

- Fazer um commit para **adicionar o formulário de login**.
- Fazer outro commit para **validar os campos do formulário**.
- Fazer um terceiro commit para **conectar o formulário com a API de login**.

Isso torna o histórico de commits mais legível e facilita o processo de revisão de código.

---

## 2. **Commits na Frequência Correta**

Evite fazer commits grandes que englobam múltiplas alterações. Em vez disso, **faça commits de forma regular**, de preferência **sempre que terminar uma tarefa ou uma unidade de trabalho específica**. 

Aqui estão algumas situações em que é adequado fazer um commit:

- **Após adicionar uma nova funcionalidade** (ex.: "Adicionar formulário de login").
- **Após corrigir um bug** (ex.: "Corrigir erro na validação do e-mail").
- **Após refatorar ou melhorar o código** (ex.: "Refatorar função de validação do formulário").
- **Após testes ou ajustes** (ex.: "Ajustar layout da página de login para mobile").

Isso ajuda a manter o histórico do projeto claro e organizado, além de permitir reverter facilmente a qualquer ponto anterior, caso necessário.

---

## 3. **Usando Commits Semânticos**

Os **commits semânticos** seguem uma convenção específica para que o significado de cada commit seja claro. Em vez de apenas escrever algo como "Alterações no código", você deve usar **mensagens de commit mais descritivas** e estruturadas, com base no tipo de alteração que está sendo feita.

Aqui estão os principais tipos de commits semânticos:

- **feat**: Adição de nova funcionalidade ao código (ex.: "feat: adicionar formulário de login").
- **fix**: Correção de um bug no código (ex.: "fix: corrigir erro de validação do e-mail").
- **docs**: Modificações na documentação do projeto (ex.: "docs: atualizar README com instruções de instalação").
- **style**: Alterações que não afetam a lógica do código, como mudanças de estilo (ex.: "style: ajustar indentação e espaçamento").
- **refactor**: Alterações no código que não adicionam funcionalidades nem corrigem bugs, mas melhoram a estrutura (ex.: "refactor: melhorar a legibilidade da função de validação").
- **test**: Alterações relacionadas a testes (ex.: "test: adicionar testes para a função de login").
- **chore**: Alterações que não se encaixam em outras categorias (ex.: "chore: atualizar dependências do projeto").

### Exemplo de commits semânticos:

```bash
git commit -m "feat: adicionar página de login"
git commit -m "fix: corrigir erro no envio de dados do formulário"
git commit -m "docs: atualizar o README com informações de configuração"
git commit -m "style: corrigir indentação na função de validação"
```

Você pode ler mais sobre **commits semânticos** e sua importância neste repositório:

👉 [Commits Semânticos - GitHub](https://github.com/HarukaIonaSao/Commits-semanticos)

---

## 4. **Como Realizar o Commit**

Após fazer as alterações no seu código, você precisa **adicionar os arquivos modificados** ao Git e **fazer o commit**.

### Passos para adicionar e subir as alterações:

1. **Adicionar os arquivos alterados**:

   ```bash
   git add .
   ```

2. **Fazer o commit com uma mensagem semântica**:

   ```bash
   git commit -m "feat: adicionar a página de login"
   ```

3. **Enviar as alterações para o repositório remoto no GitHub**:

   ```bash
   git push origin main
   ```

---

## 5. **Por que é Importante Seguir essas Práticas?**

- **Histórico de código mais claro**: Facilita o entendimento das mudanças feitas no projeto, especialmente em equipes.
- **Facilidade de reversão**: Com commits pequenos e claros, é mais fácil reverter alterações específicas.
- **Melhoria na colaboração**: Equipes podem trabalhar de forma mais eficiente com um histórico de commits bem estruturado.
- **Automação e integrações**: Algumas ferramentas, como CI/CD (Integração Contínua/Entrega Contínua), podem se beneficiar de mensagens de commit claras e bem estruturadas para automatizar o fluxo de trabalho.

---
| **Passo** | **Descrição**                                                                                             | **Comando/Link**                                                                                                                                  |
|-----------|-----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| **1**     | Instalar o Git                                                                                             | [Site oficial do Git](https://git-scm.com/)                                                                                                    |
| **2**     | Configurar Usuário e E-mail no Git (Global)                                                                 | `git config --global user.name "Seu Nome"`<br>`git config --global user.email "seu-email@example.com"`                                           |
| **3**     | Configurar Usuário e E-mail no Git (Local)                                                                  | `cd caminho/do/seu/repositorio`<br>`git config user.name "Nome Específico"`<br>`git config user.email "email-especifico@exemplo.com"`           |
| **4**     | Verificar Configurações do Git                                                                                | `git config --list --show-origin`                                                                                                                |
| **5**     | Autenticação no GitHub via HTTPS (Token de Acesso)                                                         | [Criar Token no GitHub](https://github.com/settings/tokens)<br>`git clone https://github.com/username/repo-name.git`<br>`git config --global credential.helper cache` |
| **6**     | Autenticação no GitHub via SSH (Chave SSH)                                                                  | `ssh-keygen -t ed25519 -C "seu-email@example.com"`<br>`eval "$(ssh-agent -s)"`<br>`ssh-add ~/.ssh/id_ed25519`<br>[Adicionar chave SSH ao GitHub](https://github.com/settings/keys) |
| **7**     | Clonar Repositório com HTTPS                                                                                 | `git clone https://github.com/username/repo-name.git`                                                                                            |
| **8**     | Clonar Repositório com SSH                                                                                   | `git clone git@github.com:username/repo-name.git`                                                                                               |
| **9**     | Vincular Repositório Local ao GitHub (HTTPS)                                                                 | `git remote add origin https://github.com/username/repo-name.git`                                                                                |
| **10**    | Vincular Repositório Local ao GitHub (SSH)                                                                   | `git remote add origin git@github.com:username/repo-name.git`                                                                                  |
| **11**    | Enviar Código para o GitHub                                                                                 | `git push -u origin main`                                                                                                                       |
| **12**    | Renomear Branch `master` para `main` (opcional)                                                              | `git branch -m master main`<br>`git push -u origin main`<br>`git push origin --delete master`                                                   |
| **13**    | Testar Conexão SSH                                                                                          | `ssh -T git@github.com`                                                                                                                         |
| **14**    | Criar Conta no GitHub                                                                                        | [Criar conta no GitHub](https://github.com)                                                                                                    |
| **15**    | Estrutura de Projeto Simulada                                                                                | Ver estrutura: `meu-projeto/source/index.html`                                                                                                 |
| **16**    | Inicializar Repositório Git                                                                                 | `git init`                                                                                                                                      |
| **17**    | Adicionar Arquivos ao Git                                                                                    | `git add .`                                                                                                                                     |
| **18**    | Fazer o Primeiro Commit                                                                                     | `git commit -m "Primeiro commit - subindo o projeto para o GitHub"`                                                                             |
| **19**    | Adicionar Repositório Remoto                                                                                 | `git remote add origin https://github.com/username/nome-do-repositorio.git`                                                                    |







