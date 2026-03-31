## 👨‍💻 Autor

Iády Simões – ADS / UCSAL

# Sistema das Olimpíadas – Refatoração com SOLID

## Objetivo

Refatorar o sistema legado aplicando os princípios do **SOLID**, garantindo melhor organização, manutenção e extensibilidade do código, **sem alterar o comportamento funcional original**.


## Problema identificado

Durante a análise do sistema, foi identificado que a classe `App` concentrava múltiplas responsabilidades, tornando o código:

* Difícil de manter
* Pouco reutilizável
* Difícil de evoluir

A refatoração teve como foco a **separação de responsabilidades e melhoria da estrutura do sistema**.


## 🔧 Refatorações realizadas


## 1. Extração do cálculo de nota (SRP)

### Antes

O cálculo da nota estava dentro da classe `App`:

```java
public static int calcularNota(Tentativa tentativa) {
    int acertos = 0;
    for (var r : tentativa.getRespostas()) {
        if (r.isCorreta())
            acertos++;
    }
    return acertos;
}
```

### Depois

Foi criada uma classe específica para o cálculo:

```java
public class CalculadoraNota {

    public int calcular(Tentativa tentativa) {
        int acertos = 0;
        for (var r : tentativa.getRespostas()) {
            if (r.isCorreta()) {
                acertos++;
            }
        }
        return acertos;
    }
}
```

### Princípio aplicado

**SRP – Single Responsibility Principle**
Cada classe deve ter apenas uma responsabilidade.
Agora, a responsabilidade de cálculo foi separada da classe principal.


## 2. Aplicação do Open/Closed Principle (OCP)

### Antes

Caso fosse necessário alterar o cálculo da nota, seria preciso modificar diretamente o método existente.

### Depois

Foi criada uma abstração para permitir extensão sem modificar o código existente:

```java
public interface Calculadora {
    int calcular(Tentativa tentativa);
}
```

```java
public class CalculadoraNotaPadrao implements Calculadora {
    public int calcular(Tentativa tentativa) {
        int acertos = 0;
        for (var r : tentativa.getRespostas()) {
            if (r.isCorreta()) {
                acertos++;
            }
        }
        return acertos;
    }
}
```

### Princípio aplicado

**OCP – Open/Closed Principle**
O sistema agora pode ser estendido (novas formas de cálculo) sem modificar código existente.


## 3. Aplicação do Liskov Substitution Principle (LSP)

### Implementação

As classes que implementam a interface `Calculadora` podem ser substituídas entre si sem quebrar o sistema.

```java
Calculadora calculadora = new CalculadoraNotaPadrao();
int nota = calculadora.calcular(tentativa);
```

### Princípio aplicado

**LSP – Liskov Substitution Principle**
Qualquer implementação de `Calculadora` pode ser usada sem alterar o comportamento esperado.


## 4. Aplicação do Interface Segregation Principle (ISP)

### Antes

Interfaces poderiam conter métodos desnecessários para algumas classes.

### Depois

Interfaces foram separadas conforme suas responsabilidades.

Exemplo:

```java
public interface Calculadora {
    int calcular(Tentativa tentativa);
}
```

### Princípio aplicado

**ISP – Interface Segregation Principle**
Nenhuma classe é obrigada a implementar métodos que não utiliza.


## 5. Aplicação do Dependency Inversion Principle (DIP)

### Antes

A classe principal dependia diretamente de implementações concretas.

### Depois

Agora depende de abstrações:

```java
public class App {

    private Calculadora calculadora;

    public App(Calculadora calculadora) {
        this.calculadora = calculadora;
    }

    public void executar(Tentativa tentativa) {
        int nota = calculadora.calcular(tentativa);
        System.out.println("Nota: " + nota);
    }
}
```

### Princípio aplicado

**DIP – Dependency Inversion Principle**
A classe `App` agora depende de uma interface (`Calculadora`) e não de uma implementação concreta.



## Benefícios da refatoração

* Código mais organizado
* Melhor manutenção
* Facilidade para adicionar novas funcionalidades
* Maior reutilização de código
* Melhor separação de responsabilidades



## Estrutura final (exemplo)

```
📁 src
 ┣ 📄 App.java
 ┣ 📄 Calculadora.java
 ┣ 📄 CalculadoraNotaPadrao.java
 ┣ 📄 CalculadoraNota.java
 ┗ 📄 Tentativa.java
```


## Conclusão

A aplicação dos princípios SOLID permitiu transformar um sistema com responsabilidades centralizadas em uma arquitetura mais modular, flexível e preparada para evolução, mantendo o comportamento original intacto.
