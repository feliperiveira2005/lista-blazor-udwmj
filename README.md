# Lista de Exercícios - Blazor

Disciplina: Usabilidade, Desenvolvimento Web, Mobile e Jogos

## Exercício 1 - O fim do reino do JavaScript?

O **Blazor** é um framework da plataforma .NET usado para construir interfaces web interativas com **C#**, HTML e CSS. A interface é dividida em componentes Razor, normalmente armazenados em arquivos `.razor`. Esses arquivos permitem misturar marcação HTML com expressões, propriedades e métodos escritos em C#.

Por isso, quem já desenvolve aplicações .NET pode reutilizar conhecimentos como orientação a objetos, tipos, classes, métodos, injeção de dependência, programação assíncrona e bibliotecas do ecossistema .NET na criação de aplicações web.

A principal vantagem em relação a frameworks SPA como React ou Angular é a possibilidade de usar C# e o ecossistema .NET tanto no back-end quanto em grande parte do front-end. Isso reduz a troca de contexto entre linguagens, facilita o compartilhamento de modelos e regras de validação e mantém uma experiência de desenvolvimento mais uniforme. Entretanto, isso não significa que o JavaScript deixa de existir: ele continua fazendo parte da plataforma web e pode ser usado por meio de interoperabilidade quando alguma funcionalidade do navegador não estiver disponível diretamente no Blazor.

## Exercício 2 - Blazor Server vs. Blazor WebAssembly

### Cenário A: sistema interno de caixa

O modelo mais adequado é o **Blazor Server**.

A aplicação executa no servidor, enquanto o navegador recebe as atualizações da interface por uma conexão SignalR. Como os computadores são antigos, esse modelo é vantajoso porque exige menos processamento e não precisa baixar toda a aplicação para executar no cliente. A rede local rápida e com baixa latência reduz o principal problema do Blazor Server, que é a dependência de uma conexão constante e responsiva com o servidor.

### Cenário B: aplicativo de trilhas offline

O modelo mais adequado é o **Blazor WebAssembly**.

A aplicação é baixada e executada no navegador do celular. Depois do primeiro carregamento, seus arquivos podem ser armazenados localmente, especialmente quando a aplicação é configurada como PWA. Assim, o usuário pode abrir os recursos já armazenados mesmo sem conexão. Os mapas e dados necessários também precisam ser previamente baixados e guardados no dispositivo, pois o WebAssembly sozinho não torna dados externos automaticamente disponíveis offline.

## Exercício 3 - Anatomia de um componente Razor

### a) O que representa a diretiva `@code`?

O bloco `@code` contém a parte lógica do componente escrita em C#. Nele são declarados campos, propriedades, métodos, parâmetros e outros comportamentos usados pela interface. Ele exerce um papel semelhante ao código de uma classe, pois um componente Razor é transformado em uma classe C# durante a compilação.

### b) Como `@quantidade` aparece dinamicamente no HTML?

O caractere `@` informa ao Razor que a expressão seguinte deve ser interpretada como C#. Dessa forma, `@quantidade` insere no HTML o valor atual da variável.

Quando o estado do componente muda, o Blazor renderiza novamente a parte necessária da interface. Essa ligação entre o estado em C# e sua representação visual é chamada de **data binding**, neste caso uma vinculação unidirecional, pois o valor flui da variável para a tela.

### c) O que acontece ao clicar em `@onclick="Incrementar"`?

A diretiva `@onclick` associa o evento de clique do botão ao método C# `Incrementar`. Quando o usuário clica:

1. O Blazor detecta o evento.
2. O método `Incrementar` é executado.
3. O método altera o valor de `quantidade`.
4. O Blazor cria uma nova representação da interface e compara com a anterior.
5. Somente a parte modificada do DOM é atualizada, exibindo o novo valor.

No Blazor Server, o evento e a atualização trafegam pela conexão SignalR. No Blazor WebAssembly, o tratamento acontece diretamente no navegador.

Exemplo disponível em [Exemplos/Contador.razor](Exemplos/Contador.razor).

## Exercício 4 - Comunicação entre componentes

### a) Como o componente pai envia dados ao filho?

No componente filho, a propriedade que receberá o valor deve ser marcada com o atributo **`[Parameter]`**, pertencente ao namespace `Microsoft.AspNetCore.Components`.

### b) Exemplo de estrutura pai e filho

O componente pai usa o componente filho e envia um valor pelo atributo `Titulo`:

```razor
<CartaoProduto Titulo="Produto em destaque" />
```

O componente filho recebe esse valor em uma propriedade:

```csharp
[Parameter]
public string Titulo { get; set; } = string.Empty;
```

Exemplos completos:

- [Exemplos/PaginaVendas.razor](Exemplos/PaginaVendas.razor)
- [Exemplos/CartaoProduto.razor](Exemplos/CartaoProduto.razor)

## Exercício 5 - Pensamento arquitetural

O `Console.ReadLine()` bloqueia a execução do programa até que o usuário digite algo. Esse funcionamento não é adequado para uma aplicação web porque o servidor ou a interface precisa continuar disponível para processar várias requisições, eventos e usuários. Bloquear uma thread esperando indefinidamente pela entrada de uma pessoa prejudicaria a escalabilidade e a capacidade de resposta da aplicação.

No Blazor, a interação segue o modelo **orientado a eventos**. A aplicação apresenta a interface e permanece pronta para reagir. Quando o usuário clica, digita ou envia um formulário, o evento correspondente chama um método C#. Assim, o programa não precisa parar em uma linha aguardando entrada: ele responde aos eventos conforme eles acontecem.
