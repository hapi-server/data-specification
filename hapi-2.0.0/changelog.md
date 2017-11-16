Difference between 1.1 and 2.0: https://www.diffchecker.com/sBWweoDa

Summary of changes:
1. time values output by the server now must end with "Z" (not backwards compatable) [diff](https://github.com/hapi-server/data-specification/commit/9bcb2f43014a05380425c8e2be24b457da7c5542)
2. the "units" on the bins are required (not backwards compatable) [diff](https://github.com/hapi-server/data-specification/commit/30d8c967e252069a256019b71537694cd7fe7f97)
3. incoming time values (in request URL) should have a trailing "Z" to indicate GMT+0, but are interpreted as such even if there is no trailing Z [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
4. the length value for strings is now consistently described; all unused characters in the string should be filled with null characters (so no null terminator is needed if the string is exactly the given length)
[diff 1](https://github.com/hapi-server/data-specification/commit/4d8395578c42a7a20fd8cd0895c6cedb5b28abc7) [diff 2](https://github.com/hapi-server/data-specification/commit/32f2b219fdd6e8f7c33546077f9dc95f908b0752) [diff 3](https://github.com/hapi-server/data-specification/commit/c0aa8b9ca2f3c0d573a40019bdaf5c6a1edb1008) [diff4](https://github.com/hapi-server/data-specification/commit/533120bcd3622b58ae0f776f2e19fd854b4d3b54)
5. in CSV output, the use of quotes for string values with commas is clarified [diff](https://github.com/hapi-server/data-specification/commit/a1c444298f4cb0c7da2dfeb25b40879a80d100b0)
6. the subset of ISO8601 that we use is better described [diff](https://github.com/hapi-server/data-specification/commit/a7a528b455b57a02987fb50b5e8c4890b721e774)
7. added `timeStampLocation` [diff](https://github.com/hapi-server/data-specification/commit/9ee24e0e3f9c09243b0762664e13adbf446024b8) [diff](https://github.com/hapi-server/data-specification/commit/2603550251fefee14d4f2192095981c35753a2f3)
