# Documento de Especificação de Requisitos (DER)
**Projeto:** Evolução do Sistema de Gestão Pet (Integração de IA e Atendimento Automatizado)
**Analista Responsável:** [Gerado via Assistente]
**Objetivo:** Implementar melhorias baseadas em Inteligência Artificial para automatizar o atendimento via WhatsApp, orçamentação dinâmica, upselling e gestão inteligente de agenda, espelhando os diferenciais competitivos do mercado de ponta.

---

## 1. VISÃO GERAL DAS MELHORIAS
O sistema atual receberá uma nova camada de **Front-end de Atendimento (IA Conversacional)** e um **Motor de Agendamento Baseado em Regras**. O foco deixa de ser apenas a retaguarda (back-office) e passa a ser a conversão de vendas, automação da agenda e experiência do cliente (tutor) via WhatsApp.

---

## 2. ÉPICOS E REQUISITOS FUNCIONAIS (RF)

### ÉPICO 1: Assistente Virtual Conversacional (WhatsApp)
**Descrição:** Substituir menus numéricos (URAs/Chatbots tradicionais) por um agente de IA capaz de interpretar linguagem natural.

*   **RF01 - Processamento de Linguagem Natural (Texto):** O sistema deve interpretar mensagens de texto abertas do cliente (ex: "Quero dar banho no meu cachorro sábado") e extrair a intenção (agendamento), serviço (banho) e data (sábado).
*   **RF02 - Reconhecimento e Transcrição de Áudio:** O sistema deve receber áudios enviados pelo WhatsApp, transcrevê-los para texto internamente, e processar a intenção com a mesma precisão do texto.
*   **RF03 - Transbordo Inteligente (Human-in-the-loop):** Se a IA não identificar a intenção ou o cliente solicitar, o atendimento deve ser transferido para um atendente humano. 
    *   *Detalhe:* A interface do atendente (Dashboard) deve carregar todo o histórico da conversa (contexto) para que o cliente não precise repetir as informações.

### ÉPICO 2: Motor de Agendamento e Precificação Dinâmica
**Descrição:** O sistema não pode ter preços estáticos. A IA deve calcular o serviço com base nas características do animal.

*   **RF04 - Cadastro Complexo de Pets:** O cadastro do pet deve conter campos obrigatórios para o cálculo de serviços: Espécie, Raça, Porte (P, M, G, GG), Tipo de Pelagem (Curta, Longa, Dupla, etc.) e Temperamento.
*   **RF05 - Precificação e Duração Variável:** O sistema deve cruzar o serviço desejado (ex: Banho) com os dados do Pet.
    *   *Exemplo:* Banho em Shih Tzu (Pequeno, Pelo Longo) = R$ 60,00 e 60 minutos. Banho em Golden Retriever (Grande, Pelo Duplo) = R$ 120,00 e 120 minutos.
*   **RF06 - Alocação Inteligente de Profissionais:** O sistema deve cruzar a duração calculada com a agenda livre dos profissionais, respeitando as habilidades de cada um (ex: Profissional X faz tosa na tesoura, Profissional Y só faz banho).
*   **RF07 - Inserção Direta na Agenda:** Após a aprovação do tutor via WhatsApp, a IA deve bloquear o slot de horário no sistema automaticamente, sem intervenção humana.

### ÉPICO 3: Motor de Vendas e Upsell (IA Vendedora)
**Descrição:** A IA deve atuar ativamente para aumentar o ticket médio dos atendimentos.

*   **RF08 - Sugestão Contextual de Serviços (Cross-sell/Upsell):** Baseado na raça/pelagem ou no histórico do pet, a IA deve sugerir serviços extras. 
    *   *Exemplo:* Ao agendar um banho para um Spitz Alemão, a IA deve sugerir: "Notei que faz 3 meses desde a última hidratação do [Nome do Pet]. Quer aproveitar e incluir uma máscara de hidratação por mais R$ X?"
