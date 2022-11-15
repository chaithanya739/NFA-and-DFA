# NFA-and-DFA
from graphviz import Digraph
class NFA:
    def __init__(self, no_state, states, start, finals, transitions): #no_alphabet, alphabets, start,
                 #no_final, finals, no_transition, transitions):
        self.no_state = no_state
        self.states = states
        #self.no_alphabet = no_alphabet
        #self.alphabets = alphabets
         
        # Adding epsilon alphabet to the list
        # and incrementing the alphabet count
        #self.alphabets.append('e')
        #self.no_alphabet += 1
        self.start = start
        #self.no_final = no_final
        self.finals = finals
        #self.no_transition = no_transition
        self.transitions = transitions
        #self.graph = Digraph()

#def ortransition(length):                                                             #transition table insertion function
                
 #               return(s_transitions)

##def keilclosure(length):
    ##            for i in range(length+1):
      ##              if i == :
        ##                s_transitions.append([s_nfa[i],"E",s_nfa[length]])
          ##          if i == 1:
            ##            s_transitions.append([s_nfa[length-1], "E", s_nfa[i]])
              ##  return(s_transitions)







s_nfa = []                                    #Number of states
s_transitions = []                            #Number of Transitions
regular_expression = "(a|b)*abb(a|b)*"       #defining regularexpression
if "(a|b)" in regular_expression:
    re = regular_expression.replace("(a|b)*","s")  #changing the regularexpression for simplification

for i in range(len(re)):
    print(re[i])
    
    y1 = len(s_nfa)
    if  re[i] == 's':
        for i in range(y1,y1+8):
                k = s_nfa.append(i)
        for i in range(9):
                    if i%8 == 1:
                        s_transitions.append([s_nfa[i-1],"E",s_nfa[i]])
                    elif i%8 == 2:
                        s_transitions.append([s_nfa[i-1],"E",s_nfa[i]])
                    elif i%8 == 3:
                        s_transitions.append([s_nfa[i-1],"a",s_nfa[i]])
                    elif i%8 == 4:
                        s_transitions.append([s_nfa[i-3],"E",s_nfa[i]])    
                    elif i%8 == 5:
                        s_transitions.append([s_nfa[i-1],"b",s_nfa[i]])  
                    elif i%8 == 6:
                        s_transitions.append([s_nfa[i-1],"E",s_nfa[i]]) 
                        s_transitions.append([s_nfa[i-3],"E",s_nfa[i]])
                        s_transitions.append([s_nfa[i],"E",s_nfa[i-5]])
                    elif i%8 == 7:
                        s_transitions.append([s_nfa[i-1],"E",s_nfa[i]]) 
                        s_transitions.append([s_nfa[i-7],"E",s_nfa[i]])
        
                

    elif re[i] == 'a':
        y = len(s_nfa)+1
        for i in range(y-1,y):
            s_nfa.append(i)
        y1 = len(s_nfa)
        s_transitions.append([s_nfa[y1-2],"a", s_nfa[y1-1]])
        

    elif re[i] == 'b':
        y = len(s_nfa)+1
        for i in range(y-1,y):
            s_nfa.append(i)
        y1 = len(s_nfa)
        s_transitions.append([s_nfa[y1-2],"b", s_nfa[y1-1]])
print(s_transitions[10][0])
for i in range(13,23):
     s_transitions[i][0] = s_transitions[i][0] + 10
     s_transitions[i][2] = s_transitions[i][2] + 10
s_length = len(s_nfa)
s_nfa = list(map(str, s_nfa))
for i in range(len(s_transitions)):
    s_transitions[i] = list(map(str,s_transitions[i]))
s_nfa.pop()
print(s_nfa)
s_start = str(s_nfa[0])
print(s_start)
print(s_transitions)
print(s_length)
    
