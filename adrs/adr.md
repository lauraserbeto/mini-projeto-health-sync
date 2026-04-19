# Adoção da Arquitetura Hexagonal para o Núcleo de Saúde

## Contexto
Durante o desenvolvimento inicial do HealthSync (Fase 1), identificou-se que a estrutura baseada no modelo N-Tier tradicional estava gerando um acoplamento excessivo entre a lógica de negócio (geração de alertas preventivos) e os detalhes de infraestrutura (drivers de wearables e persistência em banco de dados).

Este cenário criava "gargalos táticos": qualquer alteração técnica em protocolos de comunicação ou esquemas de dados forçava a re-homologação de regras clínicas críticas. Conforme apontado por Pressman (2011), arquiteturas mal estruturadas acumulam dívida técnica precocemente, o que poderia comprometer a confiabilidade do sistema.

## Decisão
Decidimos implementar a Arquitetura Hexagonal especificamente para o módulo de Telemetria e IA. 

O domínio da saúde — contendo as regras de correlação de sintomas e algoritmos de predição — será posicionado no centro da aplicação, totalmente isolado. A comunicação com o mundo exterior (Wearables, Banco de Dados SQL/NoSQL, APIs externas) ocorrerá exclusivamente através de:
1. **Portas (Interfaces):** Que definem como o sistema deseja interagir.
2. **Adaptadores:** Que implementam a tecnologia específica para atender a essas portas.

Seguimos a Regra de Dependência Tática de Martin: as dependências de código devem apontar apenas para dentro, em direção às políticas de alto nível.

## Consequências

### Ganhos (Prós)
* **Isolamento de Regras de Negócio:** Mudanças tecnológicas na nuvem ou nos sensores de telemetria não afetam a lógica médica.
* **Testabilidade Elevada (RNF Confiabilidade):** É possível testar o motor de IA e a lógica de alertas utilizando "Mocks" nas portas, garantindo que a precisão supere os 95% exigidos sem depender de hardware real.
* **Redução de Acoplamento:** Facilita a evolução do sistema, permitindo trocar o fornecedor de banco de dados ou o broker de mensagens com impacto zero no core do software.

### Perdas (Contras)
* **Aumento de Boilerplate:** Exige a criação de múltiplas interfaces e classes de tradução (mappers) entre camadas.
* **Curva de Aprendizado:** A equipe precisa de maior domínio conceitual para não violar os limites das camadas durante a manutenção.

## Referências
* MARTIN, Robert C. *Clean Architecture: A Craftsman’s Guide to Software Structure and Design*. Prentice Hall, 2017.
* PRESSMAN, Roger S. *Engenharia de Software: Uma Abordagem Profissional*. 7ª ed. McGraw-Hill, 2011.