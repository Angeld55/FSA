


<h1 id="finite-state-automation">Finite state tool (FST)</h1>
Finite state tool (FST) is a tool for working with different **finite-state machines** and many algorithmic constructions.
The projects consists of four components:

 1. Finite state automation 
 2. Pushdown automation 
 3. Context-free grammars 
 4. Minimal acyclic subsequential transducer

*1-3* are developed for the **Languages, Automata and computability** course at the **Faculty of Mathematics and Informatics, Sofia university**.
All algorithms are direct implementations of the shown constructions [here](https://github.com/Angeld55/Math_courses_seminars_notes_and_tasks/tree/master/Languages,%20Automata%20and%20computability/Seminars_Notes) and **may not have optimal time and space complexity** 

*4* is developed for an advanced course of the same topic and it's tested with real-world data.

**1-3 and 4 should be compiled separatly!**


<h1 id="finite-state-automation"> FST interface and commands</h1>

![enter image description here](https://i.ibb.co/x1Bcm4R/afl-example1.png "AFL")


![enter image description here](https://i.ibb.co/w67BSMP/afl-example2.png "AFL")


| Syntax:                          | Effect:                                                                      | Example:            |
|----------------------------------|------------------------------------------------------------------------------|---------------------|
| load [config_file]               | loads configuration from a file. Example files in Example_config_files       | load myfsa.fst      | 
| environment fsa                  | prints information for all registered fsa-s                                  | environment fsa     |
| regex [fsa1]                     | returns a regex for the automation.                                          | regex a1            |
| fsa [name]                       |  registers an automation with one state with this name.                      | fsa test1           |
| fsa [name] [regex]               | registers an automation with this name for this regex.                       | fsa test2 a*(a+b)*b |
| print [name]                     | prints the automation.                                                       | print test1         |
| vis [name]                       | generates an  html file with an image of the automation.                     | vis test1           |
| accepts [fsa] [string]           | returns true if the fsa accepts the string and false otherwise               | accepts a1  aabbb   |
| add_state [fsa]                  | adds a new state to the automation                                           | add_state a1        |
| make_final [fsa] [state]         | makes the state final                                                        | make_final a1 0     |
| arc [fsa] [start] [end] [symbol] | adds a new transition from state start to state end with this symbol         | arc a1 0 1 a        |
| det [fsa]                        | fsa becomes deterministc                                                     | det a1              |
| [fsa1] = det [fsa]               | registers an automation fsa1 which is the deterministic fsa2                 | a1 = det a          |
| min [fsa]                        | fsa becomes deterministic, total and minimal                                 | min a1              |
| [fsa1] = min [fsa]               | registers an automation fsa1 which is the deterministic and minimal fsa2     | a1 = min a2         |
| [fsa1] = union [fsa2] [fsa3]     | registers an automation fsa1 which is the union of fsa2 and fsa3             | a1 = union a2 a3    |
| [fsa1] = concat [fsa2] [fsa3]    | registers an automation fsa1 which is the concatenation of fsa2 and fsa3     | a1 = concat a2 a3   |
| [fsa1] = intersect [fsa2] [fsa3] | registers an automation fsa1 which is the intersection of fsa2 and fsa3      | a1 = intersect a2 a3|
| star [fsa1]                      | fsa1 becomes an new automation for the Kleene star of the old fsa1 automation| star a1             |
| [fsa1] = star [fsa2]             | registers an automation fsa1 which is the Kleene star of fsa2                | a1 = star a2        |
| compl [fsa1]                     | fsa1 becomes an new automation for the complement of the old fsa1 automation | compl a1            |
| [fsa1] = compl [fsa2]            | registers an automation fsa1 which is the complement of fsa2                 | a1 = compl a2       |
| reverse [fsa1]                   | fsa1 becomes an new automation for the reverse of the old fsa1 automation    | reverse a1          |
| [fsa1] = reverse [fsa2]          | registers an automation fsa1 which is the reverse of fsa2                    | a1 = reverse a2     |
| regex [fsa1]                     | returns a regex for the automation.                                          | regex a1            |   
| environment npda                 | prints information for all registered npda-s                                 | environment npda    |
| npda [name]                      | registers an Pushdown automation with one state with this name.              | npda test1          |
| add_state [npda]                 | adds a new state to the npda                                                 | add_state test1      |
| make_final [npda] [state]        | makes the state final                                                        | make_final test1 0  |
| arc [npda] [start] [symbol] [stack_top] [end] [replace_stack]| adds a new transition from state start to state end   | arc test1 0 a # 0 A# |
| accepts [npda] [string]           | returns true if the npda accepts the string and false otherwise             | accepts test1  aabbb   |
| cfg [name] [rules]		   | Creates a context-free grammer. The rules are separated with a space.         | cfg test1 S->aSb&#124;$   |
<h1 id="vt">Visualization tool</h1>

The FST system supports automata visualization. The command is called **vis**. 

The files *header.x*, *footer.x* and the *graphVisualiser_files* folder should be in the **same folder** as the executable.

![enter image description here](https://i.ibb.co/XjFBFc5/afl-example3.png "example of vis")

This commands generates a **html file** (with the name of the automation). You can find it in the main folder.

![enter image description here](https://i.ibb.co/ckfn3yG/sss.png "main folder")

The **html file** looks like this:

![enter image description here](https://i.ibb.co/WBWdtSC/3.png "html page")

Press the **single button** on the html page and the automation will be visualized.

![enter image description here](https://i.ibb.co/nwZXtsv/4.png "vis aut")

<h1 id="finite-state-automation">1. Finite State Automation</h1>

![enter image description here](https://i.ibb.co/FKQgLBg/69973577-352536238957668-4630865521305190400-n.png "example of FSA")


<p>Finite state automation is A = &lt;Q,Σ,s,F,δ&gt;, where<br>
Q - is a finite, non-empty set of states<br>
Σ- is the input alphabet (a finite, non-empty set of symbols).<br>
s - is an initial state, an element of Q<br>
F- is the set of final states, a  subset of Q<br>
δ- is the state-transition function</p>
<h1 id="creation">Creation</h1>
<p>Diffrent ways of creating an automation:</p>

```c++
int main()
{

	// Automation for a(a+b)*
	FiniteStateAutomation A("a(a+b)*");
	
	std::string computation;
	cout << A.accepts("abbb", computation) << endl; //true;
	cout << A.accepts("bbba", computation) << endl; //false

	FiniteStateAutomation A2; //Only one state with index 0
	
	A2.addState(); //Adds state with index 1
	A2.addTransition({"0", "1", "a"});
	A2.addTransition({"1", "1", "a"});
	A2.addTransition({"1", "1", "b"});


	return 0;
}
```


<h2 id="control">Control</h2>

<table>
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>addState()</td>
<td>Adds a new state. Returns the index of the new state.</td>
</tr>
<tr>
<td>addTransition(start,end,ch)</td>
<td>Creates a new transition from state start to state end with the letter ch. May become from DFA to NFA.</td>
</tr>
<tr>
<td>changeStartState(newStart)</td>
<td>Changes the start state of the automation. Returns false if the given new start state doesn’t exist and true if it was successful.</td>
</tr>
<tr>
<td>makeStateFinal(state)</td>
<td>Marks the given state as final. Returns false if the state doesn’t exist.</td>
</tr>
<tr>
<td>accepts(word, computation, shouldReturnComputation)</td>
<td>Returns true if the word is accepted by the DFA/NFA. If the third parameter is true, then the computation will contain the successful (if such exists) path </td>
</tr>
<tr>
<td>isEmptyLanguage()</td>
<td>Returns true if the FSA doesn't accept any strings.</td>
</tr>
<tr>
<td>removeNotReachableStates()</td>
<td>Removes all states that are not reachable from the start state..</td>
</tr>	
</tbody>
</table><h2 id="properties">Properties</h2>

<table>
<thead>
<tr>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>makeTotal()</td>
<td>Makes the automation complete. It shoud define a transition for each state and each input symbol. We create an error state for every non-existing transition.</td>
</tr>
<tr>
<td>makeDeterministic()</td>
<td>Makes the Non-deterministic finite automaton(NFA)  to Deterministic finite Automation(DFA).</td>
</tr>
<tr>
<td>minimize()</td>
<td>Transforms the given DETERMINISTIC finite automaton into an equivalent DFA that has a minimum number of states.</td>
</tr>
<tr>
<td>getRegEx()</td>
<td>Gets a regular expression for the language accepted by the NFA. The regular expression may be non-practical as it becomes long very fast.</td>
</tr>
</tbody>
</table><h2 id="operations">Operations</h2>
All basic operations with NFA-s are available:
<table>
<thead>
<tr>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>Union(A1,A2)</td>
<td>Concat(A1,A2)</td>
<td>KleeneStar(A1)</td>
<td>Complement(A1)</td>
<td>Intersect(A1,A2)</td>
<td>Reverse(A1)</td>
</tr>
</tbody>
</table>
<h2 id="example">Example of creating and minimizing a FSA</h2>

```c++
#include "FiniteStateAutomation.h"

int main() 
{
	//Regex: a(a+b)*b + b(a+b)*a
	FiniteStateAutomation A("a(a+b)*b + b(a+b)*a");

	A.print(); //Image 1 and 2

	A.minimize();

	A.print(); // Image 3 and 4.

   	return 0;
}

```


<p>The first print:<br>
<img src="https://i.ibb.co/yfQytWy/1.png"><br>
Without the unreachable states it looks like:<br>
<img src="https://i.ibb.co/k9GPG1K/2.png" alt="image2" title="image2"><br>
And after minimizing the automation, the second print:<br>
<img src="https://i.ibb.co/M6LN38w/4.png" alt="" title="image3"><br>
It looks like this:<br>
<img src="https://i.ibb.co/qk6hTfq/3.png" alt="" title="image4"></p>
<h2 id="example">Example of getting a regex from FSA</h2>

```c++


int main() 
{	
	// FSA for ab(a+b)*
	FiniteStateAutomation A("ab(a+b)*");
		
	A.minimize(); //better to be minimized, so the regex would be simple.

	A.print();
	
	cout << A.getRegEx();

   	return 0;
}
```
Here is the FSA:


![
](https://i.ibb.co/Hzr0pmM/71001177-717157422041121-5409095346024349696-n.png "FSA to regex &#40;example 2&#41;")


And the result:


![
](https://i.ibb.co/frkg9K6/Capture.png "regex example")


**ab+(ab(a+b)\*(e+a+b))** 

**= ab + (ab(a+b)\*)**  *(since e,a and b are in (a+b)\*)*

**= ab(a+b)*** *(since ab is in ab(a+b)*\*)

<h1 id="NPDA">2. Non-deterministic Pushdown Automation</h1>

![
](https://i.postimg.cc/3rqMn6gS/PDA.png "regex example")

<p>.Non-deterministic Pushdown Automation is P = &lt;Q,Σ,G,#, s,F,δ&gt;, where<br>
Q - is a finite, non-empty set of states<br>
Σ- is the input alphabet (a finite, non-empty set of symbols).<br>
G-  finite set which is called the stack alphabet.<br>
# - initial stack symbol. <br>
s - is an initial state, an element of Q<br>
F- is the set of final states, a  subset of Q<br>
δ- is the transition function</p>

**! The empty string/symbol is $ !**

**Each rule(transition) looks like this:**

****<current state, read symbol from the tape, top of the stack, destination state, string to replace the top of stack>****

* Example 1: <0,'a', 'A', 1, "BA"> From state 0 to state 1, we push B to the stack 
* Example 2: <0,'b', 'A', 3, "$"> From state 0 to state 3, we pop A from the stack (since $ is the empty string) 

**Example for NPDA for { ww^rev | w in {a,b}* }**
```c++
// Example for Nondeterministic pushdown automata for { ww^rev | w in {a,b}* }
int main()
{
	NPDA PA(3); //3 initial states

	PA.makeStateFinal(2);

	PA.addTransition({ "0", "a", "#", "0", "A#" });

	PA.addTransition({ "0", "b", "#", "0", "B#" });
	PA.addTransition({ "0", "$", "#", "2", "$" });

	PA.addTransition({ "0", "a", "A", "0", "AA" });
	PA.addTransition({ "0", "a", "A", "1", "$" });

	PA.addTransition({ "0", "b", "B", "0", "BB" });
	PA.addTransition({ "0", "b", "B", "1", "$" });

	PA.addTransition({ "0", "b", "A", "0", "BA" });
	PA.addTransition({ "0", "a", "B", "0", "AB" });

	PA.addTransition({ "1", "a", "A", "1", "$" });
	PA.addTransition({ "1", "b", "B", "1", "$" });

	PA.addTransition({ "1", "$", "#", "2", "$" });

	string computation;
	std::cout << PA.accepts("abba", computation) << std::endl; //true
	std::cout << PA.accepts("abbb", computation) << std::endl; //false
	std::cout << PA.accepts("aaabbbbbbaaa", computation) << std::endl; //true
}

```
<h1 id="NPDA">3. Context-Free Grammar</h1>

![
](https://i.postimg.cc/DzDG07Vk/CFG.png "regex example")

We can input out context-free grammar (CFG) and it will simulate it using NPDA.

**Example for simulating the CFG:**

**S->aSc|B**

**B->bB|$**

```c++
int main()
{
	//Example Nondeterministic pushdown automata  from a context-free grammar
	// S->aSc | B
	// B->bB | $
	// L(S) = { a^n b^k c^n | n,k \in N}
	ContextFreeGrammar cfg;
	cfg.grammarRules.push_back("S->aSc|B");
	cfg.grammarRules.push_back("B->bB|$");

	NPDA PA2(cfg);

	std::cout << PA2.accepts("abc", true) << std::endl; //true
	std::cout << PA2.accepts("aaaaaabbbbcccccc") << std::endl; //true
	std::cout << PA2.accepts("abcc") << std::endl; //false
}
```
<h1 id="NPDA">4. Minimal acyclic subsequential transducer</h1>

The project contains algorithms for creating and maintaining a minimal acyclic subsequential transducer for a finite function **f : Σ\* -> N**
![enter image description here](https://i.ibb.co/KDvmLyG/pic.png)
(MAST for (a,11), (ab, 3), (ac 1))

The project consist of three functions:

 - Building and MAST from a given **sorted** list of pairs 
 <key, value>.
 It follows the aproach in **[Mihov and Maurel, 2001]** Mihov, S. and Maurel, D. (2001). Direct construction of minimal acyclic subsequential transducers.
 - Inserting words to the MAST from a given list of pairs 
 - Removing words to the MAST from a given list of pairs

 All the algorithms can be easy modified for a function of the type **f : Σ\* -> Σ***

The check for equivalent states is done with a proper hash.
Each state is hashed using it's transitions, output and finality.
More information about the hash on SubsequentialTransducer.h:18
