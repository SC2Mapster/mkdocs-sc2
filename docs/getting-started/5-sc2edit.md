Now it's time to set up the files and folders to work from and in. This can be a bit tedious, but you only need to do it once.

### Initialize Font Styles

The first step, unless your SC2 project already contains modified Font Styles, is to create the Font Styles file and system. Unless you do this from `sc2edit` first, it will complain later. If your map already contains changes to Font Styles, you do not need to perform this step.

1. Tab to sc2edit, where your project should already be open
1. Hit `F8` to open the Text module (menu `Modules - Text`)
1. Change to the `Font Styles` tab
1. Right-click in the left side of the window, select the `Add Style...` menu item
1. Insert any name, like `lolplzblizz`, and set any template on it, and hit `OK`
1. Hit `c-S` to save the project
1. Close the Text module

You will never open the Text module again. They say there are no rules without exceptions, but they are mistaken. There is no need to ever open the Text module again.

Blizzard themselves do not use the Text module.

You _can_ use the Text module, it is just a less efficient method of accomplishing your work.

### Initialize UI XML

The second step is to initialize what sc2edit calls the `DescIndex` and related user interface XML files. If your map already contains changes to the DescIndex, you do not need to perform this step.

1. Hit `s-F6` to open the UI module (menu `Modules - UI`)
1. Right-click anywhere in the left side of the window, select the `Add Layout...` (*NOT* `Add Frame...`) menu item
1. Insert any name, like `wtfbbq`, and hit `OK`
1. Hit `c-S` to save the project
1. Close the UI module

And just like with the Text module, the UI module will _never again be opened_ as long as you work on your SC2 project. For eternity, or until Blizzard patches it to be useful. Which they will never do.

Blizzard themselves do not use the UI module.

### Initialize the Data

Thirdly, you need to make any change to the Data, to initialize the folders and setup on the file side. If your map already contains changes to the Data, you do not need to perform this step.

1. Hit `F7` to open the Data module (menu `Modules - Data`)
1. Open any tab, it doesn't matter which; Units, Effects, Behaviors, Abilities, anything
1. Select any data entry on the left hand side
1. Remember which tab and entry you modified, so you can revert it later.
1. Change any field on the right hand side from its default value to some other value
1. Hit `c-S` to save the project
1. Close the Data module

98% or more of your Data work will be done in sc2edits Data module. There are only a few exceptions where it makes sense to edit Data externally.
