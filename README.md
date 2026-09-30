# Teste Técnico — Tech Lead Fullstack Node.js e React

Empresa: Namu (Saúde e Bem-Estar)
Nível: Tech Lead / Coordenação Técnica
Tempo estimado: 4 a 6 horas
Prazo de entrega: 7 dias corridos
Entrega: repositório público no GitHub

---

## Sobre a Namu

A Namu é uma empresa de saúde e bem-estar. Este teste simula um cenário real de desenvolvimento na empresa, cobrindo backend, frontend e a engenharia em volta deles.

Buscamos uma pessoa que escreva código e que também decida bem. Mais do que a quantidade de funcionalidades, vamos olhar como você modela as regras de negócio, protege a integridade dos dados, organiza o projeto para que outras pessoas consigam evoluí-lo e explica as decisões que tomou.

### Sobre o uso de IA

Não é permitido usar ferramentas de IA como Copilot, ChatGPT ou Claude neste teste. O objetivo é avaliar raciocínio lógico e capacidade técnica real. Na entrevista, o time da Namu vai avaliar com base no projeto entregue.

---

## Projeto: Agendamento de sessões com especialistas

Desenvolva uma aplicação para que usuários agendem sessões com especialistas de bem-estar (psicologia, nutrição, educação física e yoga). O backend fornece a API com as regras de agendamento e o frontend permite que o usuário encontre um horário, agende e gerencie suas sessões.

### O que fornecemos

Um `docker-compose.yml` com MySQL, um `.env.example` e um script SQL de seed com profissionais, usuários, horários de disponibilidade e alguns agendamentos já existentes. As datas do seed são relativas ao dia em que o banco é criado.

O schema do seed é o ponto de partida. Você pode e deve evoluí-lo por migrations versionadas (índices, constraints, colunas novas), sem editar o `seed.sql`.

A autenticação está fora do escopo: identifique o usuário da requisição pelo header `X-User-Id`.

### Parte 1: Backend (API)

1. Profissionais (`GET /professionals`)
   - Lista os profissionais disponíveis para agendamento
   - Filtro opcional por especialidade (`?specialty=nutricao`)

2. Horários disponíveis (`GET /professionals/:id/slots?date=YYYY-MM-DD`)
   - Retorna os horários livres do profissional no dia informado

3. Agendamento (`POST /bookings`)
   - Corpo: `{ "slotId": 4 }`
   - Regras:
     - Só é possível agendar horários futuros, de profissionais ativos
     - Um horário não pode ter mais de um agendamento confirmado, inclusive quando duas pessoas tentam agendar o mesmo horário ao mesmo tempo
     - O usuário não pode ter duas sessões confirmadas em horários que se sobrepõem, mesmo com profissionais diferentes

4. Cancelamento (`DELETE /bookings/:id`)
   - Só o dono do agendamento pode cancelar
   - O cancelamento só é permitido até 24 horas antes do início da sessão
   - Ao cancelar, o horário volta a ficar disponível

5. Minhas sessões (`GET /me/bookings`)
   - Lista as sessões do usuário, separando próximas e passadas

Os horários estão gravados em UTC. O parâmetro `date` e a exibição de horários devem considerar o fuso de São Paulo (`America/Sao_Paulo`).

Erros de regra de negócio devem retornar status HTTP adequados, com mensagens claras.

### Parte 2: Frontend

Escolha **uma** plataforma, React (web) ou React Native (bare workflow, sem Expo managed), e justifique a escolha no README.

1. Encontrar e agendar
   - Escolha do profissional (com filtro por especialidade), da data e de um horário livre
   - Confirmação do agendamento, com as mensagens de erro da API exibidas de forma clara

2. Minhas sessões
   - Próximas e passadas
   - Cancelamento, deixando claro para o usuário quando ele não é mais permitido

### Requisitos técnicos

- Backend: Node.js com TypeScript, NestJS (preferencial) ou Express, MySQL
- ORM: TypeORM ou Sequelize, com migrations versionadas
- Frontend: React ou React Native, com TypeScript
- Comunicação via API REST
- Validação de dados na API e nos formulários
- ESLint + Prettier configurados
- Testes com Jest: unitários das regras de negócio e ao menos um teste de integração da API contra o banco
- Docker Compose que sobe o backend completo (banco e API) com um único comando
- GitHub Actions rodando lint e testes a cada push
- Git: ao menos uma funcionalidade (por exemplo, o cancelamento) entregue por Pull Request no próprio repositório, com uma descrição escrita para a revisão do time

