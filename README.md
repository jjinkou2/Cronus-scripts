# Cronus-scripts

hi, 

i would like to share algorithms to people who would like to program more efficiently this tiny beast cronus max. 

doax3 grin XP is a script that help you level up your XP for the game Dead or alive xtreme3.

The whole script  stays in only ONE slot and takes half of memory.

------------------------
Ok lets see how it 's done: 

The cronus max is a microcontroller, that translates into power but fewer memory than PC or smartphone. It has also 
a peculiar language script that will bang your head into walls sometimes. But i took that more of a challenge 
than a burden. This device will oblige you to think out of the box. 

So instead of whinning on memory lack, just leverage on its assets : power and script features
To reach to goal of less memory, this scripts uses 3 points: 

- The 1rst one is the most important : DATASET, use and abuse it. that's how you keep microcontroller not a jabba the hut memory eater.
- 2nd: "main is a loop". I repeat "MAIN IS A LOOP"! i'll come back on this point. 
- 3rd: State automata machine is the key to this peculiar language. 

1. Dataset
---------

Ok lets go back to the 1rst point. Dataset is a bunch of unorganised bytes. So it's up to you to 
organise it. Forget your python list / ruby object /C structures. You 'll have to structure it yourself. 
For this algorithm i just needed:
  - a entry point (cb_intro),
  - a set of combo
  - and a Id for the end of combo (EOC).
```
     //  Idx, Button,       Wait(x50ms),     Event
     //---------------------------------------------------------------
     //      Introduction
     data(
        cb_intro,
            PS4_CIRCLE,      80,         //  Travel to zack island
            PS4_CIRCLE,      100,        //
            PS4_OPTIONS,     10,         //  skip intro video
            PS4_OPTIONS,     100,         //
        EOC,
     ...
```
then it needs a function that read that kind of structure : "function get_combo_index(combo_id)" 
I copied it from a Mortal Kombat script found at cronus forum. I then wrote the State "Play_combo"
to read the combo till EOC. 

this dataset efficiently replaces the repetitive and memory greeding: 
	combo_run(PS4_XX,100);wait...

2. Main Loop
-------------

lets move on the 2nd point: MAIN IS A LOOP. You don't need While/ForLoop. That's pretty challenging
at first: how can i loop / branch to somewhere in my code ? I don't know you, but for me that's
really interessing when you have to give away your habits and explore new paths. 
Ok if main is a loop, so there's a way to loop my combo/function/code in some sort of way. 
Have you read the manual ? no ?so i'll let you re-read the chapter Combo. yes it is clearly written how too loop. 
 
ok you are lazy : here's the code you can find in the combo section 

[Cronus Manual](http://cronusmax.com/manual/combo_section.htm)

```C++
	int run_combo = 0;
 
	main {
 
	    if(event_press(19)) {       //If A / Cross is pressed...
		run_combo = 5;          //Variable 'run_combo' equals 5
	    }

	    if(run_combo && !combo_running(mycombo)) {  //If 'run_combo' has a value and mycombo is not running...
		run_combo = run_combo - 1;              //'run_combo' equals 'run_combo' minus 1
		combo_restart(mycombo);                 //restart mycombo
	    }
 
	}
 
	combo mycombo {
	    set_val(3, 100);    //set RB / R1 to 100
	    wait(200);          //wait 200 milliseconds
	    wait(200);          //wait 200 milliseconds
	}
```

3. State machine
-----------------

Now the Last point : State automata machine. Franckly, without state automata, Cronus scripts look like 
a raging bull. Now with this tool, i take it by the horns. Look at the code after the if (play). 
with State programming, you develop small modules that you will plug each one. 


     +------------------+              +------------------+
     |                  |              |                  |
     |  Previous State  |      +-----> |  Previous State  |
     |                  |      |       |                  |
     +------------------+      |       +------------------+
     |                  |      |       |                  |
     | Combo To Run     |      |       | Combo To Run     |
     |                  |      |       |                  |
     +------------------+      |       +------------------+
     |                  |      |       |                  |
     |  Next State      +------+       |  Next State      |
     |                  |              |                  |
     +------------------+              +------------------+


# Happy coding ! 


