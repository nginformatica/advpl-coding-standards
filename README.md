# AdvPL Coding Standards

O presente documento tem por objetivo estabelecer um padrão de boas práticas a serem utilizadas pelas equipes de Desenvolvimento NG que atuam na plataforma Protheus AdvPL. As regras aqui listadas **não são restritivas**, mas buscam estabelecer uma identidade única para os fontes. A adoção desse padrão deve naturalmente se tornar um hábito, aumentando a legibilidade dos fontes e proporcionando mais segurança no processo de manutenção de sistemas.

Novas funções devem ser desenvolvidas considerando as boas práticas listadas, e funções já existentes devem ser adequadas gradativamente. Entretanto **não se recomenda** a mudança de trechos que não fazem parte do escopo de alteração, de forma a não comprometer a revisão técnica do fonte nem gerar risco de inserção de novos bugs acidentalmente.


## Arquivo

- A extensão deve ser minúscula (exemplo: `.prw`, `.apw`)

- O nome do arquivo deve ser preferencialmente em minúsculo

## Estilo

- Tabs somente para indentação, não para separação

> A tabulação deve ser utilizada somente à esquerda, antes de iniciar a linha e nunca no meio da linha. O motivo é que quando o arquivo for aberto em diferentes editores (TDS, VSCode, Sublime, etc.), a formatação será respeitada

- Nome de variáveis devem se basear na notação húngara

>Iniciar por:
>- n-numérico
>- c-char/string
>- d-data
>- l-logic/boolean
>- a-array/matriz
>- o-objetos
>- b-bloco de código
>- x-indefinido

- Evite nomes de variáveis como `nX` ou `nY` (exceto para índices). Seja mais descritivo

- Prefira declarar variáveis agrupando-as logicamente por tipo ou utilização, separando por linhas

- É redundância inicializar variáveis com `Nil`, exceto variáveis públicas, que por sua vez não são comuns - no geral se considera uma prática ruim por poluir o escopo global

- Palavras-chave da linguagem devem usar **UpperCamelCase** (exemplos: `If`, `EndIf`, `While`)

- Ao terminar uma instrução `While` ou `Do While`, prefira `EndDo` ao invés de somente `End`

- Ao terminar uma instrução `For`, prefira `Next <variable>` ao invés de somente `Next`

- Nomes de variáveis locais devem ser em **lowerCamelCase** (exemplos: `cName`, `nAge`)

- Nomes de funções em notação húngara devem usar **lowerCamelCase** (exemplo: `aAdd`)

- Use as variáveis com nomes iguais em tamanho e caixa (não faça `thisIsMyVari` e `THISISMYVARI`)

- Nomes de funções sem notação húngara devem usar **UpperCamelCase** (exemplo: `RetSqlName`)

