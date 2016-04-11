---
layout: post
title: Multi-State Array Combinations in C# 
---

So, what do I mean exactly, by multi-state array combinations? Part of the work I am currently doing required me to calculate all possible combinations of a set of values of a fixed size, each value having three states. Each combination is effectively a fixed length array of tri-state values. In my case, the three states are 'Agree', 'Disagree' and 'Missing', with each position in the array representing a field within a dataset.

For example, if there were three fields (name, age, gender) you would have combinations like:

- agree, agree, agree
- disagree, agree, agree
- missing, agree, agree
- agree, disagree, agree,
- agree, missing, agree
- and so on...

Originally I had only two states, so I could get away with using a `BitArray` and `BitConverter.GetBytes()`. When the third state was required, I pulled my hair out for a bit (well, a lot actually, probably too long) before coming to a relatively straightforward solution that will work with any number of states.

This solution uses the [remainder theorem converting a number to a different base](https://en.wikipedia.org/wiki/Positional_notation#Base_conversion). If we have *n* elements and *s* states, we know the total number of combinations is *s<sup>n</sup>*. The algorithm loops from to 0 to the total number of combinations minus 1 and performs a base-conversion of this number from base-10 to base-s. Each digit of the base-*s* value is set into the resultant array that represents the combination.

OK, lets see some code. The function below does all the work and takes our two parameters, *s* and *n*. It returns a list of `int[]`, each representing a unique combination.    

    private static IEnumerable<int[]> CalculateAllPossibleCombos(int numberOfStates, int numberOfElements)
    {
      var numberOfCombinations = Math.Pow(numberOfStates, numberOfElements);
    
      for (var count = 0; count < numberOfCombinations; count++)
      {
        var combo = new int[numberOfElements];
        int input = count;
        int index = 0;
    
        while (input != 0)
        {
          combo[index] = input % numberOfStates;
          input /= numberOfStates;
          index++;
        }
    
        yield return combo;
      }
    }

If we call this with *s* = 2 and *n* = 4:

    Console.WriteLine("Binary combos");
    var binaryCombos2 = CalculateAllPossibleCombos(2, 4);
    foreach (var combo in binaryCombos2)
    {
      Console.WriteLine(string.Join(" ", combo));
    }

Results are:

    Binary combos
    0 0 0 0
    1 0 0 0
    0 1 0 0
    1 1 0 0
    0 0 1 0
    1 0 1 0
    0 1 1 0
    1 1 1 0
    0 0 0 1
    1 0 0 1
    0 1 0 1
    1 1 0 1
    0 0 1 1
    1 0 1 1
    0 1 1 1
    1 1 1 1

If we call this with *s* = 3 and *n* = 4:

	Console.WriteLine("Trinary combos");
	var trinaryCombos = CalculateAllPossibleCombos(3, 4);
	foreach (var combo in trinaryCombos)
	{
	    Console.WriteLine(string.Join(" ", combo));
	}

Results are:

    Trinary combos
    0 0 0 0
    1 0 0 0
    2 0 0 0
    0 1 0 0
    1 1 0 0
    2 1 0 0
    0 2 0 0
    1 2 0 0
    2 2 0 0
    0 0 1 0
    1 0 1 0
    2 0 1 0
    0 1 1 0
    1 1 1 0
    2 1 1 0
    0 2 1 0
    1 2 1 0
    2 2 1 0
    0 0 2 0
    1 0 2 0
    2 0 2 0
    0 1 2 0
    1 1 2 0
	...
	<results truncated>
 
It may not be the fastest solution, but I think this works pretty well and is easy to understand. 