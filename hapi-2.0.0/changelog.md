Version 1.2: e92919354889eef6c7538d81cf056add51694e7c

Here's a summary of what changed:
1. time values output by the server now must end with "Z" (not backwards compatable)
2. the "units" on the bins are required (not backwards compatable)
3. incoming time values should have a trailing "Z" to indicate GMT+0, but are interpreted as such even if there is no trailing Z
4. the length value for strings is now consistently described; all unused characters in the string should be filled with null characters (so no null terminator is needed if the string is exactly the given length)
[diff 1](https://github.com/hapi-server/data-specification/commit/4d8395578c42a7a20fd8cd0895c6cedb5b28abc7) [diff 2](https://github.com/hapi-server/data-specification/commit/32f2b219fdd6e8f7c33546077f9dc95f908b0752)
5. in CSV output, the use of quotes for string values with commas is clarified
6. the subset of ISO8601 that we use is better described