- Diretivas do pré-processador (#define < include >) devem referenciar a include com a mesma capitulação do arquivo físico, preferencialmente letras minúsculas.

> É uma boa prática que evita conflitos de case sensitive em processos de compilação no Linux, por exemplo.

- Evite ultrapassar 120 colunas horizontalmente. Quebre o código com `;` quando necessário

- Valores lógicos devem usar caixa alta (exemplo: `.F.`)

- Espaço entre operadores. Use `nValue > nExpected` ao invés de `nValue>nExpected`

- Idioma padronizado. Evite misturar português e inglês quando possível

- Deixar 1 linha em branco para cada *statement*, exceto conjuntos de *statements*

```
Function Test()

    If Something
        ...
    EndIf

    For nI := 1 To 10
        For nZ := 1 To 10
            ...
        Next nZ
    Next nI

Return
```

- Separar funções, pulando uma linha após o return, antes de iniciar um novo bloco

- Deixar 1 linha vazia no final de cada arquivo

> Ferramentas de diff se perdem sem linha final e alguns editores já salvam por padrão uma linha extra. Alguns compiladores não entendem o fim de arquivo (não é o caso do AdvPL), por isso é uma boa prática em programação.

- Prefira aspas simples `'` ao invés de duplas `"`

> Aspas simples facilitam a leitura do código e permitem, por padrão, o uso de aspas duplas nas strings (ex: cString := 'Verifique o campo "quantidade" na tela')

- Para acesso de índices múltiplos, evite `aList[ nI ][ nJ ]`. Use `aList[ nI, nJ ]`

- Evite utilizar mais do que três instruções `For.. Next` em cascata

- Funções **não** devem receber mais que 6 parâmetros (dessa forma se mantém uma função mais coesa, com um objetivo específico)

> Esse é um padrão utilizado em praticamente qualquer linguagem de programação com funções. Funções mestras, muito grandes, são muito mais difíceis de manter e de testar do que funções que fazem uma coisa, e a fazem bem feito. Testes unitários são integráveis a funções simples, mas difíceis em funções com muitos parâmetros. É até comum que as pessoas se percam com tantos parâmetros vazios (ex: TObj:New(100,30,,,,,,.F.)).

> Importante ressaltar que cada função deve ser exatamente isso: uma função. Quanto a isso, tem dezenas de artigos sobre software engineering de como funções com muitos parâmetros quebram modularidade e reuso. Normalmente funções que precisam de muitos parâmetros são passíveis de melhoria para melhor abstração em sua arquitetura.

- Não desenvolva funções de alto acoplamento, quer dizer, funções que precisam estar acopladas à outras funções ou variáveis para funcionar (como o caso de funções que utilizam variáveis private declaradas em outro fonte)

- Evite aninhamentos com mais de 3 statements (exemplo: `If` dentro de `If` dentro de `If`)

- A função de If em linha deve ser utilizada com os dois I's em maiúsculo: `IIf`

- Use `If` somente para *statement* (If .. Else .. EndIf) e `IIf` para *expressão* (x:= IIf(lVar,10,20))

> *Statements* não atribuem valor, por exemplo `If (lVar)` enquanto *expressão* atribui, por exemplo `x := If(lVar, 10, 20)`. O AdvPL permite a ambiguidade de se usar `If` e `IIf` para expressões. Nesse caso utilize sempre `IIf`, pois se usar `If` o compilador irá buscar pelo `EndIf`, o que poderia gerar erro em outras linguagens.

- Use `!=` ao invés de `<>`

- Prefira  `==` para comparação ao invés de apenas `=`

- Não faça `== .T.`

- Use `!` ao invés de `.Not.`

## Espaçamento

- Deve haver 1 espaço entre os argumentos de função, blocos e arrays (exemplo: `RetSqlName( 'STJ' )`)

- Deve haver 1 espaço após cada vírgula (exemplo: `{ 1, 2, 3 }`)

- Deve haver espaço entre parâmetros de funções (use `Call( 1, 2, 3 )` e não `Call(1,2,3)`)

> A recomendação é usar espaçamento antes de iniciar o primeiro parâmetro, entre cada parâmetro e ao final. Entretanto, o primeiro e último espaços ficam a critério de cada um.

- O `Return` pode ficar alinhado à esquerda (mesmo alinhamento de Function), mesmo não sendo um terminador

- Em comentários, 1 espaço após `//`

> Apesar de não ser uma obrigação, o espaçamento antes de começar a descrever o comentário auxilia na leitura e também a encontrar e substituir programaticamente padrões da linguagem.

- Prefira comentários em `//` para quando uma linha e `/**/` para trechos de código ou comentário de parâmetros (ex: function(10`/*nAltura*/`,20`/*nLargura*/`))

## Redundância

- Substitua `If` dentro de `If` por `.And.`

## Funcionamento

- Lembre-se de, ao criar uma tabela temporária, fechá-la com `dbCloseArea` e para casos em que se usa FWTemporaryTable, usar o método `delete`

- Lembre-se de fechar o _handler_ para o arquivo com `fClose` ao usar `fOpen`

- Quando deslocar para outro registro utilizando `dbSkip`, garanta estar posicionado na tabela desejada, caso contrário utilize `TABLE->( dbSkip() )` ou então utilize `dbSelectArea( TABLE )` antes do `dbSkip()`

- Se atente ao uso de funções (Ex: FWTemporaryTable) no meio de blocos de transação, pois em Oracle sua chamada pode realizar o commit dos dados no meio do processo

## Funções

- Toda função deve ter um cabeçalho de documentação baseado no modelo Protheus.doc, e o autor genérico (como "NG Informática") não deve ser utilizado

- São consideradas **funções genéricas** aquelas que se aplicam a qualquer módulo, normalmente sem tratar de conceitos e regra de negócio. Devem ser criadas em fontes genéricos como NGUtil e preferencialmente começar pelos caracteres `NG`

- São consideradas **funções genéricas do módulo** aquelas que atendem regras mais genéricas de algum módulo, sendo utilizadas em um grupo de rotinas. Devem ser criadas em fontes do módulo como MNTUtil ou MDTUtil e preferencialmente começar pelos caracteres do módulo, ex: `MNT` ou `MDT`

- São consideradas **funções de rotina** aquelas que atendem regras específicas de alguma rotina, mas que também podem ser chamadas externamente. Devem ser criadas no fonte que a utiliza e preferencialmente começar pelos caracteres da própria rotina, como `MNTA080Cad()`. Evitar também reduzir o identificador do fonte para `MNA080` ou `MNT080`

- São consideras **funções estáticas** aquelas que são utilizadas apenas por um determinado fonte, sem chamada externa por outros fontes. Devem ser criadas no fonte que a utiliza e preferencialmente iniciando pelo caracter `f` como `fCalcHora()`


## Referências

AdvPL Coding Standards - https://github.com/haskellcamargo/advpl-coding-standards by @haskellcamargo
