<h3 align="center" >Course: Formal Languages & Finite Automata</h3>
<h1 align="center">Intro to formal languages. Regular grammars. Finite Automata.</h1>
<h4 align="center">Technical University of Moldova  </h4>
<h4 align="center">FCIM   |   UTM   |   Spring 2024</h4><br><br>

<p align=center>                           
  <img align=center style="height: 50%;
  width: 50%; " src="https://utm.md/wp-content/uploads/2020/12/logo-sigla.png" />
</p>
</br><p align=right>  
p. Formal Languages and Finite Automata
</p>

<p align="right" > Verificat, Dumitru Cretu</p>
<p align="right" > student FAF-223, Cristian Prodius</p>
</br><p align=center>  
Chisinau 2024
</p>
<hr></br></br></br>

<h1 align='center'> 
Laboratory work nr. 6
</h1>




## Objectives:

1. Get familiar with parsing, what it is and how it can be programmed [1].
2. Get familiar with the concept of AST [2].
3. In addition to what has been done in the 3rd lab work do the following:
    1. In case you didn't have a type that denotes the possible types of tokens you need to:
        1. Have a type TokenType (like an enum) that can be used in the lexical analysis to categorize the tokens.
        2. Please use regular expressions to identify the type of the token.
    2. Implement the necessary data structures for an AST that could be used for the text you have processed in the 3rd lab work.
    3. Implement a simple parser program that could extract the syntactic information from the input text.




## Implementation description:
To understand the parser, I'll focus on a segment of my program that deals with literal parsing. This parser constructs the AST using recursive descent and a lookahead to determine the parsing strategy.

Here's the Parser:

```from Tokenizer import *

class Parser:
    def parse(self, string):
        self._string = string
        self._tokenizer = Tokenizer(string)
        self._lookahead = self._tokenizer.getNextToken()
        return self.Program()
```
Next, we dissect the strings by defining the non-terminals using Python methods. The "Program" method/non-terminal is the primary non-terminal where parsing begins. It comprises a statement list.

Here's the Program:
```def Program(self):
    return {
        "type": "Program",
        "body": self.StatementList()
    }
```

The statement list is a collection of all the statements made within the program. A statement is essentially a string terminated by ";". We access all statements by iterating through all tokens until there are none left.
```# StatementList : Statement | StatementList Statement ;
def StatementList(self):
    statementList = []
    while self._lookahead != None:
        if self._lookahead["type"] == "}":
            break
        statementList.append(self.Statement())
    return statementList

# Statement : ExpressionStatement ;
def Statement(self):
    return self.ExpressionStatement()

# ExpressionStatement : Expression ';' ;
def ExpressionStatement(self):
    expression = self.Expression()
    self._eat(";")
    return {
        "type": "ExpressionStatement",
        "expression": expression
    }

```
In the StatementList method, we create a list of statements. We continue to append statements to this list until we encounter a token of type "}" or until there are no more tokens left.

The Statement method simply returns an ExpressionStatement.

In the ExpressionStatement method, we first get the expression and then consume the ";" token. We then return a dictionary representing the expression statement, which includes the type of the statement and the expression itself.

Now, let's delve into how we parse an expression. In our current setup, we only have literals, but typically, we would need to define the components of our language, such as IfStatement, ForLoopStatement, Block, etc. However, in this scenario, it consists solely of literals, which can be of two types of non-terminals: NumericLiteral and StringLiteral. We choose which one to parse based on the type of the lookahead. Parsing the literal simply involves advancing to the next lookahead by verifying that the current one is indeed of the correct type, using the eat method.

Here's the Program:
```# Expression : Literal ;
def Expression(self):
    return self.Literal()

# Literal : NumericLiteral | StringLiteral ;
def Literal(self):
    match self._lookahead["type"]:
        case "STRING": return self.StringLiteral()
        case "NUMBER": return self.NumericLiteral()

# NumericLiteral : NUMBER ;
def NumericLiteral(self):
    token = self._eat("NUMBER")
    return {
        "type": "NumericLiteral",
        "value": int(token["value"])
    }

# StringLiteral : STRING ;
def StringLiteral(self):
    token = self._eat("STRING")
    return {
        "type": "StringLiteral",
        "value": token["value"]
    }
```

In the end, all of these dictionaries return to the stack, and the final result appears something like this.

For the input: 10; "Hello Dumitru";

The AST would be:

```{
    "type": "Program",
    "body": [
        {
            "type": "ExpressionStatement",
            "expression": {
                "type": "NumericLiteral",
                "value": 10
            }
        },
        {
            "type": "ExpressionStatement",
            "expression": {
                "type": "StringLiteral",
                "value": "Hello Dumitru"
            }
        }
    ]
}
```

## Conclusion

In this lab, I constructed a parser for a simplified programming language, utilizing recursive descent parsing and lookahead strategies. By breaking down the process of parsing literals, such as numeric and string literals, I gained a deeper understanding of the core mechanics of a parser. This exercise enabled me to comprehend how to build abstract syntax trees (ASTs) from parsed tokens, offering a structural depiction of the program.

Furthermore, by implementing methods to manage non-terminals and progress tokens, I acquired practical techniques for creating parsers in programming languages like Python. These included error management mechanisms like checking for null tokens and ensuring expected tokens are consumed. Additionally, I understood the significance of clear code organization and documentation, as demonstrated by the use of Python methods to define non-terminals and comments to elucidate parsing logic.

In conclusion, this lab broadened my knowledge of parsing techniques and their use in language processing tasks. I gained valuable experience in implementing parsers, which can be expanded to manage more intricate grammars and language features in future projects.