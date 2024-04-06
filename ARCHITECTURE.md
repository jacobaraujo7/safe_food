# ARCHITECTURE

## 1 Camadas
![!alt](assets/docs/images/layers.png)

### 1.1 Camada de regra de negócio (Domain)
Responsável por representar e executar o que o sistema deve fazer, ou seja, as regras de negócio.
Essa camada DEVE ser a mais pura possível.
Essa camada não DEVE se conectar diretamente a uma camada externa, e sim por meio da camada de adaptação de interface (vide: 1.2).
Essa camada deve validar os dados.


### 1.2 Camada de adaptação (Interface Adaptation)
Essa camada server pra ligar o domain com os componentes externos.
Essa camada será responsável por agir como intermediador, transformando e adaptando os dados.


### 1.3 Camada de componentes externos (Frameworks and Drivers)
Deve fornecer interação com o usuário e comunicações com drivers externos como consumo de API ou banco de dados.
