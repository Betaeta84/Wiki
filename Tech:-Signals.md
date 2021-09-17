
## Signaler
Signals are a different dataflow metric within the kernel. However, unlike Channels if a signal is called many times very rapidly it will result in a single trigger of the signal. You are not guaranteed to see every packet, you only get to see the last everything listening is notified on an update to a particular signal.

The signaler schedules itself within the Scheduler, and provides functionality for `listen()`, `unlisten()` and `signal()`. This allows non-duplicating signals to sent, so other instances within the kernel ecosystem know they should update, if they listened for a specific signal. The signaler does not force every message to be delivered, only the last message. When a listener is attached initially, it will get the last message for that signal if it exists. Using these for GUI fields will then give the last message sent, even if the GUI window was launched after that signal was initially sent.

Signals are context dependent.
