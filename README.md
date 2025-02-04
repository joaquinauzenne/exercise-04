Program a Word Game
Learning Objectives

Loading data files into R
Breaking a programming challenge down into discrete steps
Writing novel functions
Using arguments in functions
Using set operation functions
Using loops and conditional statements
Working with different data structures (vectors, tabular data)
Practicing data wrangling
Dealing with keyboard input

Wordle Puzzle Challenge
The rules of Wordle are simple: A player has SIX tries to guess a 5-letter word that has been selected at random from a list of possible words. Guesses need to be valid 5-letter words. After each guess, a player receives feedback about how close their guess was to the word, which provides information they can use to try to solve the puzzle. This feedback lets you know whether each letter your guess is either [1] in the solution word and in the correct spot, [2] in the solution word but in the wrong position, or [3] not in the solution word. In app/browser versions of Wordle, this feedback is provided visually using colors, but that need not be the case.

This week’s programming exercise is to work with a partner to get as far along as you can writing R code that allows you to play Wordle to your heart’s content using R/RStudio!

The assignment and steps below were inspired by this fun blog post… but before you reference it or other online sites, try to tackle this coding exercise on your own.

Preliminaries
Set up a new GitHub repo in you or your partner’s GitHub workspace named “exercise-04” and clone that down to both of your computers as a new RStudio project. The instructions outlined as Method 1 in Module 6 will be helpful. Make sure that both you and your partner are collaborators on the repo and that you add me as a collaborator as well (my GitHub username is “difiore”).

Go to https://github.com/difiore/ada-datasets, download the following two data files, and add them to your repo:

collins-scrabble-words-2019.txt
google-10000-english-usa-no-swears.txt
The first of these files (279,497 lines long) contains a list of “Official Scrabble Words” in the English language based on the Collins English Dictionary published by HarperCollins. The first line in the file is the word “words” and is used as a header.

The second of these files (9885 lines long) contains a list of ~10,000 of the most common words in the English language, based on data compiled by Google, and omitting common swear words. Again, the first line is the word “words” and is used as a header.

Create a new Quarto or RMarkdown document, called “wordle.qmd” or “wordle.Rmd”. In this, you do your best to recreate all of the wordplay used in Wordle.
Introduction
Before we begin programming, an important first step is to break down the problem we are trying to down into discrete pieces… What do we need to do to set up a Wordle game? What steps does game play need to follow? What has to be evaluated at each step? How does the game end?

As an early step, for example, we need to choose a mystery “solution” word that players will try to guess. We will use the list of the most common words in the English language to do this as a possible source of solution words for the puzzle.

Additionally, also need to establish a dictionary of “valid” words that players can guess, and for that we will use the list of Official Scrabble Words.

NOTE: The list of possible “solution” words used in the original Wordle puzzle consists of ~2100 5-letter words, while the list of “valid” words that can be used as guesses totals ~13,000. How many 5-letter words are in each of the two data files you have downloaded?

Step 1
Create your own custom function called load_dictionary() that takes a single argument, “filename”, that can be used to read in either of the two data files your downloaded.

Once you have created your function, use that function to create two variables, solution_list and valid_list, that, respectively contain vectors of possible solution words and valid words to guess. That is, you should be able to run the following to create these two vectors:

valid_list <- load_dictionary(<filename here>)
solution_list <- load_dictionary(<filename here>)

Running str(valid_list) should return…

chr [1:279496] "AA" "AAH" "AAHED" "AAHING"...

Running str(solution_list) should return…

chr [1:8336] "THE" "OF" "AND" "TO"...

