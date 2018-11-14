The following terms may or may not be written as for example; sc2edit, "sc2edit", or `sc2edit`, depending on who writes the specific part of the documentation.

* `sc2edit` refers to the Starcraft II Editor, no particular part of it
* `Data Editor` or `Data module` refers to the part of `sc2edit` that deals with the Data, accessible by the default shortcut `F7` from `sc2edit`
* `Trigger Editor` or `Trigger module` refers to the part of `sc2edit` that deals with Triggers, accessible by the default shortcut `F6` from `sc2edit`
* `UI Editor` or `UI module` refers to the part of `sc2edit` that deals with UI editing. You will never use it.

And so forth.

### Keybindings / Shortcuts

Hotkeys/Keyboard shortcuts on this site are referred to by their shorthand. Mac users figure it out on your own. There are a few key bindings in the SC2 client that are exceptionally useful for user interface development - the binding for showing the interface inspector, and the binding for reloading the interface from disk. XXX: I have rebound both from the standard bindings, and would rather type out this sentence than do a simple discord search or start SC2 to find the standard bindings. Which means someone else should enter them and remove this notice.

For example, `c-s-a-P` is `Ctrl+Shift+Alt+P`, `c-s-F` is `Ctrl+Shift+F`, and `c-P` is simply `Ctrl+P`.

Lowercased letters `c`, `s`, and `a`, refer to the _modifier keys_ `Ctrl`, `Shift`, and `Alt`. `Tab` has no shorthand.

A shortcut listed as `c-K-O` means "While holding down `Ctrl`, hit `K` followed by `O`".

Though shortcut keys are listed as uppercase, you should not hold down `Shift` or enable `Caps Lock`. It is just a way of distinguishing them from the modifier keys.

### SC2Components

`SC2Components` and `SC2Map` are two sides of the same coin. `SC2Map` is, in laymans terms, a ZIP-file containing all the files that go into making a SC2 map. `SC2Components` is simply the unzipped version - again, in laymans terms.

If the above paragraph made sense to you, that's really all there is to it.

There is no magic otherwise. Some people in the community are under the impression that one is *different* to the other in weird ways that noone can comprehend. This is utter nonsense. The difference is archived vs not archived.

When you click `File - Save As` in `sc2edit`, you are presented with the option of saving as `.SC2Components`. Always do this.

!!! warning "A note on publishing"
    When you are ready to publish your map to Battle.net, it does not let you publish unarchived projects. At this point, you simply save your project as an `.SC2Map` or whatever type is relevant to your project, then publish, and then close the map again.
    This is the only inconvenience from working unarchived, and frankly it means nothing.

