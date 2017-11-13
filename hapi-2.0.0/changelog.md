Version 1.2: e92919354889eef6c7538d81cf056add51694e7c

Here's a summary of what changed:
* time values output by the server now must end with "Z" (not backwards compatable)
* the "units" on the bins are required (not backwards compatable)
* incoming time values should have a trailing "Z" to indicate GMT+0, but are interpreted as such even if there is no trailing Z
* the length value for strings is now consistently described; all unused characters in the string should be filled with null characters (so no null terminator is needed if the string is exactly the given length)
* in CSV output, the use of quotes for string values with commas is clarified
* the subset of ISO8601 that we use is better described
