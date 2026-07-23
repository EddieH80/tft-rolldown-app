# tft-rolldown-app
Version Notes

V1
- None of the images for the units seem to be loading, could you maybe scrape them from this website? https://mobalytics.gg/tft/champions
- The traits for different units and the thresholds for them seem to be incorrect (for example, "Rebel" and "Slayer" are not traits in Set 17). Please consult https://mobalytics.gg/tft/champions for information on what traits each unit falls under.
- Need to be able to just hover mouse over the units on the board/bench and sell them by pressing E. You should not have to click on the unit to sell it, just hover your mouse over it and press E
- Similarly, you should not have to click on a unit on your bench and then click on a hex on the board to place it on your board, but rather just hover over the unit with your mouse and hold left click to drag the unit onto the board. The moving of units on the bench and board should follow how TFT handles things in game.
- The trait synergies should only be affected when units are placed onto the board, not when they are bought.

V2
- Images still not loading in the preview. Can you fix this?
- When the bench is full with 9 units, if there are two one star copies of a champion on the bench and you attempt to buy another copy of that champion from the shop, the units should be combined into a two star copy of the unit. The same idea applies to three starring a unit, if you have two two star copies of a champion and two one star copies of a champion on your bench while it is full, attempting to buy another copy of the champion from the shop should combine all the units into a three star copy of the unit
- The traits and images for units in the team planner are not showing. They should be displayed on the side of the team planner like the following attached image

V3
- There is now a bug where if there are 2 2-star units and your bench is full, you can buy that same unit from the shop but it doesn't appear on the bench or the board. The correct behavior should be that the unit is not able to be bought from the shop. If the bench also contains 2 1-star copies of the unit alongside the 2 2-star copies, then the player can buy the unit from the shop if the bench is full, and all the copies should combine into a 3-star copy of the unit
- When launching the app from the html file, the spacing of the shop is too large and completely covers the bench. The user should be able to clearly see the bench at all times

V4
- The board should be a hexagonal grid like the image below rather than a rectangular table of rows and columns

V5
- When copies of units combine, if any copies are on the board then the unit should be combined on the board not the bench
- When buying a unit in the shop would result in upgrading the unit to 2-star or 3-star, the unit should have a respective indicator in the shop

V6
- Once a player gets a 3* star unit, it should no longer appear in their shop anymore
- Displaying of traits in team planner should show also show partial, non-active traits in gray

V7
- Add a configuration option for infinite gold as a checkbox
- Sell value of units should be equal to 3 times the Star Level multiplied by unit cost, and then subtract 1 if the unit is not a one cost 
- Player should be able to drag and drop a unit from their board or bench into the store to sell it

V8
- Sell value of one-star units should be changed to just their unit cost
- Sell value of 2-star and 3-star units that are not one-costs should be changed to: 9 x Unit Cost - 1
- Sizing of unit icons in shop should be wider like the attached image

V9
- Sell value of 2-star units should be changed to 6 x Unit Cost - 1
- Images of units in the shop should be more zoomed out and cropped so that the more of the champion art is visible

V10
- Units in shop that would upgrade a unit to 2* or 3* should have a slow flashing effect
- Champion art should be zoomed out twice as much
- Sell value of 2-star units should be changed to 3 x Unit Cost - 1

V11
- Sell value of 2-star one cost units should be 3 x Unit Cost = 3. Right now it is 6
- The full body and head of the champion should show in the shop art
- Shop odds for different unit costs should be displayed above shop, with the text color corresponding to the unit cost

V12
- The listing of the active and non-active traits on the board should not move when the units are placed in different hexes
- The image portraits in the shop don't quite fill out the entire slot and are also incorrect for the current set. Please use images from https://mobalytics.gg/tft/champions instead and make the champion shop slots look like the attached image

V13
- Make the unit slots in the shop wider without rescaling the images so that they are more adjacent to each other. See the attached reference image 
- Add a fraction indicator for the number of missing units on board when board is not full. The numerator is the number of units on the board and the denominator is the player's level, which dictates the maximum amount of units on the board. See the attached reference image

