# Missionaries-and-Cannibals

˚ ༘✶ ⋆｡˚ ⁀➷ ˚ ༘✶ ⋆｡˚ ⁀➷ ˚ ༘✶ ⋆｡˚ ⁀➷ ˚ ༘✶ ⋆｡˚ ⁀➷

Coding in Visual Studio Code with Python language

┌───── •✧✧• ─────┐
    -SOLUTION- 
└───── •✧✧• ─────┘

State Problem

Missionaries and Cannibals

1. Tree missionaires and 3 cannibales in right side of the river
2. Only one boat with capacity for two people
3. At any side of river, the number of cannibals can not be higher than the number of missionaires

Cannibals =< Missionaries

Otherwise cannibals can eat the missionaries.

1. State

First approach
state (ML, CL, MR, CR, Boat).

Second approach
state (ML, CL, Boat).

MR = 3 - ML
CR = 3 - CL

2. Movements
    2.1. Move one missionaire to the left
    2.2. Move one cannibal to the left
    2.3. Move one missionaire to the left
    2.4. Mone one cannibal to the right
    2.5. Move two missionaire to the left
    2.6. Move two cannibals to the left
    2.7. Move two missionaire to the right
    2.8. Move two cannibals to the right
    2.9. Move one mis and one cannibals to the right
   2.10. Move one mis and one cannibals to the left

     state (ML, CL, Boat).

  First approach
mov (oneMisLeft, state(ML, CL, right), state (ML2, CL)) :-
     MR is 3 - ML,
     MR > 0,		
     ML2 is ML + 1.

  Second approach
mov (move(M, C, left), state (ML, CL, right),
     state (ML2, CL2, left)) :-
     ML2 is ML + M,
     CL2 is CL + C.


mov (move(M, C, right), state (ML, CL, left),
     state (ML2, CL2, right)) :-
     ML2 is ML - M,
     CL2 is CL - C.


  move (1,1,_) .
  move (2,0,_) .
  move (0,2,_) .
  move (1,0,_) .
  move (0,1,_) .

  valid(state (3,3,_)) .
  valid(state (0,0,_)) .
  valid (state (1,1,_)) .
  valid (state (2,2,_)) . 
  valid (state()) .

  not_valid (estate (2,3,_)).
  not_valid (estate (1,3,_)).
  not_valid (estate (1,2,_)).
  not_valid (estate (2,1,_)).
  not_valid (estate (1,0,_)).
  not_valid (estate (2,0,_)).

3. Path to the solution

path (+InitialState, +FinalState, +Visited, -Path)
this is true if Path unify with the listh of movements to go from
InitialState to FinalState without repeating visited states.

Path = [move (1, 0, left), move (1, 0, right), ...]


Ini ---------------> Final
            Path

Ini --> mov --> Aux
                         ---------------> Final
                               path
                                N-1
*/ 

  path (Ini, Ini, _, [] ) .
  
  path (Ini, Fin, Visited, [move (M, C, Dir) | Path]   ) :-
      mov (move (M, C, Dir),
      mov (move (M, C, Dir), Uni, Aux),
      \+ not_valid (Aux),
       path (Aux, Fin, [Aux|Visited], Path).


States : combination of missionaries and cannibals and boat on each side of river.
Initial State: 3 missionaries, 3 cannibals and the boat are on the near bank
Successor Function : Move boat containing some set of occupants across the river (in either direction) to the other side
Constraint : Missionaries can never be outnumbered by cannibals on either side of river, or else the missionaries are killed. 
Action : Raid the boat with maximum two persons (one or two) across the river (in either direction) to the other side
Goal Test : Move all the missionaries and cannibals across the river.
Path cost : Requires minimum number of moves

0 Initial setup:             MMMCCC   B   – 
1 Two cannibals cross over:    MMMC   B   CC 
2 One comes back:     MMMCC     B       C 
3 Two cannibals go over again:   MMM   B   CCC 
4 One comes back:     MMMC       B   CC 
5 Two missionaries cross:    MC    B   MMCC 
6 A missionary & cannibal return:   MMCC  B        MC 
7 Two missionaries cross again:   CC    B   MMMC 
8 A cannibal returns:           CCC      B      MMM 
9 Two cannibals cross:             C            B   MMMCC 
10 One returns:                     CC     B   MMMC 
11 And brings over the third: -             B   MMMCCC

