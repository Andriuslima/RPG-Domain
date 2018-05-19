# PDDL Formalization
:beginner: Classical Planning Assignment

**Felipe Meneguzzi**  
**Mauricio Magnaguagno (PhD Student)**  
**Ramon Fraga Pereira (PhD Student)**

Domain adapted from the [2016 The Fifth International Competition on Knowledge Engineering for Planning and Scheduling (ICKEPS)](https://helios.hud.ac.uk/scommv/ICKEPS/Scenarios.pdf), credits to [Dr. Lukas Chrpa](https://helios.hud.ac.uk/scomlc/) for the scenario.

Overview
=========
The hero woke up in a dungeon full of monsters and traps (perhaps the party last night went wrong...) and needs your help to get out. 
Here are basic facts about the dungeon:

- The dungeon contains rooms that are connected by corridors (the dungeon can thus be represented by an undirected graph);
- Each room can either be empty, have a monster, a trap in it, or a sword in it (this is an exclusive or, so only one object per room); and
- One of the empty rooms is the “goal”: it has an exit, so the hero can escape;

The hero is lucky, since he has full knowledge about the dungeon's layout.  
He is not that lucky, though, because just after the hero leaves room s/he just visited, the room is destroyed and cannot be visited again. 

The hero can perform the following actions, but only if s/he is alive!

- The hero can move to an adjacent room (connected by a corridor) that has not been destroyed (i.e., the hero has not yet visited the room);
- The hero can Pickup the sword if it is present in the room the hero is currently in and the hero is empty handed;
- S/He can also destroy the sword the hero currently holds. However, this can have unpleasant effects if done in a room with a trap or a monster;
- Finally, the hero can disarm a trap, if there is a trap in the room the hero is in and the hero is empty-handed (does not hold a sword), then the hero can disarm it.

However, there are some (dangerous) constraints the hero has to consider:
- If the hero enters a room with a monster in it, s/he has to carry a sword (so the monster is afraid of him/her), otherwise the monster kills him/her. Notice that the hero is a pacifist, so s/he cannot kill the monster;
- If the hero destroys the sword in a room with a monster in it, the monster kills him/her as well;
- The only action the hero can safely perform in a room with a trap in it is the “disarm a trap” action. Any other action (even moving away) triggers the trap, which kills the hero.

Whenever we need to use automated tools to solve problems on our behalf, we must provide a consistent specification of the *transition system* of the underlying problem. 
If we specify the problem poorly, we jeopardise the planner software ability to generate valid responses, leading to false negatives and unnecessarily long waiting times for the planner at best, or incorrect plans at worse. 
Thus, specifying the RPG domain in PDDL gives you a chance to develop your skills in designing consistent transition systems, helping you avoid bugs in the software you will write in the future to handle all kinds of other processes.

Your assignment is to develop a domain file from the specification above, and then model the situations depicted in the images below as individual problem files.

Problem Instances
=================

The image below represents the problem instances that you need to model in PDDL, once you are done encoding your domain file. 
The text that accompanies each image is self-explanatory; remember, you must model corridors between rooms, rooms for the hero to move through, and you can only move between connected rooms and visit each room only **once**. 
We specify problems so that cells stand for rooms and edges between them represent corridors. 
“Init” is the hero's initial position, “Goal” is hero's desired goal position, “Sword” indicates a sword, “Monster” is a monster, and “Trap” stands for trap.

### Problem 1

![Problem 1](fig/RPG-pb1.png "Problem 1")
---

### Problem 2
![Problem 2](fig/RPG-pb2.png "Problem 2")
--

### Problem 3
![Problem 3](fig/RPG-pb3.png "Problem 3")


Miscellaneous Advice
====================

Here are some lessons we learned in creating our own solution and writing papers/reports:

-   [JavaGP] does not parse “or” conditions or effects. You need to specify conditions / effects as a list of predicates bound together by a single “and”.

-   As a first step, you should at least look at the sample domain and problem files given at the beginning of this specification.
    Additionally, here is a link to [PDDL domain and problem files](http://icaps-conference.org/index.php/Main/Competitions) (for scenarios different from the one you need to model in this project), from past iterations of the International Planning Competition (IPC).

-   The best way to figure out how to model a domain and associated problems is to look at these examples. If you feel the need for documentation, here is a paper that talks about the complete PDDL specification, with BNF specification at the end (Appendix A): [PDDL 2.1 Specification](http://www.public.asu.edu/~ktalamad/tmp/files/pddl21specs.pdf)

-   Using type predicates increases memory usage and slows down the creation of ground atoms but speeds up the (dominant) time to solve the problem (PDDL, however, allows typing of parameters);

-   Tables and graphs are a useful tool to show runtime performance of software, take a loot at [GnuPlot](http://www.gnuplot.info/ "GnuPlot is an excellent (and free) graph making software");

-   In order to evaluate the performance of a planning encoding, you need to specify problems with most of the parameters locked in, and measure runtime as one parameter increases (e.g. number of locations, number of containers, etc);

-   Pasting your entire domain specification (or problems) into the paper does not count as content (now you cannot say you were not warned);

-   Overly large figures used to simply fill space in the report are also not a good idea;

-   Reviewers have a more pleasant reading experience when papers are generated using <span style="font-variant:small-caps;">LaTeX</span>, it is very easy to spot the difference.

### Most common requirements
- ``:strips`` - default requirement (positive preconditions, add and delete effects)
- ``:negative-preconditions`` - able to use ``not`` in preconditions
- ``:equality`` - able to use ``=`` in preconditions
- ``:typing`` - able to type objects and variables, ``obj - type``
- ``:disjunctive-preconditions`` - able to use ``or`` in preconditions
- ``:conditional-effects`` - able to use ``when`` in effects

### Downgrading PDDL
- Some planners may lack the desired requirements
- Sometimes we can rewrite the description using simpler constructions
- ``:negative-preconditions``
  - Duplicate predicate and use antonym instead, sometimes you can remove the original predicate
    - ``(not (clean ?space))`` => ``(dirty ?space)``
- ``:equality``
  - Add an equality predicate ``(equal obj obj)`` for each object at the initial state and replace preconditions
- ``:typing``
  - Move types to initial state and parameters to preconditions
    - ``(?var - type)`` => ``(type ?var)``
- ``:disjunctive-preconditions``
  - Break each part of the precondition into different actions

Other Planning Software
=======================
- [Fast Downward](http://www.fast-downward.org)

- [JavaFF](https://github.com/Optimised/JavaFF)
  - Local planner, port of FF (C to Java)
  - Installation steps
    - Download and unpack zip
    - Check if java and javac are available in your path (JDK)
    - ``cd JavaFF/src``
    - ``javac javaff/*.java``
  - Write domain and problem in separate files and use their path as arguments
    - ``java javaff/JavaFF ../problems/depots/domain.pddl ../problems/depots/pfile01``

- [JavaGP]
  - Local planner

- [Planning Domains](http://editor.planning.domains/#)
  - Online planner, editor and planning API
  - Write both domain and problem in the editor
    - New files can be created from the **File** top menu
    - Files are listed in the left, double click to rename
  - Click **Solve** in the top menu
    - Select domain and problem files before clicking **Plan**
    - Bottom right corner will display information about planning
    - Output file is generated, contain plan or error message

- [STRIPS-Fiddle](https://stripsfiddle.herokuapp.com/)
  - Online planner and editor
  - Write both domain and problem in the code editors
  - Click **Run** in the top menu
 
- [Web-planner](http://web-planner.herokuapp.com/) - This is the online planner we are developing and would like you to test
  - online planner, editor and visualizer.
  - The editor supports syntax highlighting for PDDL and shows both domain (left) and problem (center) at the same time.
  - The solve button calls the planner and shows the output (right).
  - The planner supports the following requirements:
    - ``:strips`` as the basic requirement for preconditions and effects
    - ``:negative-preconditions`` to use not in preconditions
    - ``:equality`` to use ``=`` in preconditions
    - ``:typing`` to define type of objects and parameters, ``?var - typeofvar``

[JavaGP]: https://github.com/pucrs-automated-planning/javagp "Project page at Github"
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA1MDQxODE2MiwtODUxMDk5NjgwXX0=
-->