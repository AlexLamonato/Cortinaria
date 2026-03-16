# Organização das informações por assunto.
# 📄 Documento de Arquitetura da Informação - Cortinaria

## 1. Introdução e Objetivos
**Objetivo:** Definir como clientes e vendedores de cortinas encontrarão informações e realizarão tarefas no sistema **AgendaPorf**, garantindo um fluxo de compra fluido e uma gestão de estoque eficiente para as empresas.

### Personas:
* **Persona 1 - Vendedor:** Profissional vinculado a uma empresa que precisa cadastrar seus produtos e gerenciar a comunicação com os clientes.
* **Persona 2 - Cliente:** Usuário que busca praticidade para encontrar, visualizar e comprar cortinas através do aplicativo.

---

## 2. Inventário e Agrupamento de Conteúdo

### Inventário de Conteúdo:

| ID | Item | Tipo | Onde aparece |
| :--- | :--- | :--- | :--- |
| **C01** | Catálogo de Cortinas | Lista de cards | Home, Busca de Produtos |
| **C02** | Detalhes do Produto | Texto + Imagem | Página do Produto |
| **C03** | Formulário de Login/Cadastro | Campos de entrada | Tela de Autenticação (RF1, RF2) |
| **C04** | Painel de Gestão (Vendedor) | Lista de itens | Área do Vendedor (RF3) |
| **C05** | Chat/Comunicação | Interface de texto | Central de Atendimento (RN1) |

### Agrupamento Lógico:
* **Área Pública:** Vitrine de produtos, busca e telas de login/registro.
* **Área do Cliente:** Meus Pedidos, Carrinho de Compras e Mensagens com Vendedor.
* **Área do Vendedor:** Cadastro de Produtos (RF3), Vínculo com Empresa (RN3) e Gestão de Vendas.

---

## 3. Sistemas de Organização e Navegação

### Sitemap (Estrutura):
```text
HOME (Vitrine de Cortinas)
├── Categorias
├── Buscar Produtos
└── Login / Criar Conta

ÁREA DO CLIENTE (LOGADO)
├── Perfil (Dados Pessoais)
├── Minha Sacola (Carrinho)
├── Meus Pedidos (Histórico de Compras)
└── Falar com Vendedor (Chat)

ÁREA DO VENDEDOR (LOGADO)
├── Painel da Empresa (Vínculo institucional)
├── Gerenciar Estoque (Lista de Produtos)
│   └── + Adicionar Novo Produto (Cortina)
└── Mensagens de Clientes
```
## 4. Sistemas de Rotulagem (Nomenclaturas)

| Nome Técnico (Rota) | Nome para o Usuário | Justificativa |
| :--- | :--- | :--- |
| `/auth` | **Entrar ou Criar Conta** | Termo padrão para os processos de Autenticação (RF1) e Registro (RF2). |
| `/produtos` | **Nossas Cortinas** | Linguagem direta focada no nicho do produto principal. |
| `/cadastro-item` | **Anunciar Produto** | Verbo de ação focado na tarefa do Vendedor de expandir o estoque (RF3). |
| `/carrinho` | **Minha Sacola** | Termo amigável para o fluxo de compra do cliente (RF4). |
| `/chat` | **Falar com Vendedor** | Indica claramente o canal de gerência de comunicação (RN1). |

---

## 5. Fluxos de Usuário (Coreografia)

### Fluxo Principal: Compra de Cortina (Cliente)

1.  **Acesso e Identificação:**
    * O usuário acessa a tela de Login/Cadastro.
    * Realiza a autenticação para liberar as funcionalidades de compra (RF1, RF2).

2.  **Seleção de Produto:**
    * O Cliente navega pela lista de produtos cadastrados pelos vendedores.
    * Escolhe o item desejado (garantindo que o vendedor tenha produtos disponíveis - RN2).

3.  **Finalização da Compra:**
    * O Cliente adiciona o item à sacola e confirma a transação (RF4).

4.  **Pós-Venda e Suporte:**
    * O sistema habilita o canal de comunicação direta entre o Cliente e o Vendedor responsável (RN1).

---

## 6. Modelagem de Dados

### Entidade: Usuários (RF1, RF2)
* **e-mail**: Identificador único para login.
* **senha**: Código de acesso seguro.
* **perfil**: Define se é Vendedor ou Cliente.

### Entidade: Vendedores (RN3)
* **ID_Vendedor**: Identificador único.
* **Nome**: Nome do profissional.
* **Empresa_Vinculada**: Empresa à qual o vendedor pertence (Obrigatório - RN3).

### Entidade: Produtos / Cortinas (RF3, RN2)
* **ID_Produto**: Identificador único.
* **Nome_Produto**: Nome comercial da cortina.
* **Descricao**: Detalhes técnicos e medidas.
* **Preco**: Valor unitário.
* **ID_Vendedor**: Referência ao vendedor que possui o produto (RN2).

### Entidade: Compras / Pedidos (RF4, RN1)
* **ID_Pedido**: Identificador da transação.
* **ID_Cliente**: Referência ao comprador.
* **ID_Produto**: Item adquirido.
* **Status_Comunicacao**: Gatilho para gerência de contato entre as partes (RN1).

---

## Fluxo de Autenticação e Direcionamento (RF1, RF2)
### graph TD
- A[Início] --> B{Possui conta?}
- B -- Não --> C[Registro de Usuário - RF2]
- C --> D[Definir Tipo: Vendedor ou Cliente]
- D --> E[Login - RF1]
- B -- Sim --> E
- E --> F{Tipo de Usuário?}
- F -- Vendedor --> G[Painel de Vendas / Empresa]
- F -- Cliente --> H[Vitrine de Produtos]

## Fluxo do Vendedor: Cadastro de Produtos (RF3, RN2, RN3)
### graph LR
- A[Login Vendedor] --> B{Vinculado à Empresa? - RN3}
- B -- Não --> C[Solicitar Vínculo Empresarial]
- C --> B
- B -- Sim --> D[Acessar 'Meus Produtos']
- D --> E[Cadastrar Nova Cortina - RF3]
- E --> F[Produto disponível na Vitrine - RN2]

## Fluxo do Cliente: Compra e Comunicação (RF4, RN1)
### graph TD
- A[Home / Vitrine] --> B[Selecionar Cortina]
- B --> C[Adicionar ao Carrinho]
- C --> D[Finalizar Compra - RF4]
- D --> E[Gerar Confirmação]
- E --> F[Abrir Chat com Vendedor - RN1]
- F --> G[Fim do Processo]