*   **RF09 - Conversão para Assinaturas/Pacotes:** Quando o cliente pedir um serviço avulso, a IA deve ser capaz de oferecer a conversão para um pacote mensal com desconto.
*   **RF10 - Contorno de Objeções:** O sistema deve ter um prompt configurado para responder a objeções de preço, destacando o valor agregado (shampoo premium, toalhas esterilizadas, etc.).

### ÉPICO 4: Gestão de Retenção e Anti No-Show
**Descrição:** Reduzir buracos na agenda por esquecimento ou falta dos clientes.

*   **RF11 - Lembretes Interativos:** Enviar mensagem de WhatsApp X horas antes do serviço (configurável) com opções claras de confirmação (Ex: "Sim, confirmo" ou "Preciso reagendar").
*   **RF12 - Reestruturação Automática de Agenda:** Se o cliente cancelar/reagendar via IA, o horário original deve ser imediatamente marcado como "Livre" no sistema, permitindo que a IA o ofereça para o próximo cliente.

### ÉPICO 5: Operação de Retaguarda e Integrações
**Descrição:** Garantir que o back-office funcione perfeitamente com foco no serviço, delegando obrigações fiscais complexas.

*   **RF13 - Painel de Execução (Checklist):** Tela exclusiva para os tosadores/banhistas. Ao abrir o serviço do dia, deve exibir o checklist específico do pet (ex: alergias, tipo de tosa específica, restrições).
*   **RF14 - API de Integração com ERPs:** O sistema deve possuir Webhooks e APIs para enviar os dados de faturamento e vendas consolidadas para ERPs de mercado (Bling, ContaAzul, VHSYS), a fim de que a emissão de Nota Fiscal e controle de fluxo de caixa complexo ocorram nestas ferramentas.

---

## 3. REGRAS DE NEGÓCIO (RN)

*   **RN01 - Bloqueio de Segurança de Horário:** A IA não pode sugerir horários que tenham menos de 15 minutos de margem entre um serviço longo e o próximo, para evitar atrasos em cascata.
*   **RN02 - Prioridade de Fidelidade:** Em caso de horários concorridos (ex: sábados de manhã), a IA deve priorizar clientes que possuem pacotes/assinaturas ativas, oferecendo esses horários primeiro para eles.
*   **RN03 - Autorização de Transbordo:** O sistema só transfere para um humano em horário comercial. Fora do horário comercial, a IA deve registrar a solicitação, informar que a equipe humana entrará em contato no próximo dia útil e criar um "Card" de pendência no dashboard.

---

## 4. FLUXO IDEAL DE CONVERSA (Exemplo para Prompting)
1. **Cliente:** "Oi, queria marcar tosa pro Bidu." *(Áudio)*
2. **Sistema:** Transcreve áudio. Busca "Bidu" no banco de dados atrelado ao número de telefone.
3. **IA (WhatsApp):** "Olá, [Nome do Tutor]! Claro, vamos cuidar do Bidu. Como ele é um Poodle (Porte M), a Tosa Padrão + Banho fica R$ 85,00 e leva cerca de 1h30. Tenho horário amanhã às 10h ou às 14h. Qual prefere?"
4. **Cliente:** "Pode ser às 10h. Faz por 70?"
5. **IA (WhatsApp):** "Infelizmente não consigo alterar o valor avulso, pois usamos produtos premium hipoalergênicos e toalhas esterilizadas. Mas se você fechar o pacote mensal, cada banho e tosa sai por R$ 75,00! Quer agendar o avulso das 10h ou prefere que eu te explique o pacote?"
6. **Cliente:** "Pode ser o avulso mesmo."
7. **IA (WhatsApp):** "Combinado! Bidu agendado para amanhã às 10h. Te mando um lembrete logo cedo. Até lá!" *(Ação interna: Bloqueia a agenda do profissional habilitado às 10h).*