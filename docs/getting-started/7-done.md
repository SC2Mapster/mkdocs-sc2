## Back in sc2edit

Make sure you save all files in VSCode, tab back to sc2edit, and reopen your project.
Now, the only setup you need to do, is the following:

1. Open the Trigger editor (`F6`)
1. Right-click anywhere in the left side pane
1. Select `New - New Custom Script` (`c-a-T`)
1. Name it "Bootstrap" (or whatever, obviously)
1. Put this line of code in it: `include "scripts/main"`
1. At the bottom of the window, there is a single-line text entry called `Initialization function (optional)`, in this textbox, simply enter `main`
1. Done

This makes the map execute the `main()` function in the file `scripts/main.galaxy` at game start. Try hitting `c-F12` to Compile and `c-F9` to run your map at this point.

Once in game, try entering `-help`, `-help lol`, and `-lol` as in the chat box.

## Done

* XXX add moar UI stuff and comments in overrides.sc2layout, also paths to common frames like minimap etc
* XXX probably add a custom unitframe and show how to add it to a unit somewhere?
* XXX add in functions from the util library (by Alevice I think?) that is linked in some discord pin?
* XXX add in more galaxy util code mostly for reference
* XXX add a section about userdata, and compare the speed of userdata lookups to data lookups

Congratulations, you're now set up for success!
