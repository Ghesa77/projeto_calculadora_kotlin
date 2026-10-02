# Calculadora

Calculadora web feita em Kotlin com Spring Boot. As contas são feitas na tela e cada resultado é guardado na API da Pekus, então não existe banco de dados local. Tem também uma página de histórico onde dá para ver tudo o que já foi salvo e apagar o que não interessa mais.

## Como rodar

Você só precisa ter o Java 17 (ou mais novo) instalado. O Gradle já vem junto com o projeto.

```
./gradlew bootRun
```

No Windows, se não estiver usando o Git Bash, o comando é `gradlew.bat bootRun`. Na primeira vez demora um pouco, porque ele baixa as dependências. Quando aparecer a linha "Started CalculadoraApplicationKt", é só abrir http://localhost:8080.

## Configuração

A URL da API e a chave ficam no arquivo `src/main/resources/application.yml`, e só ali. Se você não quiser deixar a chave escrita no arquivo, defina a variável de ambiente `CALC_API_KEY` que ela passa a valer no lugar.

## O que a API recebe

Tudo passa pelo endereço `/api/Calculadora`, e a chave vai na própria URL, no parâmetro `apikey`.

- POST guarda um cálculo. O corpo leva valorA, valorB, operacao, resultado e dataCalculo. O id não é enviado, a API gera e devolve o número.
- GET lista os cálculos guardados.
- DELETE apaga um cálculo, passando o `id` na URL.

A operação sempre vai como um único caractere: `+`, `-`, `*` ou `/`.

## Como o código está organizado

Dentro de `src/main/kotlin/br/com/calculadora` ficam o controller, que recebe as chamadas da página, o service, que valida os números e faz a conta, e o client, que é o único lugar que conversa com a API. Os modelos ficam em `dto`, os erros em `exception` e a leitura da configuração em `config`.

A parte visual está em `src/main/resources`: o HTML em `templates`, e o CSS, o JavaScript e a logo em `static`.

## Se algo der errado

A API fica na intranet da Pekus, então fora da rede da empresa (ou sem VPN) ela pode não responder. Nesse caso a tela avisa que não conseguiu conectar. Erros de chave, de timeout e respostas estranhas da API também aparecem como mensagens simples, sem detalhes técnicos.