### Documentação esperada

Além das instruções de execução, o README deve ter:

- **Decisões técnicas:** ao menos três decisões relevantes, com as alternativas consideradas e por que escolheu cada uma (por exemplo: como garantiu que um horário não recebe dois agendamentos, como tratou o fuso horário, como organizou os módulos)
- **Caminho para produção:** o que faltaria para colocar a aplicação em produção com segurança (observabilidade, segurança, escala, deploy) e em que ordem você atacaria

### Diferenciais (não obrigatórios)

- Documentação da API (OpenAPI/Swagger)
- Testes de integração cobrindo a disputa pelo mesmo horário
- Testes no frontend
- Logs estruturados e tratamento centralizado de erros
- Esboço de infraestrutura na AWS (texto ou diagrama no README)
- Conhecimento em PHP/Laravel (ex: menção no README de como integraria com uma API Laravel existente)

### Critérios de avaliação

| Critério | Peso |
|----------|------|
| Funcionamento end-to-end (API + Frontend) | 20% |
| Regras de negócio e integridade dos dados | 20% |
| Arquitetura e qualidade do código | 20% |
| Testes e CI | 15% |
| Decisões técnicas e documentação (README e Pull Request) | 15% |
| Frontend e experiência do usuário | 10% |

---

## Fluxo do processo

1. Teste take-home: você recebe este documento, desenvolve a solução e envia o link do repositório GitHub em até 7 dias
2. Avaliação técnica: time da Namu avalia o código entregue com base nos critérios deste documento
3. Entrevista com a Namu: baseada no projeto entregue, o time avalia raciocínio, entendimento da solução, as decisões que você tomou e o que você sabe fazer de fato

---

## Instruções de entrega

Crie um repositório público no GitHub com um `README.md` que contenha: descrição do projeto e funcionalidades implementadas, tecnologias utilizadas e por que as escolheu, instruções de instalação e execução, as decisões técnicas, o caminho para produção e o que faria diferente com mais tempo.

### Prazo

7 dias corridos a partir do recebimento do teste.

### O que valorizamos

Regras de negócio corretas e dados íntegros importam mais do que quantidade de features. Decisões explicadas com clareza mostram como você conduziria um time. Commits bem escritos e um Pull Request bem descrito mostram processo de pensamento. Separação de responsabilidades, tratamento de erros e testes das regras críticas contam bastante. O projeto precisa rodar seguindo as instruções do README, sem ajustes.

### O que não valorizamos

Over-engineering para o escopo proposto. Regras de negócio garantidas só no frontend. Código sem tratamento de erros. Repositório com um commit gigante. README genérico ou vazio. Projeto que não roda.

---

## Rubrica de avaliação

| Nota | Classificação | Descrição |
|------|--------------|-----------|
| 9-10 | Excelente | Atende todos os requisitos, regras de negócio corretas em todos os casos, decisões bem fundamentadas |
| 7-8 | Bom | Atende os requisitos principais, código organizado, poucas falhas nas regras |
| 5-6 | Satisfatório | Funciona parcialmente, regras críticas incompletas ou decisões pouco explicadas |
| 3-4 | Insuficiente | Muitas falhas, código desorganizado, requisitos principais incompletos |
| 0-2 | Eliminatório | Não funciona, plágio evidente, ou entrega vazia |

Nota mínima para aprovação: 7.0

### O que diferencia uma entrega de Tech Lead

| Aspecto | Abaixo do esperado | Esperado para Tech Lead |
|---------|-------------------|------------------------|
| Integridade | Regras validadas só com consultas antes de gravar | Regras garantidas também no banco ou por transação, resistentes a requisições simultâneas |
| Modelagem | Usa o schema como veio | Evolui o schema por migrations, com índices e constraints justificados |
| Arquitetura | Tudo no controller | Camadas claras, regras de negócio isoladas e testáveis |
| Testes | Alguns testes unitários | Regras críticas cobertas, integração contra o banco rodando no CI |
| Comunicação | README com instruções de execução | Decisões com alternativas e trade-offs, PR que outra pessoa conseguiria revisar |
| Visão de produto | Implementa o que foi pedido | Explicita riscos, limites da solução e próximos passos |
