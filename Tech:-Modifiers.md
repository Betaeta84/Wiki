
## Modifiers
Modifiers provide functionality for a particular context. The assumption is modifiers will alter the context or provide a link to the main code. For example `Spooler` provides .spooler at the given context as place to send cutcode. Modifiers also get called `boot()` when the kernel boots at start-up. These are always called the name of the modifier itself. So `modifier/Spooler` is opened as `modifier/Spooler`
