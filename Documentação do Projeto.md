# Documentação do Projeto — Valorant Team Manager (VTM)

**Disciplina:** Análise e Projeto de Sistemas (APS)
**Aluno:** Olavo Appel Neto

## Sumário

1. [Descrição e delimitação do escopo](#1-descrição-e-delimitação-do-escopo)
2. [Fase de análise](#2-fase-de-análise)
3. [Fase de projeto](#3-fase-de-projeto)
4. [Protótipo do sistema](#4-protótipo-do-sistema)
5. [Considerações finais](#5-considerações-finais)
6. [Referências](#6-referências)

---

## 1. Descrição e delimitação do escopo

### 1.1 Cenário

O Valorant é um FPS tático 5x5 em que duas equipes se alternam entre ataque e defesa em diferentes mapas. Cada jogador escolhe um agente (personagem) de uma das quatro funções: duelista, iniciador, controlador ou sentinela. Em campeonatos, o desempenho de um time depende muito da preparação: quais agentes cada jogador domina, qual composição de agentes é a mais forte em cada mapa e quanto o time treina contra adversários.

O **Valorant Team Manager (VTM)** é um sistema web que apoia essa preparação, reunindo em um só lugar o elenco, as composições por mapa, os treinos e a participação em campeonatos.

### 1.2 Propósito

Organizar a rotina competitiva de uma equipe de Valorant, substituindo planilhas e mensagens soltas em aplicativos de conversa por um registro único, consultável e padronizado.

### 1.3 Contexto de uso

O sistema é usado no dia a dia de times amadores, universitários e semiprofissionais que disputam campeonatos. O treinador define composições e marca treinos; os jogadores consultam suas funções, agentes e agenda; o manager mantém o cadastro do time e sua inscrição em campeonatos.

### 1.4 Público-alvo

- **Treinador (coach):** monta composições, agenda treinos e acompanha o desempenho do time.
- **Jogador:** consulta composições, agenda e confirma presença; mantém seus agentes principais.
- **Manager (Gerente do time):** administra o elenco e inscreve o time em campeonatos.

### 1.5 Problema que o sistema resolve

- As composições por mapa ficam espalhadas em planilhas, imagens e conversas, sem versão única.
- Não há controle claro de quais agentes cada jogador domina, o que gera composições impossíveis de executar.
- Combinar treinos com outros times é feito de forma manual e desorganizada, com conflitos de horário e faltas.
- O histórico de treinos e partidas não é registrado, dificultando a análise de evolução do time.

### 1.6 Funcionalidades contempladas

- Cadastro e gerenciamento do time e do elenco (jogadores, função e papel no time).
- Registro dos agentes que cada jogador domina.
- Montagem de composições de agentes por mapa, associando um agente a cada jogador.
- Agendamento de treinos (scrims) com outros times, com convite, aceite ou recusa e confirmação de presença dos jogadores.
- Registro do resultado e de observações dos treinos.
- Cadastro de campeonatos e inscrição do time, com o conjunto de mapas disputados.
- Registro das partidas do campeonato e dos resultados.
- Autenticação e controle de acesso por perfil (treinador, jogador, manager).

### 1.7 Fora do escopo

- Integração com a API oficial da Riot Games ou importação automática de estatísticas.
- Transmissão ao vivo, chat em tempo real e comunicação por voz.
- Gestão financeira, patrocínios e contratos.
- Organização de campeonatos (chaveamento e arbitragem); o sistema apenas registra a participação do time.

### 1.8 Atores identificados (preliminar)

| Ator | Descrição |
|------|-----------|
| Treinador | Responsável técnico; define composições e organiza treinos |
| Jogador | Membro do elenco; consulta informações e confirma presença |
| Manager | Administra o time e as inscrições em campeonatos |

---

## 2. Fase de análise

### 2.1 Requisitos funcionais

| ID | Requisito | Ator |
|----|-----------|------|
| RF01 | O sistema deve permitir que o usuário se autentique com e-mail e senha e acesse apenas as funcionalidades do seu perfil (treinador, jogador ou manager). | Todos |
| RF02 | O sistema deve permitir que o manager cadastre e edite os dados do time (nome, tag e logo). | Manager |
| RF03 | O sistema deve permitir que o manager adicione e remova jogadores do elenco, informando nickname, função principal e papel no time (titular ou reserva). | Manager |
| RF04 | O sistema deve permitir que o jogador registre os agentes que domina, informando o nível de proficiência de cada um (básico, intermediário ou avançado). | Jogador |
| RF05 | O sistema deve permitir que o treinador crie uma composição para um mapa, associando um agente a cada jogador titular. | Treinador |
| RF06 | O sistema deve permitir que o treinador edite, exclua e consulte as composições do time, filtrando por mapa. | Treinador |
| RF07 | O sistema deve permitir que o treinador agende um treino (scrim) com outro time cadastrado, informando data, horário, mapas e formato (MD1, MD2 ou MD3). | Treinador |
| RF08 | O sistema deve permitir que o manager do time convidado aceite ou recuse o convite de treino recebido. | Manager |
| RF09 | O sistema deve permitir que o jogador confirme ou recuse sua presença em um treino ou partida agendados. | Jogador |
| RF10 | O sistema deve permitir que o treinador registre o resultado de um treino (placar por mapa) e observações sobre o desempenho do time. | Treinador |
| RF11 | O sistema deve permitir que o manager cadastre um campeonato (nome, datas e mapas disputados) e inscreva o time nele. | Manager |
| RF12 | O sistema deve permitir que o treinador registre as partidas de um campeonato (adversário, data, mapa e resultado). | Treinador |
| RF13 | O sistema deve exibir ao jogador a agenda do time, com os treinos e as partidas futuras e o status de sua confirmação. | Jogador |
| RF14 | O sistema deve permitir consultar o histórico de treinos e partidas, filtrando por período, adversário e mapa. | Treinador, Manager |
| RF15 | O sistema deve permitir que o treinador cancele um treino agendado, notificando o time adversário e os jogadores. | Treinador |

### 2.2 Requisitos não funcionais

| ID | Categoria | Requisito |
|----|-----------|-----------|
| RNF01 | Desempenho | As consultas (agenda, composições e histórico) devem responder em até 3 segundos em 95% das requisições, com até 100 usuários simultâneos. |
| RNF02 | Usabilidade | O sistema deve ser responsivo e utilizável em computadores e celulares, e um usuário novo deve conseguir consultar sua agenda sem treinamento prévio. |
| RNF03 | Segurança | As senhas devem ser armazenadas com hash e toda comunicação deve usar HTTPS. |
| RNF04 | Segurança | O acesso às funcionalidades deve ser controlado por perfil, e um usuário só pode ver e alterar dados do seu próprio time. |
| RNF05 | Manutenibilidade | O sistema deve ser organizado em camadas (apresentação, regras de negócio e persistência), de modo que uma alteração em uma camada não exija mudanças nas demais. |
| RNF06 | Disponibilidade | O sistema deve estar disponível em pelo menos 95% do tempo, medido mensalmente. |
| RNF07 | Compatibilidade | O sistema deve funcionar nas duas últimas versões dos navegadores Chrome, Firefox e Edge. |
| RNF08 | Confiabilidade | Os dados devem ter backup automático diário, com possibilidade de restauração. |

### 2.3 Regras de negócio

| ID | Regra |
|----|-------|
| RN01 | Uma composição deve ter exatamente 5 jogadores titulares, cada um com um agente, e um mesmo agente não pode aparecer duas vezes na mesma composição. |
| RN02 | Um jogador só pode ser escalado em uma composição com um agente que conste na sua lista de agentes dominados. |
| RN03 | O elenco pode ter no máximo 5 titulares e 2 reservas, e um jogador só pode pertencer a um time ativo por vez. |
| RN04 | Um jogador não pode ser escalado nem confirmar presença em dois eventos (treinos ou partidas) com horários sobrepostos. |
| RN05 | Um treino só é considerado confirmado depois que o manager do time convidado aceita o convite, e não pode ser agendado para uma data passada. |
| RN06 | Um time só pode ser inscrito em um campeonato se tiver pelo menos 5 jogadores titulares cadastrados. |
| RN07 | O resultado de um treino ou partida só pode ser registrado depois da data e do horário do evento. |
| RN08 | Somente o treinador pode criar e alterar composições, agendar e cancelar treinos e registrar resultados, e somente o manager pode alterar o elenco, as inscrições em campeonatos e responder aos convites de treino. |

### 2.4 Documento de requisitos

#### Documento de Especificação de Requisitos

| | |
|---|---|
| **Projeto / Sistema** | Valorant Team Manager (VTM) |
| **Cliente / Solicitante** | Não se aplica (projeto acadêmico). Disciplina de Análise e Projeto de Sistemas, Prof. Mateus de Freitas Invernisi |
| **Responsável** | Olavo Appel Neto |
| **Data** | Novembro de 2026 |
| **Versão** | 1.0 |

#### 2.4.1 Introdução

##### 2.4.1.1 Sobre este documento

Este documento apresenta e organiza os requisitos levantados para o Valorant Team Manager (VTM), servindo de referência para as etapas de projeto, desenvolvimento, testes e validação.

##### 2.4.1.2 Descrição do Mini-Mundo

O Valorant é um jogo de FPS tático 5x5 em que duas equipes se alternam entre ataque e defesa em diferentes mapas. Cada jogador escolhe um agente (personagem) de uma das quatro funções: duelista, iniciador, controlador ou sentinela. Times amadores, universitários e semiprofissionais disputam campeonatos e, para se preparar, definem composições de agentes para cada mapa e realizam treinos (scrims) contra outros times.

Hoje esse processo é manual: as composições ficam em planilhas, imagens e conversas de Discord, os treinos são combinados por mensagens e não há registro organizado dos resultados.

Os principais envolvidos são o **treinador**, que monta composições e organiza treinos; o **jogador**, que informa os agentes que domina e confirma presença; e o **manager**, que administra o elenco e inscreve o time em campeonatos. As entidades relevantes são time, jogador, agente, mapa, composição, treino, campeonato e partida. As regras do domínio incluem composições com 5 jogadores e agentes não repetidos, e jogadores escalados apenas com agentes que dominam.

##### 2.4.1.3 Propósito do Sistema
Organizar a rotina competitiva de uma equipe de Valorant em um registro único, consultável e padronizado, reunindo elenco, agentes dominados, composições por mapa, treinos e participação em campeonatos. Espera-se reduzir composições inviáveis, conflitos de horário e faltas em treinos, além de manter o histórico para acompanhar a evolução do time.

#### 2.4.2 Requisitos Funcionais

| ID | Descrição | Prioridade | Depende de |
|----|-----------|------------|------------|
| RF01 | O sistema deve permitir que o usuário se autentique com e-mail e senha e acesse apenas as funcionalidades do seu perfil (treinador, jogador ou manager). | Alta | — |
| RF02 | O sistema deve permitir que o manager cadastre e edite os dados do time (nome, tag e logo). | Alta | RF01 |
| RF03 | O sistema deve permitir que o manager adicione e remova jogadores do elenco, informando nickname, função principal e papel no time (titular ou reserva). | Alta | RF01, RF02 |
| RF04 | O sistema deve permitir que o jogador registre os agentes que domina, informando o nível de proficiência de cada um (básico, intermediário ou avançado). | Alta | RF01, RF03 |
| RF05 | O sistema deve permitir que o treinador crie uma composição para um mapa, associando um agente a cada jogador titular. | Alta | RF03, RF04 |
| RF06 | O sistema deve permitir que o treinador edite, exclua e consulte as composições do time, filtrando por mapa. | Média | RF05 |
| RF07 | O sistema deve permitir que o treinador agende um treino (scrim) com outro time cadastrado, informando data, horário, mapas e formato (MD1, MD2 ou MD3). | Alta | RF02, RF03 |
| RF08 | O sistema deve permitir que o manager do time convidado aceite ou recuse o convite de treino recebido. | Alta | RF07 |
| RF09 | O sistema deve permitir que o jogador confirme ou recuse sua presença em um treino ou partida agendados. | Média | RF07, RF12 |
| RF10 | O sistema deve permitir que o treinador registre o resultado de um treino (placar por mapa) e observações sobre o desempenho do time. | Média | RF07 |
| RF11 | O sistema deve permitir que o manager cadastre um campeonato (nome, datas e mapas disputados) e inscreva o time nele. | Média | RF02 |
| RF12 | O sistema deve permitir que o treinador registre as partidas de um campeonato (adversário, data, mapa e resultado). | Média | RF11 |
| RF13 | O sistema deve exibir ao jogador a agenda do time, com os treinos e as partidas futuras e o status de sua confirmação. | Média | RF09 |
| RF14 | O sistema deve permitir consultar o histórico de treinos e partidas, filtrando por período, adversário e mapa. | Baixa | RF10, RF12 |
| RF15 | O sistema deve permitir que o treinador cancele um treino agendado, notificando o time adversário e os jogadores. | Baixa | RF07 |

#### 2.4.3 Requisitos Não Funcionais

| ID | Descrição | Categoria | Prioridade | Depende de |
|----|-----------|-----------|------------|------------|
| RNF01 | As consultas (agenda, composições e histórico) devem responder em até 3 segundos em 95% das requisições, com até 100 usuários simultâneos. | Desempenho | Alta | RF06, RF13, RF14 |
| RNF02 | O sistema deve ser responsivo e utilizável em computadores e celulares, e um usuário novo deve conseguir consultar sua agenda sem treinamento prévio. | Usabilidade | Média | RF13 |
| RNF03 | As senhas devem ser armazenadas com hash e toda comunicação deve usar HTTPS. | Segurança | Alta | RF01 |
| RNF04 | O acesso às funcionalidades deve ser controlado por perfil, e um usuário só pode ver e alterar dados do seu próprio time. | Segurança | Alta | RF01 |
| RNF05 | O sistema deve ser organizado em camadas (apresentação, regras de negócio e persistência), de modo que uma alteração em uma camada não exija mudanças nas demais. | Manutenibilidade | Média | — |
| RNF06 | O sistema deve estar disponível em pelo menos 95% do tempo, medido mensalmente. | Disponibilidade | Média | — |
| RNF07 | O sistema deve funcionar nas duas últimas versões dos navegadores Chrome, Firefox e Edge. | Compatibilidade | Média | — |
| RNF08 | Os dados devem ter backup automático diário, com possibilidade de restauração. | Confiabilidade | Média | — |

#### 2.4.4 Regras de Negócio

| ID | Descrição | Prioridade | Depende de |
|----|-----------|------------|------------|
| RN01 | Uma composição deve ter exatamente 5 jogadores titulares, cada um com um agente, e um mesmo agente não pode aparecer duas vezes na mesma composição. | Alta | RF05 |
| RN02 | Um jogador só pode ser escalado em uma composição com um agente que conste na sua lista de agentes dominados. | Alta | RF04, RF05 |
| RN03 | O elenco pode ter no máximo 5 titulares e 2 reservas, e um jogador só pode pertencer a um time ativo por vez. | Alta | RF03 |
| RN04 | Um jogador não pode ser escalado nem confirmar presença em dois eventos (treinos ou partidas) com horários sobrepostos. | Alta | RF09 |
| RN05 | Um treino só é considerado confirmado depois que o manager do time convidado aceita o convite, e não pode ser agendado para uma data passada. | Alta | RF07, RF08 |
| RN06 | Um time só pode ser inscrito em um campeonato se tiver pelo menos 5 jogadores titulares cadastrados. | Média | RF11 |
| RN07 | O resultado de um treino ou partida só pode ser registrado depois da data e do horário do evento. | Média | RF10, RF12 |
| RN08 | Somente o treinador pode criar e alterar composições, agendar e cancelar treinos e registrar resultados, e somente o manager pode alterar o elenco, as inscrições em campeonatos e responder aos convites de treino. | Alta | RF01 |

#### 2.4.5 Requisitos Suplementares

| ID | Descrição | Categoria | Prioridade | Depende de |
|----|-----------|-----------|------------|------------|
| RS01 | O sistema deve estar em conformidade com a LGPD, coletando apenas os dados pessoais necessários (nome, nickname e e-mail) e permitindo a exclusão da conta. | Legal | Média | RF01 |
| RS02 | O sistema deve informar que não é afiliado à Riot Games e que Valorant e os nomes dos agentes são marcas de seus respectivos proprietários. | Legal | Baixa | — |
| RS03 | O sistema deve ser acessado por navegador web, sem instalação de software nos computadores dos usuários. | Instalação | Média | — |
| RS04 | A interface deve estar em português do Brasil. | Internacionalização | Baixa | — |
| RS05 | O sistema deve ser acompanhado de um guia de uso com os fluxos principais de cada perfil. | Documentação | Baixa | — |

### 2.5 Diagrama de casos de uso

![Diagrama de casos de uso do VTM](imagens/diagrama-casos-de-uso.png)

*Figura 1 – Diagrama de casos de uso do Valorant Team Manager.*

#### Atores

| Ator | Descrição |
|------|-----------|
| Manager | Administra o time, o elenco e as inscrições em campeonatos, e responde aos convites de treino. |
| Treinador | Responsável técnico: monta composições, agenda e cancela treinos e registra resultados e partidas. |
| Jogador | Membro do elenco: informa os agentes que domina, confirma presença e consulta a agenda. |

#### Casos de uso

| ID | Caso de uso | Atores | Requisitos |
|----|-------------|--------|------------|
| UC01 | Autenticar-se | Manager, Treinador, Jogador | RF01 |
| UC02 | Gerenciar time | Manager | RF02 |
| UC03 | Gerenciar elenco | Manager | RF03 |
| UC04 | Registrar agentes dominados | Jogador | RF04 |
| UC05 | Gerenciar composições | Treinador | RF05, RF06 |
| UC06 | Agendar treino | Treinador | RF07 |
| UC07 | Responder convite de treino | Manager | RF08 |
| UC08 | Confirmar presença | Jogador | RF09 |
| UC09 | Registrar resultado de treino | Treinador | RF10 |
| UC10 | Inscrever time em campeonato | Manager | RF11 |
| UC11 | Registrar partida de campeonato | Treinador | RF12 |
| UC12 | Consultar agenda | Jogador | RF13 |
| UC13 | Consultar histórico | Treinador, Manager | RF14 |
| UC14 | Cancelar treino | Treinador | RF15 |
| UC15 | Verificar conflito de horários | (incluído por UC06 e UC08) | RN04 |

#### Relacionamentos

- **UC06 – Agendar treino «include» UC15 – Verificar conflito de horários:** ao agendar, o sistema verifica se algum jogador já tem outro evento no mesmo horário (RN04).
- **UC08 – Confirmar presença «include» UC15 – Verificar conflito de horários:** ao confirmar presença, o sistema faz a mesma verificação. Por ser um comportamento compartilhado por dois casos de uso, ele foi separado em UC15.

### 2.6 Documentação dos casos de uso

> *A ser preenchido.*

### 2.7 Diagrama de classes — modelo conceitual

> *A ser preenchido.*

---

## 3. Fase de projeto

### 3.1 Diagrama de classes — modelo de domínio

> *A ser preenchido.*

### 3.2 Segundo diagrama UML

> *A ser preenchido.*

---

## 4. Protótipo do sistema

### 4.1 Mapa do site

> *A ser preenchido.*

### 4.2 Telas

> *A ser preenchido.*

---

## 5. Considerações finais

### 5.1 Principais desafios
> *A ser preenchido.*

### 5.2 Aprendizados (integração entre análise e projeto)
> *A ser preenchido.*

### 5.3 Melhorias futuras
> *A ser preenchido.*

---

## 6. Referências

- **Ferramenta dos diagramas:** PlantUML, por meio do editor online PlantText (planttext.com)
- **Ferramenta do protótipo:** *a definir*
- **Uso de IA:** *a preencher*
- **Bibliografia:** *a preencher*
