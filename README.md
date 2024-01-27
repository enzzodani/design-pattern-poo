**Projeto Prático/Estudo de Caso: Padrão de Projeto Proxy**

**Nome do Padrão:**

Proxy

**Problema:**

Quando é necessário controlar o acesso a um objeto, seja para adicionar alguma lógica antes ou depois do acesso, verificar permissões, ou até mesmo para adiar a criação de um objeto custoso.

**Contexto:**

Situações em que é necessário controlar ou adiar o acesso a um objeto, como por exemplo, em sistemas de cache, sistemas de permissões, ou na criação de objetos pesados.

**Solução:**

Utilizar um objeto intermediário (proxy) que atua como substituto de outro objeto, controlando o acesso a ele. O proxy possui uma interface idêntica ao objeto que está sendo protegido, permitindo assim que seja utilizado de maneira transparente.

**Diagrama de Classes:**

```
          +---------------------+
          |        Subject      |
          +---------------------+
          |      request()      |
          +----------+----------+
                     ^
                     |
          +----------+----------+
          |         Proxy       |
          +---------------------+
          |  RealSubject subject|
          |      request()      |
          +----------+----------+
                     ^
                     |
          +----------+----------+
          |     RealSubject     |
          +---------------------+
          |      request()      |
          +---------------------+
```

**Classes e Métodos em Java:**

```java
// Interface comum entre Proxy e RealSubject
interface Subject {
    void request();
}

// Classe RealSubject - Objeto real que será protegido ou controlado
class RealSubject implements Subject {
    public void request() {
        System.out.println("RealSubject: Handling request.");
    }
}

// Interface Proxy - Define a mesma interface de Subject, com métodos adicionais se necessário
interface Proxy extends Subject {
    void additionalRequest();
}

// Classe Proxy - Controla o acesso ao RealSubject
class ProxyImpl implements Proxy {
    private RealSubject subject;

    public void request() {
        if (subject == null) {
            subject = new RealSubject();
        }
        System.out.println("Proxy: Handling request by delegating to RealSubject.");
        subject.request();
    }

    public void additionalRequest() {
        if (subject == null) {
            subject = new RealSubject();
        }
        System.out.println("Proxy: Handling additional request by delegating to RealSubject.");
        subject.request();
    }
}

// Exemplo de uso
public class TesteProxy {
    public static void main(String[] args) {
        Proxy proxy = new ProxyImpl();

        // Primeira chamada - o objeto RealSubject é criado e a requisição é delegada a ele
        proxy.request();

        // Chamada adicional - o objeto RealSubject já foi criado, então a requisição é novamente delegada a ele
        proxy.request();

        // Chamada de um método adicional
        ((ProxyImpl) proxy).additionalRequest();
    }
}
```

**Referências:**

- "Design Patterns: Elements of Reusable Object-Oriented Software" by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.
- Padrão Proxy em https://refactoring.guru/design-patterns/proxy

**Explicação do Código Apresentado:**

- **Subject:** Interface comum entre o Proxy e o RealSubject, contendo o método `request()` que será chamado pelo cliente.
  
- **RealSubject:** Classe que representa o objeto real que será protegido ou controlado. Implementa a interface `Subject` e contém a lógica real do método `request()`.

- **Proxy:** Interface que estende `Subject` e pode conter métodos adicionais. Serve como uma interface comum entre o cliente e o objeto real (RealSubject).

- **ProxyImpl:** Implementação concreta do Proxy. Controla o acesso ao RealSubject, criando-o quando necessário. Pode adicionar lógica antes ou depois de delegar a chamada ao RealSubject.

- **TesteProxy:** Exemplo de uso do padrão Proxy, onde o Proxy é criado, suas operações são chamadas e o comportamento de controle de acesso é demonstrado.