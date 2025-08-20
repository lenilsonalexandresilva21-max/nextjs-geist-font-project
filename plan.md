```markdown
# Plano Detalhado de Implementação do Sistema de Gestão Escolar

Este plano detalha a criação de um sistema de gestão escolar com autenticação e dashboards para os seguintes perfis: admin, secretaria, professor e gestão. A seguir, encontra-se o roteiro de criação/modificação por arquivo, considerando as dependências existentes, tratamento de erros e melhores práticas.

---

## 1. Criação de Contexto de Autenticação

### Novo Arquivo: src/context/AuthContext.tsx
- **Objetivo:** Gerenciar o estado de autenticação e os dados do usuário (email e role) via React Context.
- **Conteúdo:**
  - Criação de um contexto com interface (ex.: AuthContextType) contendo os métodos `login`, `logout` e o estado `currentUser`.
  - Implementação do `AuthProvider` que encapsula a aplicação.
  - Inclusão de tratamento de erros durante o processo de login.
- **Boas Práticas:**
  - Uso de TypeScript para tipagem.
  - Mensagens de erro amigáveis e log de exceções.

---

## 2. Configuração do Layout Global

### Novo Arquivo (se inexistente): src/app/layout.tsx
- **Objetivo:** Envolver toda a aplicação com o AuthProvider para que o estado de autenticação esteja acessível a todas as páginas.
- **Conteúdo:**
  - Importar e envolver o componente `AuthProvider` ao redor de `{children}`.
  - Inclusão de um header simples que pode exibir um botão de logout (condicional, se o usuário estiver autenticado).
- **Boas Práticas:**
  - Estrutura de layout consistente e responsiva com tipografia e espaçamentos modernos.

---

## 3. Criação da Tela de Login

### Novo Arquivo: src/app/login/page.tsx
- **Objetivo:** Apresentar uma tela de login com campos de e-mail e senha.
- **Conteúdo:**
  - Componente funcional com diretiva `"use client"`.
  - Utilização dos componentes UI existentes (ex.: `Input`, `Button`, `Form`) para construir o formulário.
  - Validação dos campos e exibição de mensagens de erro com o componente `Alert`.
  - Simulação de autenticação (verificando se o e-mail corresponde a um perfil, por exemplo: admin@example.com para admin, secretaria@example.com para secretaria, etc.).
  - Redirecionamento para o dashboard respectivo usando o hook de navegação do Next.js.
- **UI/UX:**
  - Layout centralizado e moderno com cores neutras e espaçamento adequado.
  - Feedback visual claro para erros e sucesso.

---

## 4. Criação dos Dashboards Baseados em Perfis

### 4.1 Dashboard do Admin

#### Novo Arquivo: src/app/dashboard/admin/page.tsx
- **Objetivo:** Exibir funcionalidades administrativas com acesso a todas as áreas.
- **Conteúdo:**
  - Verificar se o usuário autenticado possui role “admin”; caso contrário, exibir mensagem “Acesso negado”.
  - Interface com cards ou seções para navegação para as demais áreas (secretaria, professor, gestão).
  - Integração de componentes UI como `Card`, `Button` e `NavigationMenu`.
- **UI/UX:**
  - Design limpo com ênfase em tipografia e espaçamentos, sem o uso de ícones externos.

### 4.2 Dashboard da Secretaria

#### Novo Arquivo: src/app/dashboard/secretaria/page.tsx
- **Objetivo:** Permitir cadastro de alunos e professores, além de gestão de faltas e boletins.
- **Conteúdo:**
  - Verificação de role “secretaria”.
  - Interface com **tabs** (usando o componente `Tabs`) para separar funcionalidades:
    - **"Cadastrar Aluno":** Formulário com inputs para nome, idade, etc.
    - **"Cadastrar Professor":** Formulário similar para cadastro de professores.
    - **"Gerenciar Faltas":** Formulário/tabela para input e visualização de faltas.
    - **"Boletim de Notas":** Área para entrada e visualização de notas.
  - Validação de dados com feedback em tempo real e tratamento de erros usando alertas.
- **UI/UX:**
  - Layout em abas com cores suaves e espaçamentos para diferenciar seções.

### 4.3 Dashboard do Professor

#### Novo Arquivo: src/app/dashboard/professor/page.tsx
- **Objetivo:** Permitir que professores lancem frequências, notas e gerenciem conteúdos.
- **Conteúdo:**
  - Verificação de role “professor”.
  - Interface com **tabs** para:
    - **"Frequência":** Formulário para registrar presença.
    - **"Notas":** Interface para lançar notas dos alunos.
    - **"Área de Conteúdos":** Espaço para upload ou gerenciamento de materiais didáticos.
  - Feedback imediato em caso de erros (ex.: campos obrigatórios não preenchidos).
- **UI/UX:**
  - Design moderno, com formulários intuitivos, botões claros e layout responsivo.

### 4.4 Dashboard da Gestão

#### Novo Arquivo: src/app/dashboard/gestao/page.tsx
- **Objetivo:** Fornecer uma visão geral do desempenho escolar com gráficos e médias.
- **Conteúdo:**
  - Verificação de role “gestao”.
  - Uso do componente `Chart` para renderizar gráficos (ex.: média de notas, taxas de faltas).
  - Seção de cards para exibir principais métricas e indicadores.
  - Validação dos dados e exibição de mensagens amigáveis caso os dados estejam ausentes.
- **UI/UX:**
  - Dashboard visualmente atrativo com cards, gráficos claros e tipografia moderna.

---

## 5. Implementação de Serviços de API Simulados

### Possíveis Novos Arquivos (Opcional para Futuras Funcionalidades)
- **src/app/api/login/route.ts:** Endpoint POST para realizar login real, com tratamento de erros (ex.: 401 para credenciais inválidas).
- **src/app/api/register-student/route.ts e src/app/api/register-teacher/route.ts:** Para registrar alunos/professores no backend.
- **Boas Práticas:**
  - Cada endpoint deverá retornar JSON estruturado com status de sucesso/erro.
  - Uso de try/catch para captura de exceções e resposta com códigos HTTP adequados.

---

## 6. Boas Práticas e Considerações Gerais

- **Validação e Tratamento de Erros:** Todos os formulários deverão validar os dados no cliente e exibir mensagens de alerta utilizando os componentes UI existentes.
- **Segurança:** Verificar as permissões do usuário para cada dashboard. Em produção, implementar verificação no servidor.
- **Design Moderno e Consistente:** Usar tipografia, cores, espaçamentos e layout consistentes. Evitar o uso de ícones externos, priorizando componentes minimalistas e textos descritivos.
- **Reutilização de Componentes:** Aproveitar os componentes em “src/components/ui” para formular interface e interatividade, garantindo consistência visual e facilidade de manutenção.
- **Modularidade:** Cada funcionalidade (login, cadastro, dashboards) é implementada em arquivos separados, facilitando testes e futuras manutenções.

---

## Resumo

- Foi criado o contexto de autenticação em "src/context/AuthContext.tsx" para gerenciar o estado do usuário.  
- Um layout global foi implementado em "src/app/layout.tsx" para englobar a aplicação com o AuthProvider.  
- A tela de login em "src/app/login/page.tsx" utiliza componentes UI existentes e validações.  
- Dashboards separados foram implementados para admin, secretaria, professor e gestão, cada um com verificação de role e funcionalidades específicas.  
- Foram planejados endpoints simulados para login e registro, com tratamento de erros e retorno de status HTTP.  
- O design segue um padrão moderno utilizando tipografia, cores e espaçamentos adequados, sem dependência de ícones externos.
