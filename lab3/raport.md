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
Laboratory work nr. 3
</h1>

<p align="center"><i>my variant : </i><b> Variant 3</b></p>

```
p = {
    'S' : ['dB', 'A'],
    'A' : ['d', 'dS', 'aAdAB'],
    'B' : ['aC', 'aS', 'AC'],
    'C' : [''],
    'E' : ['AS']
}

vn = ['S', 'A', 'B', 'C', 'E']
vt = ['a', 'd']
a = vt
```

## Objectives:

1. Understand what lexical analysis is.
2. Get familiar with the inner workings of a lexer/scanner/tokenizer.
3. Implement a sample lexer and show how it works.

---

## On Lexical Analysis and Scanners

For the purposes of this laboratory, lexical analysis means the
transformation of a sequence of characters into a series of lexical
tokens.
The program/algorithm that does this is called the lexer (or scanner),
and it is the main character of this here laboratory.

---

## Implementation description:

The class new 'Lexer' was created, that opens a text file where the
code in the given language is.

The particular code that will be used, looks like this:

```commandline
BEGIN
v
INPUT
I 011. Q 011;
OUTPUT
Q 011. M 011;
RAM
CN01.AND I 011. Q 011 . CN04.NOT Q 011. I 011;
I 011 := NOT I 011
END
```

How the algorithm works, is by mapping the grammar into a dictionary:

```python
self.grammar = {
            'BEGIN': 'keywords',
            'END': 'keywords',
            'INPUT': 'keywords',
            'OUTPUT': 'keywords',
            'RAM': 'keywords',
            'I': 'variables',
            'Q': 'variables',
            'M': 'variables',
            'AND': 'logic gates',
            'OR': 'logic gates',
            'XOR': 'logic gates',
            'NOT': 'logic gates',
            'CN01': 'contacts',
            'CN02': 'contacts',
            'CN03': 'contacts',
            'CN04': 'contacts',
            ':=': 'operators',
            ';': 'separators',
            '.': 'separators'
        }
```

From the code file, the algorithm separates the code into individual
words, by use of the '.split()' method, and splits them again if there
are any points or semicolons.

From there on, using the dictionary, the words are tokenized, according
to their keyes. If the word is numeric, it also tokenized, despite not
being in the dictionary.

If, however, the word is not in the grammar or a numeric one, it is
defined as 'unknown'.

The end result, i.e. the token list, for the provided code looks like
this:

```commandline
[['keywords', 'BEGIN'], ['unknown', 'v'], ['keywords', 'INPUT'],
['variables', 'I'], ['numeric', '011'], ['separators', '.'],
['variables', 'Q'], ['numeric', '011'], ['separators', ';'],
['keywords', 'OUTPUT'], ['variables', 'Q'], ['numeric', '011'],
['separators', '.'], ['variables', 'M'], ['numeric', '011'],
['separators', ';'], ['keywords', 'RAM'], ['contacts', 'CN01'],
['separators', '.'], ['logic gates', 'AND'], ['variables', 'I'],
['numeric', '011'], ['separators', '.'], ['variables', 'Q'],
['numeric', '011'], ['separators', '.'], ['contacts', 'CN04'],
['separators', '.'], ['logic gates', 'NOT'], ['variables', 'Q'],
['numeric', '011'], ['separators', '.'], ['variables', 'I'],
['numeric', '011'], ['separators', ';'], ['variables', 'I'],
['numeric', '011'], ['operators', ':='], ['logic gates', 'NOT'],
['variables', 'I'], ['numeric', '011'], ['keywords', 'END']]

```

## Conclusionj

This Python code is part of a lexer, a program that takes a sequence of characters and breaks it down into a sequence of tokens.

The Lexer class has a tokenize method that processes a list of unorganized tokens (self.unorganized_tokens), which are obtained by reading a file and splitting its contents.

The tokenize method does the following:

It checks if a token has a dot somewhere in the middle (not at the ends). If it does, the token is split into three parts: the part before the dot, the dot itself, and the part after the dot. These parts are inserted back into the list of unorganized tokens.

It checks if a token has a semicolon somewhere in it (not at the ends). If it does, the token is split into two parts: the part before the semicolon and the semicolon itself. These parts are inserted back into the list of unorganized tokens.

It classifies each token based on the defined grammar (self.grammar). If a token is in the grammar, it is classified accordingly. If a token is numeric, it is classified as 'numeric'. If a token is not in the grammar and is not numeric, it is classified as 'unknown'.

The classified tokens are appended to self.token_list in the form of a list with two elements: the classification and the token itself.

Finally, the method returns self.token_list, which is a list of classified tokens.

This lexer can be used as part of a compiler or interpreter for a programming language, to convert source code into a format that is easier to analyze and process.
