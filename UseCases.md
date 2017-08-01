# Use Cases and Scenario’s

[back to main page](README.md)

## Goal and approach
Use case analysis is an intuitive way to understand and organize what the system should do, since it is based on user tasks and expresses the tasks in natural language. Use case analysis shows their strength when it is a good thing to mirror the way users worked before.

A use case is a typical sequence of actions that an actor performs in order to complete a given task.

We will use use case analysis to drive the tokenization analysis process. 

A scenario is an instance of a use case that expresses a specific occurrence of the use case with a specific actor operating at a specific time and using specific data. It can help to clarify the associated use case and bring it to life. 

We frame the scenario’s from the perspective of asset exchange – and we strongly suggest to develop use cases and scenario’s outside this exercise from the perspective of game development, fund management etc itself.

### Example

**Use case**: tops up lives during playing, paying with Google Play
**Steps**:

| Actor actions                                     	| System responses                   	|
|---------------------------------------------------	|------------------------------------	|
| A gamer uses his last life                 	        | Detects the gamer has 0 lives        	|
|                                                      	| Displays 'top up lives' screen     	|
| The user selects how many lives he would like to buy 	| Starts the Google Play buy feature 	|
| The user completes the purchase                      	| Displays how many lives were bought  	|
|                                                      	| Increases the user’s life count     	|
| The user goes back to the game                       	| Close the op up lives' screen      	|

**Scenario**:

| Actor actions                                                              	| System responses                   	|
|----------------------------------------------------------------------------	|------------------------------------	|
| Adam ran into a ghost while playing  Crypto Gravity and lost his last life 	| Detects Adam has 0 lives           	|
|                                                                            	| Displays 'top up lives' screen     	|
| Adam selects he wants to buy 30 lives                                        	| Starts the Google Play buy feature 	|
| Adam completes the purchase                                                	| Displays 'bought 30 lives'         	|
|                                                                            	| Increases Adam's life count by 30  	|
| Adam goes back to the game                                                 	| Close the op up lives' screen      	|

## Use case template - description

**Name**: A short, descriptive name
**Actors**: Who can perform this use case? (e.g., a gamer)
**Goals**: What are the actors trying to achieve? (e.g., the goal of paying for a life is to buy lives to continue playing)
**Preconditions**: What must be true before this can be done? (e.g., one should be registered with a game)
**Summary**: Short summary what happens.
**Related use cases**: Are they extremely similar to another use case? This is not necessarily a problem.
**Steps**: describe each step of the use case using a two-column format, with the left column showing the action taken by the actor, and the right column **showing the system’s responses.
**Postconditions**: What state is the system (read: the blockchain) after completion of this use case?

Please fill out between 10 and 20 use cases using the template on the next page. 
A good strategy for filling out the use cases is to work in this order:
1. generate a list of 5-10 goal
2. identify the actors and the summary
3. provide first input to the postconditions
4. describe the steps
5. describe the preconditions and the name
6. repeat step 3. and 4.

A markdown table editor for editing the steps in the tables can be found [here](http://www.tablesgenerator.com/markdown_tables#)

## Use case template

### Name

### Actors

### Goals

### Preconditions

### Summary

### Related use cases

### Steps

| Actor actions                                     	| System responses                   	|
|---------------------------------------------------	|------------------------------------	|
|                                                      	|                                    	|

### Postconditions