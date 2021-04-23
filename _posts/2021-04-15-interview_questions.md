---
layout: post
title: Interview Questions
author: "Jordan Calcutt"
categories: journal
tags: [python, blog]
image: numerals.png
---
<b>
Coding Interview Questions. Time-pressured, no-googling tasks that you are not likely to encounter in your usual day-to-day - but are used to help determine if you’re suitable for the job. I’ve encountered a few of late(!), so I’ve picked a selection of my ‘favourites’ to walk through.
</b>

### Roman Numerals

Given a year, convert it to its Roman Numeral representation. I won’t go into too much depth here about the underlying logic of how numbers are converted to Roman Numerals (you can look that up [here](https://en.wikipedia.org/wiki/Roman_numerals)). However, to keep it simple, we have to bear in mind that certain numerals are concatenated together to create larger numbers (i.e. 3 is III), or other numerals placed to the left or right of larger numerals to add or subtract from that number (i.e. 4 is IV, 40 is XL, 45 is XLV).

_Here’s my solution:_

<script src="https://gist.github.com/jcalcutt/1c147f818f7118d3d4f1d2666f517058.js"></script>

We iterate over each value in the integer year we’re passed (by converting to a string), apply the logic to convert that value to a numeral, append to a list, then finally concatenate that into one string and return.

My high-level thought process, was to treat each digit in the ‘overall’ year in isolation. We need to know which value in the year we’re dealing with: the millennium, century, decade or ‘individual’ year. We also need to be flexible, as we could be passed e.g. the year: 25, 225, or even 2025. 

With that in mind, Millenniums are treated in isolation and we just multiply the Roman Number representation of 1000 (“M”) by the millennium number i.e. 3 becomes “MMM”.

Next, and the key step to all of this, was that I realised the logic to covert a century, decade or individual year is exactly the same and can be abstracted to one function. The difference between each one we’re interested in just boils down to the numerals we’re manipulating and can be passed as variables to this helper function. For example, the logic to convert the century 4 or the decade 4 is the same, it just involves different numerals: the century 4 uses “C” and “D” to form “CD” and the decade 4 uses “X” and “L” to form “XL”.

_Congrats if you converted the main header image to 1982…_



### Max Palindrome

Given a string of letters, find the longest palindrome substring that can be created out of them - i.e. a string which is exactly the same backwards as it is ‘forwards’. Preference should be given to alphabetically ‘smaller’ values if multiple solutions exist. For example, a given string of “aaabbb” should return: “ababa”, not “babab”.

_Again, here’s my solution:_

<script src="https://gist.github.com/jcalcutt/5201c695fa0b0707ac885e3e826b9d6f.js"></script>

Now, this isn’t the ‘purest’s’ way of solving this problem. I’ve done some research, and [Manacher’s algorithm](https://en.wikipedia.org/wiki/Longest_palindromic_substring) is considered the best approach here, bringing down the time complexity to an impressive O(n).
Anyway, my first steps here were to create a sorted string of the unique letters we’re passed, and also create a letter count mapping dictionary. 
The logic to build up the palindrome was fairly simple: we loop over each character in the sorted string, if we have 2 or more ‘counts’ of this letter we add the count/2 (rounding down) times of that letter to a list. Next, we then update the word count dictionary, reducing the count number by 2x the number of that letter we’ve added to the list.

Once that’s done, we’re pretty much finished. The penultimate step is to check if we have any ‘extra’ letters we can add to the middle of the palindrome - using the smallest alphabetical letter. Finally, we then concatenate; the list, the smallest alphabetical letter (if we have one), then the list again in reverse, convert it all to a string and return.

### Word Count Board

Given a 2-dimensional ‘board’ of characters, find the number of occurrences of a given string in that board. Matches can be found: left-right horizontally, top-bottom vertically or top left - bottom right diagonally. Matches of the given string value can overlap each other.
So, for example, given the board below and the string “ABC”, there are 3 matches:


![word_count_board]({{ "/assets/word_count_board.png" | absolute_url }})




_Solution:_

<script src="https://gist.github.com/jcalcutt/8e592d071f71eb80ead0ac546342bab3.js"></script>


I split my solution up into 3 distinct ‘sections’ - one to deal with each direction we can match the string. The focus of each ‘section’ is to create and pass all possible alignments of characters (i.e. an array) for that specific direction to a common helper function which counts the number of occurrences the string appears. 

Of course, the horizontal implementation of this was fairly trivial, we have the arrays already created for us, and just need to loop through them. The vertical arrays were slightly more difficult, which involves just looping over the matrix, creating a new vertical array by indexing a particular value for each board loop.

The left-right diagonal creation of these arrays was the most difficult. I’m sure I haven’t implemented the ‘best’ solution here, but I’ll explain my thinking. Left-right diagonals can start anywhere along the top row and anywhere along the left side column, so, first we need to iterate over these matrix cells. 

![word_count_board_diag]({{ "/assets/word_count_diag.png" | absolute_url }})

Once we have some logic to do this, for a given starting point, we can then build up a diagonal array by adding cells which are one row down and one column across from this starting cell (and the then the cell that came before it, etc). To save us some time, once we’ve created a diagonal array, we can check if it’s shorter than the length of the word we’re trying to match - if so, we can discard it, otherwise we can then implement the word count helper function. Finally, we return the running total we’ve been keeping track of.