nfa = NFA(s_length,s_nfa,s_nfa[0],['17'], s_transitions)
nfa.graph = Digraph()
for x in nfa.states:
    if (x not in nfa.finals):
        nfa.graph.attr('node', shape = 'circle')
        nfa.graph.node(x)
    else:
        nfa.graph.attr('node', shape = 'doublecircle')
        nfa.graph.node(x)
nfa.graph.attr('node', shape = 'none')
nfa.graph.node('')
nfa.graph.edge('', nfa.start)
for x in nfa.transitions:
    nfa.graph.edge(x[0],x[2], label=(x[1]))
#nfa.graph.render('nfa', view=True)

##DFA conversion
## intializing the transition table
dfa = Digraph()
inputs = ['a','b','E']
inputs_dict = dict()
s_dfa_dict = dict()
for i in range(len(s_nfa)):
    s_dfa_dict[s_nfa[i]] = i
for i in range(len(inputs)):
    inputs_dict[inputs[i]] = i

s_transitiontable = dict()
for i in range(len(s_transitions)):
    for j in range(len(inputs)):
        s_transitiontable[str(i)+str(j)] = []
for i in range(len(s_transitions)):
    s_transitiontable[str(s_dfa_dict[s_transitions[i][0]])+str(inputs_dict[s_transitions[i][1]])].append(s_dfa_dict[s_transitions[i][2]])

## E-closure algorithm
dfa_epsilon = dict()
for i in s_nfa:
    epsilon_closure = dict()
    epsilon_closure[s_dfa_dict[i]] = 0
    epsilon_closure_stack = [s_dfa_dict[i]]
    while(len(epsilon_closure_stack) > 0):
        current = epsilon_closure_stack.pop(0)
        for x in s_transitiontable[str(current)+str('2')]:
            if x not in epsilon_closure.keys():
                epsilon_closure[x] = 0
                epsilon_closure_stack.append(x)
        epsilon_closure[current] =  1
    dfa_epsilon[i] = epsilon_closure.keys()
print(dfa_epsilon)

def dfa_name(dfa_name):
    name = ''
    for x in dfa_name:
        name = name + dfa_name[x]
    return name


dfa_stack  = list()
dfa_stack.append(dfa_epsilon[s_nfa[0]])

for i in dfa_stack:
    if i == '17':
        dfa.attr('node', shape = 'doublecircle')
    else:
        dfa.attr('node', shape = 'circle')

dfa.node(dfa_name(dfa_stack[0]))

dfa.attr('node', shape = 'none')
dfa.node('')
dfa.edge('', dfa_stack[0])

dfa_states = list()
dfa_states.append(dfa_epsilon[0])

while(len(dfa_stack) > 0):
    current = dfa_stack.pop(0)
    for i in range(len(input)-1):
        Mov = set()
        for j in current:
            Mov.update(s_transitiontable[s_dfa_dict[str(j)+str(i)]])
        print("Mov : ", Mov)

        if (len(Mov) > 0):
            dfa_E_closure = set()
            for x in list(Mov):
                dfa_E_closure.update(set(dfa_epsilon[s_nfa[i]]))
            print("E-closure: ", dfa_E_closure)
        
            if list(dfa_E_closure) not in dfa_states:
                dfa_states.append(list[dfa_E_closure])
                dfa_stack.append(list[dfa_E_closure])
        
            for i in dfa_E_closure:
                if i == '17':
                    dfa.attr('node', shape = 'doublecircle')
                else:
                    dfa.attr('node', shape = 'circle')
                dfa.node(dfa_name(list(dfa_E_closure)))

            dfa.edge(dfa_name(current),dfa_name(list(dfa_E_closure)),label = inputs[i])

        else:
            if (-1) not in dfa_states:
                dfa.attr('node', shape = 'circle')
                dfa.node('K')

                for i in range(len(inputs)-1):
                    dfa.edge('K', 'K', inputs[i])

            dfa.edge(dfa_name(current), 'K', label = inputs[i])

dfa.render('dfa',view=True) 

    
            