Step 2
Winnow your variable solution_list to only include words that are included in valid_list. There are multiple ways that you could do this, but the set operation function, intersection() is an easy way. Use R help to look at the documentation for the intersection() function to see if you can get that to work. How many words are in your updated solution_list vector?
Step 3
Write a custom function called pick_solution() that [1] removes all words from solution_list that are not 5 letters in length, [2] then randomly chooses a single word from those that remain, and [3] then splits that word into a vector of single-character elements. You should be able to pass your solution_list vector as the argument to the function.
HINT: For [1], you will want to subset the solution_list vector according to some criterion of word length. For [2], you may find the sample() function useful (use R help to look up documentation on that function). For [3], you may want to look at the functions strsplit() from {base} R or str_split() from the {stringr} package (part of {tidyverse}). Pay attention to the data structures that those functions return, because you will need to carefully reference one of the elements that is returned in order to wind up with a vector!

As a bonus, you might include a second argument for your pick_solution() function called “word_length” that makes your function flexible enough to select a solution word that is something other than 5 characters long.

Once your function works, run it and assign the result to a variable called solution.

solution <- pick_solution(solution_list)

Step 4
Now, to tackle the bulk of the problem, create two more functions. The first should be called play_wordle() and it should take three arguments: [1] the answer to the puzzle (the value of your solution variable), [2] a list of valid guesses (the contents of your valid_list variable), and [3] a value for “number of guesses”, which you should set to the original Wordle game default of 6.

HINT: Here is possible skeleton code for that function.

play_wordle <- function(solution, valid_list, num_guesses=6){
      <function code here>
    }

Think carefully about what the play_wordle() function needs to do. It should:

At the onset, tell the player the rules of the game, e.g., “You have … chances to guess a word of length …”

Display what letters the player has not yet guessed (at the onset, this would be all 26 letters of the alphabet), e.g., “Letters left: …”

HINT: There is a built-in dataset in R called LETTERS, which contains the 26 capital letters in the English alphabet and another, letters, that contains all the lowercase letters. You will probably want to use either the toupper() or tolower() functions to ensure that you are always working with the same formatted letters in words and guesses.

# using the LETTERS built-in vector
LETTERS

##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
## [20] "T" "U" "V" "W" "X" "Y" "Z"
# using the `toupper()` function
toupper("change my case")

## [1] "CHANGE MY CASE"
Prompt the player for a guess, e.g., “Enter guess number …”, read in their guess, and check that their guess is valid (i.e., that it contains the correct number of letters and is a word included in the “valid” word list).
HINT: The readline() function, which can take a character string as an argument, will provide a “prompt” entering a line of numeric or character data. Hitting <enter> or <return> signals the end of the line.

guess <- readline("Enter some data here, then press <enter>: ")

Compare the guess to the solution word and generate the necessary feedback, e.g., * for in the word and in the correct position, + for in the word but in the wrong position, and - for not in the word. For this step, try writing a separate “helper” function, evaluate_guess(), called from within play_wordle(). This function should take, as arguments, the player’s guess and the value of the solution variable. This is probably the trickiest part of the problem to program, and there are lots of approaches you might take to evaluating guesses. After you work on this for a while, I can share one solution.

Update the list of letters not yet guessed.

HINT: Again, consider using set operations to update the list of letters not yet guessed. setdiff() is a function that returns the difference between two vectors.

Check if the puzzle was solved. If so, the function should indicate that the player WON the game and print out their guess and feedback history. If not, the function should prompt the player for another guess, unless they have already hit the maximum number of guesses allowed.

If all guesses are exhausted, the function should indicate that the player LOST the game and, again, print out their guess and feedback history.

Optional Next Steps?
Try modifying your code to mimic the “hard mode” in Wordle, where information about the letters in the solution and their positions revealed in prior guesses has to be used in subsequent guesses.

Try spicing up the feedback given using colors or alternative formatting. One way to do this would be to use the {huxtable} package, which is a package for creating text tables that can be styled for display in the R console and can also output to HTML, PDF, and a variety of other formats.

Have R keep track of the date and not let you play more than one Wordle game per day.

Have R keep track of your performance across multiple games of Wordle.

Allow R to post your Wordle results to a social media platform of your choosing. For this, check out, e.g., the {Rfacebook} or {rtweet} packages.

Convert your code to an interactive {shiny} app to have it run in a web brower. Later modules will introduce you to programming with {shiny}.

Exercise 03