V14
- There is a bug where the player cannot move any of the units on the board to empty hexes if the number of units on the board is equal to the player's level. The player should always be able to move units on the board to any of the hexes
- Make the indicator for units in the shop that are in the team planner more noticeable and larger
- The art for the 2 cost Meepsie should be the art used for the 4 cost The Mighty Mech. The art for The Mighty Mech should be https://raw.communitydragon.org/latest/game/assets/ux/tft/championsplashes/patching/tft17_galio_teamplanner_splash.tft_set17.png

V15
- Adding a copy of the same unit to the board should not increase the trait level for that units traits. Only unique units should count for the traits
- The "Target" indicator for units in the team planner that are in the shop is a bit too large and yellow. The indicator should just be a small icon in the top left, like in the attached image
- The traits should have an icon alongside the text for units in the shop and traits on the sidebar. References for what these trait icons look like can be found here: https://tactics.tools/traits

V16
- If the player clicks a unit in the shop but holds down the mouse, the unit should be bought if they drag and release the mouse outside of the shop. If they release the mouse inside the shop the unit should not be bought
- Next to the team planner, add button to clear out the team planner that can be used in the configuration screen and the rolldown screen
- When a trait reaches its second threshold level (e.g 4/6 Vanguard, 4/6 Bastion, 5/10 Meeple), the trait icon for it should change from bronze to silver. Similarly, when a trait reaches its third threshold level (e.g 6/6 Vanguard, 5/5 N.O.V.A, 6/10 Dark Star) the trait icon should go from silver to gold

V17
- When the player clicks and drags a shop unit to buy it, the units picture should follow the players mouse while they hold the mouse button
- I'm getting bug scenarios where clicking and dragging the unit in the shop to buy it doesn't always work
- The team planner should be able to add/remove units while in the rolldown screen
- Use the official SVG icons you were using before for the different trait levels (bronze, silver, gold) instead of a CSS filter

V18
- I updated the shop odds and pool sizes to match what was listed at https://www.metatft.com/tables/shop-odds
- Updated XP thresholds to match https://wiki.leagueoflegends.com/en-us/TFT:Experience
- I also updated the trait breakpoints to match https://tactics.tools/info/traits
- Please make sure that the current version of the file is updated with the changes in the uploaded file, and also make sure the bronze/silver/gold trait break points are accurate with the updated breakpoints

V19
- Add filters for the team planner to let the user organize by units that are in a specific trait to add them to their planner. Traits that are unique (only 1 unit has that trait) should not be a filter
- Add a checkbox in the configuration screen for including Zed in the unit pool or not for 5 costs. Zed is a special unit that can only be unlocked through an augment, so without the augment the player should not see him while rolling in the game

V20
- Add sound effects from the actual TFT game for actions like rerolling the shop, buying a unit, placing a unit on the board, buying exp, and selling a unit

V21
- Remove champion voicelines when buying the units
- Please use the official TFT in-game sounds for rerolling, buying, etc.

V22
- When buying EXP, when the player levels up the EXP numerator should go to 0 and the denominator should be the amount of EXP needed to reach the next level. Currently the EXP numerator remains from the previous level, but it should be reset to 0 upon level up
- One slow flash that fades away should appear on units that are on the bench or the board when they appear in the shop

V23
- Make sure the EXP thresholds match the numbers in the table at: https://wiki.leagueoflegends.com/en-us/TFT:Experience. Level 7 to 8 is for sure incorrect and going to level 9 makes the XP denominator infinity. When a player hits level 10, the can no longer buy EXP and the EXP fraction should go away

V24
- The thresholds from the table are being misinterpreted. When a player is at level 4, 0 EXP, the must buy 10 EXP to hit level 5. Since EXP is bought in increments of 4, the player spends 12 gold to buy 10 EXP, which results in the being Level 5 with 2/20 EXP progress to Level 6. The player then needs 18 EXP to get to level 6, which means they need to spend 20 gold to buy 20 EXP, leaving them at Level 7 with 2 / 36 EXP bought. Please mirror this behavior for all levels, especially at level 8, 9, and 10 where the player will need to spend 60, 68, and 68 again to reach each level.

V25
- Improvements to sound effects
- Fixing leveling bugs