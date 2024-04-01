# The title of the work

### Course: Formal Languages & Finite Automata

### Author: Prodius Cristian

---

## Theory

Alphabet is a finite, nonempty set of symbols. The string is a finite sequesne of symbols from the alphabvet. The string wwithout any symbols is called an empty string. The grammar G is an ordered quadruple
G=(VN, VT, P, S) where:
 VN- is a finite set of non-terminal symbols;
 VT - is a finite set of terminal symbols;
VN  VT = 
 S  N is a start symbol;
 P – is a finite set of productions of rules

There are a few types of Grammer like Turing machines, Liner-bounded automata, push down automata and finite state automata.
BNF is the way of writinga grameer to difine the languages.

Context free grammer can be presenbted as type 2 as Chomsky clasification.

The language of a CFG is the set of all strings at the end of a derivation.

Ambiuguity means when gramer generates the same string but with different languages.

The finite automata is a mothematical model of comutation.

There are two types of finite automata: Deterministic Finite Automaton (DFA) and Non-deterministic Finite Automaton (NFA). In a DFA, for each state, there is one transition for each symbol in the alphabet. In an NFA, there can be zero, one, or more transitions from a state for a symbol.

## Objectives:

Discover what a language is and what it needs to have in order to be considered a formal one;

Provide the initial setup for the evolving project that you will work on during this semester. You can deal with each laboratory work as a separate task or project to demonstrate your understanding of the given themes, but you also can deal with labs as stages of making your own big solution, your own project. Do the following:

a. Create GitHub repository to deal with storing and updating your project;

b. Choose a programming language. Pick one that will be easiest for dealing with your tasks, you need to learn how to solve the problem itself, not everything around the problem (like setting up the project, launching it correctly and etc.);

c. Store reports separately in a way to make verification of your work simpler (duh)

According to your variant number, get the grammar definition and do the following:

a. Implement a type/class for your grammar;

b. Add one function that would generate 5 valid strings from the language expressed by your given grammar;

c. Implement some functionality that would convert and object of type Grammar to one of type Finite Automaton;

d. For the Finite Automaton, please add a method that checks if an input string can be obtained via the state transition from it;

## Implementation description

- The code defines a context-free grammar (CFG), generates strings from that grammar, and then converts the CFG into a finite automaton (FA). It uses the FA to test whether certain strings are accepted by the automaton, based on the defined grammar rules. The results of the string generation and acceptance tests are printed out.

- Explanation of the code.

The code starts by defining the variables VN, VT, and P. These represent the non-terminal symbols, terminal symbols, and production rules of a CFG, respectively.

The Grammar class is defined. It represents a CFG and has a method generate_string that generates a string from the grammar by recursively applying the production rules.

An instance of the Grammar class is created using the previously defined VN, VT, and P. This instance represents a specific CFG.

The code generates five strings from the grammar and prints them out. This is done by calling the generate_string method of the Grammar instance.

The function grammar_to_finite_automaton is defined. This function takes a Grammar instance as input and converts it into a finite automaton. The conversion is done by creating a state in the FA for each non-terminal symbol in the CFG, and a transition in the FA for each production rule in the CFG.

The FiniteAutomaton class is defined. It represents a FA and has a method accepts that checks if a given string is accepted by the automaton. This is done by starting in the start state of the FA and following the transitions based on the characters in the string. If the FA ends in an accept state after processing the entire string, the string is accepted.

The code tests a list of strings to see if they are accepted by the FA. This is done by converting the Grammar instance into a FiniteAutomaton instance and calling the accepts method for each string. The results are printed out.

## Conclusions / Screenshots / Results

as for conclusion we can see that regular grammas cand be reprresented as finite automata

The code is designed to do this:

1. Generate strings based on the defined context-free grammar (CFG). The generated strings are printed out.

2. Convert the CFG into a finite automaton (FA).

3. Test a set of predefined strings ("ab", "aba", "abba", "abab", "abcb") to see if they are accepted by the FA. The acceptance or rejection of each string is printed out.

## References
