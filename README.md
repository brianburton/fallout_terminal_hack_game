# Fallout Terminal Hacking Practice

This simple ruby program provides a practice session for learning the skills to solve the terminal hacking puzzles in Fallout 3 and 4.
When you run the program it leads you through a series of increasingly difficult hacking puzzles.
See if you can complete them all!

## Word Lists

Finding a good word list proved to be non-trivial.
Various open source lists exist but many contained trash words, lacked common suffix endings, contained hyphenated or compound words, etc.
However the open source [EOWL (EOWL-v1.1.2.zip)](http://dreamsteep.com/projects/98-software/53-the-english-open-word-list-eowl.html) word list proved to work extremely well.
The primary limitation of this list is the maximum word length of 10 characters.

For convenience and faster load times the distribution contains a single `words.txt` file containing all of the words.

To replace or modify the word list simply edit the words.txt file.  
Alternatively you could add other files and add appropriate load_file() calls in the ruby program.

## Game Play

The game creates a series of increasingly difficult puzzles.
Work your way through them one at a time.
Every puzzle consists of a list of words of the same length.
One of these is the password that you are trying to find.
The rest are duds.

For each puzzle you have 4 tries to pick the correct password.
Choosing the correct word successfully completes the puzzle.
Choosing the wrong word reveals the number of letters that the chosen word has in common with the real password, reduces your remaining tries, and gives you your next attempt.

If all tries are taken for a puzzle without finding the password the puzzle is considered failed and the game displays a new puzzle of the same difficulty for you to try.
You can fail puzzles many times without any other penalty than the lack of progress.

The game considers picking the password on the first try to be too lucky to stand and immediately gives you a new equivalent puzzle to solve.

The number of letters in common (called `likeness` in the program) is based on character position.  Two letters in common but at different positions do not count.  For example `ABLE` and `BALE` have only 2 characters in common, (the `LE` in positions 3 and 4) even though both are formed using the exact same 4 letters.

## Word Picking Advice

### Choosing The First Word

Before choosing your first word look at the list and see if there are any endings used in multiple words.  Common endings include `ING`, `ED`, `ERS`, etc.  If there are pick one of the words containing this ending.  This has several advantages:

- If the likeness is smaller than the length of the ending then you know that words with that ending are less likely to be the password.
- If the likeness is at least as large as the length of the ending then you know that words with the ending are more likely to be the password.

When there are several common endings in use in the puzzle try choosing a word with the longest one.
When there are no common endings in use in the puzzle just pick a word at random.

### Choosing Subsequent Words

Remember that for any word to be the password its likeness relative to each of your prior picks must match the likeness score of the pick itself.
For example, suppose you have tried two words so far:
````
    ARCHER (2)
    WODGES (2)
````

This is an interesting case where although both words have a likeness of 2 the words themselves do not have any 2 characters in common with each other.
This is a sign that the player didn't bother following this section's advice on the second pick!

Now suppose you were considering trying either `GRADED` or `GRATES`.  You can quickly see that `GRADED` can't be the password because it has only the `E` in common with both previous tries.  However `GRATES` has `.R..E.` in common with `ARCHER` and `....ES` in common with `WODGES` so it could potentially be the password.

## Ownership etc

- The ruby code is copyright 2017 Burton Computer Corporation
- EOWL is Copyright © J Ross Beresford
- Fallout 3 and 4 are owned by Bethesda Softworks
