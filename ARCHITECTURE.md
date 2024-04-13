# ARCHITECTURE

## 1 Camadas
![!alt](assets/docs/images/layers.png)

### 1.1 Camada de regra de negócio (Domain)
Responsável por representar e executar o que o sistema deve fazer, ou seja, as regras de negócio.
Essa camada DEVE ser a mais pura possível.
Essa camada não DEVE se conectar diretamente a uma camada externa, e sim por meio da camada de adaptação de interface (vide: 1.2).
Essa camada deve validar os dados.

### 1.1.1 Designs do domain

**Entities**: Representar a regra de negócio.[Documentation](https://www.codeproject.com/Articles/4293/The-Entity-Design-Pattern).
A classe que representa uma entidade não deve conter execuções de lógica ou factories como **fromJson** e **toJson**.
Toda entidade deve herdar de Equatable para facilitar a compreensão das instâncias perante uma situação de comparação.
Toda entidade deve ser imutável.

```dart
class UserEntity extends Equatable {
    final int id;
    final String name;
    final String email;

    const UserEntity(this.id, this.name, this.email);

    UserEntity copyWith({
        int? id,
        String? name,
        String? email,
    }){
        return UserEntity(
            id ?? this.id,
            name ?? this.name,
            email ?? this.email,
        );
    }

    List get props => [id, name, email];
}
```
**ValueObject**: Auxiliar na validação dos dados. [Documentation](https://medium.com/@lexitrainerph/deep-dive-into-the-value-object-pattern-in-c-basics-to-advanced-b058b49d8565#:~:text=Introduction,which%20have%20a%20distinct%20identity.).

Deve ser uma instância imutável.
O uso de **ValueObject** pode ser feito para validação de formulários mas não se limitando a ser.

```dart
class Email {
    final String value;

    Email(this.value);

    String? validate(){
        if(value.isEmpty){
            return 'Email can\'t be empty';
        }
    }
}
```


**Usecase**: Responsável por executar as regras de negócio. [Documentation](https://martinfowler.com/bliki/UseCase.html).

Um usecase não deve chamar outro usecase. Este também não deve fazer acesso direto a componentes externos, necessitando o uso de algum design ou instrução da camada de adaptação.
UseCases devem ter uma interface e uma implementação separadas.
O método call deve conter apenas um argumento. Se houver a necessidade de mais argumentos, favor usar DTO.

```dart
abstract class FetchFoods {
   Future<List<FoodEntity>> call();
}

class FetchFoodsWithFoodRepository implements FetchFoods {
    Future<List<FoodEntity>> call(FetchFoodDTO dto){
        return [];
    }
}
```

**Data Transfer Object**: Deve ser usado para mitigar o uso de mais de um argumento nas execuções de regras.

```dart
class FetchFoodDTO {
    final int page;
    final int offset;
    final String? query;

    FetchFoodDTO(this.page, this.offset, this.query);
}
```


### 1.2 Camada de adaptação (Interface Adaptation)
Essa camada serve pra ligar o domain com os componentes externos.
Essa camada será responsável por agir como intermediador, transformando e adaptando os dados.


### 1.3 Camada de componentes externos (Frameworks and Drivers)
Deve fornecer interação com o usuário e comunicações com drivers externos como consumo de API ou banco de dados.
