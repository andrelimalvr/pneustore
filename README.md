# Diagrama de Arquitetura de TI - Empresa de Pneus

## Descrição do Cenário
A empresa de pneus possui um ambiente de TI híbrido, com uma aplicação de ecommerce que representa a maioria da receita anual e um ambiente on-premise para 250 funcionários.

## Ambiente de Ecommerce
- **AWS Cloud:**
  - Subnet Privada  
    - Hospeda a backend e banco de dados do ecommerce, acessível apenas para a empresa.
    - Tecnologias:
        - Backend: .NET Core.
    - Banco de Dados:
        - MongoDB em replicaSet.
  - Subnet Publica
    - Hospeda o Frontend do ecommerce para o publico
    - Tecnologias
        - Frontend: Node.js.

### Descrição da Resolução - Cloud
- Apenas essa aplicação do node.js precisa ser acessada por toda a internet,  já o resto estará privado para garantir a confiabilidade, integridade e disponibilidade nas aplicações na subnet privada
- Para solução de DRP (Disaster Recovery Plan), cada BD (banco de dados) está alocado em DCs (Data centers) diferentes, como mostra que a zona difere da outra, mantendo assim a resiliencia de dados
          
## Ambiente On-premise
- **Servidores Virtualizados com VMWare:**
    - Virtualiza 3 servidores:
        1. FileServer. 3 Servidores
        2. Active Directory local. 3 Servidores
        3. PrintServer. 2 Servidores
        4. SMTP Server - Email - 2 Servidores

### Descrição da Resolução - On-premisse
Para resolver o problema dos servidores criticos, foi criado uma DMZ interna na qual o acesso é limitado pelo Firewall, apenas a VLAN da TI é autorizada a ter acesso a esta rede para motivos de manutenção e gestão dos servidores da TI, 
para toda outra VLAN, o acesso é zero trust, necessitando de um chamado de liberação de firewall para que o colaborador de outro setor possa ter acesso, descrevendo qual servidor, motivo do acesso e tendo autorização do seu Gestor e
o Gestor da TI.
Foi criado mais de um servidor cada para ter redundancia de dados de forma clusterizada

## Comunicação
- **Principal Ferramenta de Comunicação:**
    - Email.
 
### Descrição da Resolução - Email
O email começou a ser gerenciado pela TI com a seguinte proposta:
- Quando o dominio é interno, da empresa, não precisa de autorização para mandar email, 
- Quando o dominio é externo, vai enviar email para outro dominio que seja, vai passar por uma autorização do gestor do setor, ele podendo ler todo o email que você elaborou, para decidir se aprova ou não!
- Solução proposta para evitar vazamento de dados

## Armazém
- **Armazenamento de Estoque:**
    - O armazém da empresa mantém todo o estoque de pneus que é revendido na região.
    - Controle desses estoques é gerido pela logistica da empresa com a aplicação SAP, podendo ser o TOTVS, solução não incluída no diagrama, mas citada no md.

