# Entendendo sobre Segurança e Identidade na Azure

Este repositório reúne as anotações do desafio da **DIO (Digital Innovation One)** sobre **identidade, acesso e segurança no Microsoft Azure**. O objetivo é compreender como proteger recursos e dados com autenticação adequada, permissões mínimas, controles de rede e governança.

> Material de estudo: os exemplos abaixo são conceituais e não afirmam que recursos foram provisionados no laboratório.

## Objetivo do desafio

- Entender a diferença entre autenticação e autorização.
- Conhecer o Microsoft Entra ID e os mecanismos de proteção de contas.
- Controlar acesso a recursos Azure com RBAC.
- Aplicar princípios de Zero Trust e defesa em profundidade.
- Compreender segurança de rede, proteção de segredos e governança.
- Identificar ferramentas de monitoramento e boas práticas.

## Conceitos estudados

### 1. Microsoft Entra ID

Serviço de identidade e acesso em nuvem (antigo Azure Active Directory). Centraliza usuários, grupos, aplicações e políticas de autenticação. Não é o mesmo serviço que o Active Directory Domain Services tradicional.

### 2. Autenticação e autorização

Autenticação verifica **quem é** o usuário ou aplicação; autorização define **o que pode fazer**. Por exemplo, fazer login no portal não implica permissão para excluir uma máquina virtual.

### 3. SSO e MFA

**Single Sign-On (SSO)** permite acessar vários aplicativos com uma sessão de identidade. **Autenticação multifator (MFA)** adiciona uma verificação independente, reduzindo o risco do uso de senhas comprometidas.

### 4. Controle de acesso baseado em funções (Azure RBAC)

O **Azure RBAC** concede permissões para recursos Azure com papéis e escopos. Papéis comuns incluem Reader, Contributor e Owner. As permissões podem ser atribuídas nos níveis de grupo de gerenciamento, assinatura, grupo de recursos e recurso. Deve-se aplicar o **menor privilégio**.

### 5. Acesso Condicional

Políticas do **Microsoft Entra Conditional Access** combinam sinais como identidade, dispositivo, localização e risco para exigir MFA, bloquear um acesso ou aplicar outros controles. A disponibilidade de recursos depende do licenciamento.

### 6. Identidades gerenciadas

As **Managed Identities** permitem que aplicações em serviços compatíveis da Azure obtenham tokens para acessar recursos sem armazenar senhas ou chaves no código. Podem ser atribuídas pelo sistema ou pelo usuário.

### 7. Zero Trust e defesa em profundidade

A abordagem **Zero Trust** adota verificação explícita, menor privilégio e pressuposição de violação. A **defesa em profundidade** distribui proteções entre identidade, perímetro, rede, computação, aplicação e dados.

### 8. Proteção da infraestrutura

**Network Security Groups (NSG)** controlam tráfego de rede; **Azure Firewall** oferece filtragem centralizada; **Azure DDoS Protection** ajuda a mitigar ataques de negação de serviço distribuídos. Eles têm funções diferentes e não substituem políticas de identidade.

### 9. Microsoft Defender for Cloud

Ferramenta de gerenciamento da postura de segurança e proteção de workloads, com recomendações, avaliação de configurações e recursos de detecção conforme os planos habilitados.

### 10. Governança com Azure Policy

O **Azure Policy** avalia e pode impor regras de conformidade, como restringir regiões ou exigir tags. Ele complementa o RBAC: RBAC controla ações de identidades; Policy controla a conformidade da configuração dos recursos.

### 11. Segredos e chaves com Azure Key Vault

Serviço para armazenar e gerenciar segredos, chaves criptográficas e certificados. O acesso deve ser concedido apenas aos serviços e usuários necessários, preferencialmente via Microsoft Entra e RBAC quando aplicável.

### 12. Monitoramento e auditoria

Logs de atividades da Azure registram operações no plano de controle. Logs de entrada do Microsoft Entra auxiliam na investigação de autenticações. Azure Monitor e Log Analytics ajudam a consultar dados e criar alertas, conforme a configuração.

## Exemplo prático de controle de acesso

Imagine uma equipe com uma aplicação em uma máquina virtual e um banco de dados no Azure:

```text
Usuário → Microsoft Entra ID (login + MFA)
             ↓
         Azure RBAC
             ↓
        Resource Group
        ├── VM (acesso restrito por NSG)
        ├── Banco de Dados
        └── Key Vault (segredos)
             ↓
       Azure Monitor / Logs
```

Um analista que só precisa consultar o ambiente pode receber **Reader** no grupo de recursos. Uma aplicação que precisa ler um segredo pode utilizar uma **identidade gerenciada** com permissão limitada ao segredo necessário. Isso reduz permissões excessivas e evita credenciais fixas no código.

## Comparação rápida

| Solução | Pergunta que resolve |
| --- | --- |
| Microsoft Entra ID | Quem está tentando acessar? |
| MFA / Acesso Condicional | Em quais condições permitir a autenticação? |
| Azure RBAC | Quais operações essa identidade pode realizar? |
| Azure Policy | Os recursos seguem as regras definidas? |
| NSG / Azure Firewall | Qual tráfego de rede é permitido? |
| Key Vault | Como gerenciar chaves e segredos? |
| Defender for Cloud | Como avaliar e melhorar a postura de segurança? |
| Azure Monitor | Como observar eventos e investigar problemas? |

## Boas práticas

1. Exigir MFA, especialmente para contas privilegiadas.
2. Usar papéis de menor privilégio e revisar concessões periodicamente.
3. Preferir identidades gerenciadas a credenciais gravadas em aplicações.
4. Não expor portas administrativas à internet sem necessidade.
5. Centralizar segredos no Key Vault e configurar acesso restrito.
6. Aplicar políticas de governança e acompanhar não conformidades.
7. Manter sistemas e componentes atualizados.
8. Habilitar logs relevantes, alertas e processos de investigação.
9. Planejar procedimentos de recuperação e resposta a incidentes.

## Conclusão

Segurança na nuvem é uma responsabilidade compartilhada. A Microsoft protege a infraestrutura subjacente dos serviços, enquanto a organização deve configurar corretamente identidades, permissões, dados e aplicações conforme o modelo de serviço utilizado.

O aprendizado central deste desafio é que **segurança e identidade devem orientar as decisões de arquitetura desde o início**, em vez de serem tratadas apenas após o provisionamento.

## Referências

- [Microsoft Entra ID](https://learn.microsoft.com/pt-br/entra/fundamentals/whatis)
- [Azure RBAC](https://learn.microsoft.com/pt-br/azure/role-based-access-control/overview)
- [Acesso Condicional](https://learn.microsoft.com/pt-br/entra/identity/conditional-access/overview)
- [Identidades gerenciadas](https://learn.microsoft.com/pt-br/entra/identity/managed-identities-azure-resources/overview)
- [Azure Policy](https://learn.microsoft.com/pt-br/azure/governance/policy/overview)
- [Azure Key Vault](https://learn.microsoft.com/pt-br/azure/key-vault/general/overview)
- [Microsoft Defender for Cloud](https://learn.microsoft.com/pt-br/azure/defender-for-cloud/defender-for-cloud-introduction)
- [Azure Monitor](https://learn.microsoft.com/pt-br/azure/azure-monitor/overview)

---

**Desafio de Projeto — DIO**
