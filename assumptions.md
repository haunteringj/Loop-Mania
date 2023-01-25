# COMP2511 Major Project: Loop Mania
### Assumptions

### UML Diagram
- The goals are associated with the character AND the application
- Vampire is associated with VampireCastle building
- Zombie is associated with ZombiePitt
- Character is associated with Shop


### Game world
- There is always one complete loop that has the hero’s castle on some tile along the path.
- No entity can spawn outside of the tiles provided for the game world.

### Goals
- Goals cannot have two AND goals and two other AND goals OR'ed together for e.g. (Exp && Gold) || (Exp && Loops)

### Character
- Character attacks after an (one) allied soldier has attacked. 
- Character attacks before the enemies. 
- The character will only travel clockwise around the world path.
- Characters can equip 2 weapons and 2 equipments at one time. The weapons must be different types and the equipments must be different types.


### Allied Soldier
- The allied soldier(s) will take all damage inflicted from enemies.
- Only when there are no allied soldiers, the character will be wounded in battle.
- A player character can attain multiple allied soldiers at a time.
- The allied soldier will always attack first, even before the player character.


### Items
- Potions can be sold for gold.
- When the character loses all its health, if the player has ‘The One Ring’ item, then that item will be removed from the inventory and the character will have full health.
- ‘The One Ring’ item can only be obtained after killing an enemy
- The character can only hold onto one ‘The One Ring’ item until it has been used.
- DoggieCoin fluctuates in price for every tick of the world (kind of like in the stock market).


### Enemies
- There can be one or more enemies in a single battle.
- Only one enemy can occupy a path tile
- Vampires can move the fastest amongst the slugs and zombies
- Vampires critical rate (A float value between 0 and 1)
- Zombies critical rate (A float value between 0 and 1)
- In a battle with more than one enemy, when the player wins the rewards distributed by each individual enemy are added together.
- Enemies attack after all allied soldiers and the character have attacked.
- Enemies travel anti-clockwise through the game loop (towards the character)


### Battle
- An enemy (enemy1) may battle if EITHER: 
1. The character is in enemy1's battle radius.
2. Another enemy is currently battling the character and the character is in the support radius of the enemy1.
E.g. If a vampire is battling the character and the character is in the support radius of a zombie, then the zombie will also battle the character.
- How to check if a character is in an enemy's battle radius is calculated as follows:  
`Let dx = difference between enemy's x-position and character's x-position.`  
`Let dy = difference between enemy's y-position and character's y-position.`  
`If (dx)^2 + (dy)^2 <= (enemy's battle radius)^2 --> the battle ensues`  
** Why? **  
By squaring dx and squaring dy then adding the results, we get the square of the hypotenuse formed by a right-angle triangle with sides dx and dy.  
The hypotenuse of the triangle is the radius-distance between the character and the enemy.   
Therefore, if the square of the hypotenuse <= square of the enemy's battle radius, then the character is in the enemy's battle radius.  
- A supporting enemy will support a currently battling enemy if the character is in its support radius (Whether a character is in the enemy's support radius is calculated the same as above - except replace battle radius with support radius).
- The sequence of battle is: 
1. The soldier and enemy battle. 
2. If there is an allied zombie then the character and allied zombie battle (i.e. the soldier from step 1 that turned into a zombie).
3. If the enemy from step 1 survives then the character and enemy attack.
4. The character fights all supporting enemies. 
- If the soldier has turned into a zombie: it immediately attacks the character and the two battle until either entity die. That is, the character will fight the allied zombie before it attacks the enemy that turned the soldier into an allied zombie. [further explanation of step 2 and step3 of the battle]
- The soldier and enemy battle will end on 3 conditions: the soldier has died, the enemy has died or the soldier has turned into a zombie.
- The enemy and character battle will end on 2 conditions: the character has died or the enemy has died.
- The character and supporting enemies battle will end on 2 conditions: the character has died or ALL supporting enemies have died.
- A soldier-turned-zombie will keep its original statistics (from being a soldier).
- In the character and enemy battle, the battle ends if either side die.
- If an enemy is in trance, it will fight the supporting enemies for the character for a given number of trance attacks (randomised).
- An enemy in trance will keep its original statistics. The only thing that changes is it battles for the character.
- The vampire can inflict a critical bite the first time it attacks the soldier/character.
- When Elan Muske is in battle with the character, he will heal ALL current enemies in the world state.



### Weapon 
- The chance of a trance being inflicted by the staff is randomised (between 0 - 20%).
- When in a trance, the enemy deals up to 10 attacks against other enemies.
- Sword increases character's attack by 10
- Staff increases character's attack by 2
- Stake increases character's attack by 5. It inflicts double the damage (in total 10) on a vampire.
- Anduril increases character's attack by 4 against all enemies. It also triples the character's TOTAL attack when battling against a boss.


### Equipment
- The Helmet, Shield and Armour defensive items will increase the player’s defense statistic
- Each equipment piece has an endurance attribute that will cause the equipment to break after the character has participated in multiple battles (to be implemented in Milestone 3)
- Armour increases character's defense by 5
- Shield increases character's defense by 7
- Helmet increases character's defense by 2 but also decreases character's attack by 2
- TreeStump increases character's defense by 10 against all enemies. It increases the character's defense by an extra 25 when battling against a boss.


### Consumable
- Health potion increases character's health by 20

### Buffs
- A character can withhold multiple buffs such as increases in damage or defense.
- The ‘trance’ effect will disappear after a certain amount of turns


### Buildings 
- A building can exist without a card (the Hero’s castle) 
- Buildings will despawn corresponding to their degredation counter.
- Two cards cannot be placed on the same tile. 
- Traps will not harm the character if the character walks over it.
- Tower will harm the enemy if it is in range (even when the character is not in battle with the enemy)


### Inventory management 
- Any items dropped by the enemies will appear in the top-left most slot of the inventory slots that are available.
- If the inventory is full, then the incoming item will be sold for experience and gold.
- Only one item can be stored per slot.
- Any building card dropped by enemies will appear in the left-most slot in the card slots that is available.
- If the card slot is full, then the incoming card will be sold for experience and gold.


### Shop
- The shop will stock all items in standard mode (never runs out of stock).
- In Survival Mode, the player only has the options to either buy 1x potion, sell potions in its inventory or sell the DoggieCoin. 
- In Berserker Mode, the player only has the options to either buy 1x randomised piece of equipment, or sell the DoggieCoin. 
- Equipment sold in the shop will be randomised and only two will be available to the character every time they open the shop.
- Each item in the shop can be bought in bulk (0..* items can be bought at one time).
- Items can only be sold out of unequipped inventory (can't sell equipped items).
- The rare items cannot be sold. 


### Frontend
- The size of the game application is modelled for the game to be played on desktops.
- The human player is using a mouse and keyboard in order to interact with the user interface.
- The language of the game is in English and all text entities will be in English.
- If the player does not select a gamemode, the default mode is standard mode. 
- If the player does not select a character class, the knight is selected as the default class. 

### Confusing mode
- A rare item is chosen at the creation of the world to bind to other rare items. 
