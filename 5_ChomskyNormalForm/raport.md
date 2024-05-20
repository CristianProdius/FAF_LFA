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
Laboratory work nr. 5
</h1>

<p align="center"><i>my variant : </i><b> Variant 3</b></p>



## Objectives:

1. Learn about Chomsky Normal Form (CNF) [1].
2. Get familiar with the approaches of normalizing a grammar.
3. Implement a method for normalizing an input grammar by the rules of CNF.
    1. The implementation needs to be encapsulated in a method with an appropriate signature (also ideally in an appropriate class/type).
    2. The implemented functionality needs executed and tested.
    3. A BONUS point will be given for the student who will have unit tests that validate the functionality of the project.
    4. Also, another BONUS point would be given if the student will make the aforementioned function to accept any grammar, not only the one from the student's variant.




## Implementation description:
We begin by eliminating all states that lead to epsilon or states that eventually lead to epsilon. To accomplish this, I created two methods: one to identify which states ultimately lead to epsilon, and another to transform these states into their non-epsilon equivalents.

The getAllEpsilonNonTerminals method is a recursive function that checks if a production of a non-terminal contains a character related to epsilon. For instance, if a non-terminal leads to epsilon and another non-terminal leads to it, we perform a recursive call to check if any other non-terminals lead to this non-terminal. As we do this, we add all of them to a list and return it

```
def getAllEpsilonNonTerminals(self):
    epsilonNonTerminals = set()

    def dfs(non_terminal):
        for production in self.productions[non_terminal]:
            if "ImagineEpsilonIsHereBecauseLatexIsCringe" in list(production):
                epsilonNonTerminals.add(non_terminal)
                dfs(non_terminal)

    for non_terminal in self.productions.keys():
        if non_terminal not in epsilonNonTerminals:
            dfs(non_terminal)

    return epsilonNonTerminals
```

Now that we have a list of the non-terminals we need to normalize, we can start doing that using the getEpsilonEmptyProduction method.

```
def getEpsilonEmptyProduction(self, productions):
    res = set()

    def dfs(ignore=set()):
        if len(productions) == 1:
            return
        curr = ""
        for part_id in range(len(productions)):
            str_part_id = str(part_id)
            if str_part_id not in ignore:
                part = productions[part_id]
                curr += part
            if part.isalpha() and part == part.upper():
                dfs(ignore | set(str_part_id))
        if curr not in ["", productions]:
            res.add(curr)
        dfs()
        return res
```

In this section, we're implementing a backtracking solution to generate all combinations of a word, excluding a specific non-terminal (identified by its id, as there can be multiple ignores) and terminals. For example, for 'AXaD', we would generate 'XaD', 'Aa', 'AaD', 'aD', 'a', 'AXa', 'Xa'.

We then combine the two methods getAllEpsilonNonTerminals and getEpsilonEmptyProduction to eliminate all epsilon transitions:

```
def removeEpsilonTransitions(self):
    epsilonNonTerminals = self.getAllEpsilonNonTerminals()
    for non_terminal in epsilonNonTerminals:
        for production_id in range(len(self.productions[non_terminal])):
            production = self.productions[non_terminal][production_id]
            if len(production) > 1 and production != production.lower():
                self.productions[non_terminal].extend(list(self.getEpsilonEmptyProduction(production)))
```

Next, we eliminate single non-terminals by replacing their occurrences with their actual values. For example, if we have C: aB and A: C, we replace C in A with aB to get C: aB and A: aB:

```
def moveNonTerminals(self):
    for non_terminal, productions in self.productions.items():
        for production_id in range(len(productions)):
            production = productions[production_id]
            if len(production) == 1 and production != production.lower():
                productions.pop(production_id)
                productions.extend(self.productions[production])
    self.moveNonTerminals()```

    Finally, we replace terminals in productions with more than one terminal:

    ```class Iterator():
    def __init__(self, data, iterr=0):
        self.data = data
        self.iter = iterr

    def __iter__(self):
        return self

    def __next__(self):
        if self.iter < len(self.data):
            self.iter += 1
            return self.data[self.iter]
        else:
            raise IndexError("Iterator Out Of Bounds.")

    def reset(self):
        self.iter = 0

def replaceTerminals(self):
    data = ['All', 'Cyrillic', 'Letters', 'Here', '', '', '', '', '', '', '', '', '', '', '', '', '', '', '']
    iterator = self.Iterator(data)
    new_non_terminals = {}
    for non_terminal, productions in self.productions.items():
        for production_id in range(len(productions)):
            production = list(productions[production_id])
            if len(production) > 1:
                for item_id in range(len(production)):
                    item = production[item_id]
                    if item == item.lower():
                        if new_non_terminals.get(item) == None:
                            new_non_terminal = next(iterator)
                            new_non_terminals[item] = new_non_terminal
                        production[item_id] = new_non_terminals[item]
                productions[production_id] = "".join(production)
    for productions, non_terminal in new_non_terminals.items():
        self.productions[non_terminal] = [productions]
```

        This code replaces terminals in productions with Cyrillic letters, using an iterator to cycle through a list of Cyrillic letters. It creates a new non-terminal for each unique terminal and replaces the terminal with the new non-terminal in the production. The new non-terminals and their productions are then added to the grammar's productions.



## Conclusion

In this lab, I delved into the process of transforming context-free grammars (CFGs) into Chomsky Normal Form (CNF). CNF is a specific format for CFGs with strict production rules, making it useful for certain parsing algorithms. My aim was to devise a systematic approach for this transformation while understanding the fundamentals of formal grammars.

The conversion process presented several challenges. First, I worked on eliminating epsilon productions, which are productions that result in an empty string. I developed a recursive algorithm (getAllEpsilonNonTerminals) to identify non-terminals that can derive such productions. Then, I used a backtracking solution (getEpsilonEmptyProduction) to generate all possible combinations of productions, excluding those that lead to epsilon.

Next, I focused on removing unit productions, which are productions where a single non-terminal derives another single non-terminal. I created a recursive function (moveNonTerminals) that iteratively replaced these productions with the right-hand side of the referenced non-terminal, effectively eliminating unit productions.

To handle productions that exceed CNF's length restrictions, I mapped terminals to unique non-terminals using an Iterator class. This approach allowed me to isolate terminals within long productions and adhere to CNF's rules.

Finally, I ensured all production rules complied with CNF's requirement of having at most two non-terminals on the right-hand side. The groupSelfLiterals function achieved this by recursively combining pairs of non-terminals into new non-terminals, preserving the CNF structure.

Throughout this lab, I gained insights into manipulating formal grammars, the interaction of data structures and algorithms, and the step-by-step problem-solving process in computational linguistics. While my solution was successful, there's room for improvement. Providing formal correctness proofs would enhance the theoretical robustness of the work. Additionally, a detailed analysis of computational complexity could lead to potential optimizations.

In the future, I plan to add robust error handling mechanisms and explore alternative CNF conversion algorithms. This will further deepen my understanding of formal language theory and its practical applications.

