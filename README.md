## Teste para Desenvolvedor Flutter Pleno

### Parte 1: Conhecimentos Técnicos Básicos
**Instruções:** Responda às seguintes perguntas teóricas.

1. **Flutter e Dart:**
   -Explique a diferença entre `StatelessWidget` e `StatefulWidget`.

**Resposta:** 

 **StatelessWidget** não possuem estado, ele é construído, caso haja, com os valores passados por parâmetros, e é constante. Sempre que seu contexto pai é recriado ele também é recriado novamente. Ele é de certa forma imutável.
    **StatefulWidget** possuem estados, um ciclo de vida próprio, onde mesmo que o contexto pai possa ser recriado é possível manter o seu estado atual. É possível também mudar seu estado independente do contexto pai, portanto por ter um estado ele se torna mutável.


   - Descreva o ciclo de vida de um `StatefulWidget`.

**Resposta:**
1. **createState()**: Inicializar e instanciar o estado dentro de uma widget.
2. **initState():** : Chamado uma única vez e pode ser reescrito para inicialização de propriedades.
3. **didChangeDependencies()**: Chamado após o initState e quando as dependências do widget mudam.
4. **build():** Método que renderiza os widgets na tela.
5. **setState()**: Método que informa uma mudança no estado do widget, fazendo que o build() seja chamado e assim renderizando novamente a tela e exibindo então as mudanças (se houverem).
6. **didUpdateWidget()**: Verifica que se houve alguma mudança no widget pai.
7. **deactivate()**: Quando a widget é removida temporariamente da arvore de widgets. Ex: quando você está na página A, que possui a Widget em questão, e navega para a pagina B, a Widget é desativada temporariamente caso a página não tenha sido destruída e exista a possibilidade do usuário retornar para a pagina A.
8. **dispose()**: Chamado quando o widget é destruído, removido permanentemente da arvore de widgets.


3. **Gerenciamento de Estado:**
   - Compare os métodos `setState`, `Provider`, e `BLoC` para gerenciamento de estado no Flutter. Quais são as vantagens e desvantagens de cada um?
   - 
**Resposta:**

**setState** : O mais simples de ser utilizado dos três, não requer nenhum pacote externo e é só chamar `setState((){})` em qualquer statefulWidget.  Ideal para componentes menores. Porém torna-se complexo para utilizar em partes "maiores" do app, além de reconstruir todo o widget, caso seja necessário recriar só um pedaço, é possível envolvendo o trecho especifico em um `statefulBuilder`, mas isso se tornará muito verboso e complexo.

**Provider**: Simplifica a gestão de estado em partes mais complexas, é possível também reconstruir só trechos necessários.
Requer um pouco mais de entendimento que o setState caso contrário pode se tornar complexo, tornando toda a gestão de estado mais confusa.

**BloC:** Regra de negócios e interface do usuários bem separadas, altamente personálizável e escalável. 
O Bloc pode ser bem mais complexo de implementar e utilizar principalmente por iniciantes, sua curva de aprendizado é mais acentuada e diversas vezes pode complexo demais para situações simples, "matar uma formiga com uma bazuca". Porém nesses casos Eu particularmente gosto de utilizar o Cubit (uma simplificação do BloC) , onde é requerido uma menor complexidade.

### Parte 2: Programação e Resolução de Problemas
**Instruções:** Desenvolva um aplicativo Flutter com as seguintes especificações.

- **Aplicativo de Lista de Tarefas:**
  - Permitir adicionar, remover e marcar tarefas como concluídas.
  - Incluir uma tela para adicionar detalhes da tarefa.
  - Implementar persistência de dados (pode ser local).
    
**Resposta**: https://github.com/mariovitor/flutter-todo-list-cubit

### Parte 3: Conhecimento Avançado e Melhores Práticas
**Instruções:** Responda à seguinte pergunta teórica e desenvolva um exemplo prático.

1. **Pergunta Teórica:**
   - Como você implementaria o padrão BLoC para gerenciamento de estado em um aplicativo Flutter? Explique com exemplos.

**Resposta**: Definiria as classes de todos os eventos possíveis de interação do usuário. Depois definiria os estados decorrentes daqueles eventos.
Depois criaria o bloc e implementaria as lógicas.

2. **Exemplo Prático:**
   - Implemente um exemplo simples usando BLoC para gerenciar o estado de um formulário de login.
     
   **RESPOSTA**:
  https://github.com/mariovitor/teste-flutter/blob/main/exercício-3-bloc.md

### Parte 4: Testes e Debugging
**Instruções:** Escreva testes para o código fornecido.

- **Desafio de Teste:**
  - Escreva testes unitários para uma função de validação de email.
  - Escreva testes de widgets para um botão de login que se habilita apenas quando o formulário é válido.
    
  **RESPOSTA**:
  https://github.com/mariovitor/teste-flutter/blob/main/exercicio4%20-%20testes.md

### Parte 5: Avaliação de Código e Análise Crítica
**Instruções:** Revise o seguinte trecho de código Flutter.

- **Revisão de Código:**
  - Identifique e corrija erros no código fornecido.
  - Sugira melhorias para otimização de desempenho e legibilidade.

```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: () {
        print('Widget tapped');
      },
      child: Container(
        padding: EdgeInsets.all(10),
        decoration: BoxDecoration(
          color: Colors.blue,
          borderRadius: BorderRadius.circular(5),
        ),
        child: Text('Tap Me'),
      ),
    );
  }
}
```

**RESPOSTA** :
Faltou construtor, também otimizei para um reaproveitamento do widget para outros lugares sem afetar os lugares onde o mesmo já era utilizado. Mas se esse não fosse o desejo, poderíamos colocar const no texto também. Segue a seguir.

Outra possibilidade seria colocar o valor 'Tap me' em uma const string textLabel
e o "widget tapped" em outra com o nome printText.

Também prefiro utilizar o InkWell ao invés de GestureDetector em alguns lugares, muitas vezes ele não capta tão bem o clique em lugares com fundo transparente.
```dart
class  MyWidget  extends  StatelessWidget {
		const  MyWidget({
			Key?  key,
			this.text  =  'Tap Me',
			this.onTap,
		}) :  super(key:  key);
		final  String  text;
		final  VoidCallback?  onTap;
		void  onTapMethod() {

			if (onTap  !=  null) return  onTap?.call();
			print('Widget tapped');
		}
		@override
		Widget  build(BuildContext  context) {
			return  InkWell(
			onTap:  onTapMethod,
			child:  Container(
			padding:  const  EdgeInsets.all(10),
			decoration:  BoxDecoration(
			color:  Colors.blue,
			borderRadius:  BorderRadius.circular(5),
			),
			child:  Text(text),
			),
		);
	}
}
```
