* XXX just add some more utility functions and perhaps links to raw galaxy files implementing some hash algorithms and other weird things?
* XXX I'm not sure this section is even necessary? Perhaps just link to some large maps that use raw galaxy?
* XXX Perhaps explain more about static, and how to compartmentalize your code?
* XXX It might be useful to explain TriggerSendEvent and things like `DataTableGetString(false, TriggerEventParamName(EventGenericName(), p))`?
* XXX perhaps a list of which API entry points expect to start indexing at 0 and 1
* XXX perhaps a code example of how to loop and use userdata, because it's not obvious maybe?
* XXX what else isn't obvious?
* explain that there's no parallelism in Galaxy and multihreading is non-preemptive. outline the differences in context of sc2 + link some general explanation on the web
* remind that SC2 uses lockstep and explain how that applies to Galaxy - 16 ticks per seconds, explain what's the deal with wait(0.0) for /32 tick - and why it is a "fake". differences between real-time/game time.
* funcref/structref/arrayref

XXX add in any code from discord pins

XXX I imagine this file mostly as a collection of snippets, perhaps with some bread text above it to explain the code

??? tldr "Snippet title"
    ```c
    // like this
    ```
