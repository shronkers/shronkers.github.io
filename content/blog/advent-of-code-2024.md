+++
date = '2025-01-02T18:46:28+03:00'
title = 'My experience with Advent of Code 2024'
description = 'Advent of Code 2024 solutions and thoughts.'
+++

This year was the first time I solved every puzzle in Advent of Code.

In this post I'll describe my solutions for each day and my share my thoughts about some of the puzzles.

Here's a [link](https://github.com/shroomwastaken/advent-of-code-2024) to the repo with all of my solutions.

<hr>

# Day 1

**Part 1**: Calculate `abs(n1 - n2)` for each `n1` in the sorted first list and `n2` in the sorted second list,
add them all up.

**Part 2**: Calculate `n1 * list2.count(n1)` for each `n1` in the first list, add them all up.

Not much to say about this day.

<hr>

# Day 2

**Part 1**: For each sequence check if its sorted or reverse sorted, then check if
all of the distances between numbers in the sequence are either 1, 2 or 3.

**Part 2**: For each sequence, perform the same check. If a sequence doesn't pass
the check, try checking every possible sequence made by removing one element from
the original sequence.

When I originally solved this day in Rust I overcomplicated this quite a bit.

Here's a one-liner in Python:

```python
print(str((a:=[list(map(int,x.split())) for x in open("i")]))[:0],(l:=len)([x for x in a if (e:=lambda x:(x==(s:=sorted)(x) or x==s(x)[::-1]) & all(y in [1,2,3] for y in [abs(x[i]-x[i-1]) for i in range(1,l(x))]) and l(set(x))==l(x))(x)]),'\n',l([x for x in a if any(e(z) for z in [x]+[x[:i]+x[i+1:] for i in range(l(x))])]),sep='')
```

<hr>

# Day 3

**Part 1**: Use this regex `mul\((\d{1,3}),(\d{1,3})\)` to find every mul(),
multiply the numbers and add the results up.

**Part 2**: Find the `mul()`s, `do()`s and `don't()`s with regex.
Construct a list with elements `(range, true/false)` where the range represents indexes
in the input string and the boolean represents if we're allowed to add a `mul()` result
in this range to the result. Iterate over the `mul()`s and check what range they're in.
Add up the results of `mul()`s in good ranges to get the final result.

This day was the first time I've ever used regex properly. Took me a little bit to figure out
the logic to solve this puzzle.

<hr>

# Day 4

**Part 1**: Check every possible vertical, horizontal, diagonal line for instances of `XMAS` or `SAMX`

**Part 2**: Check every possible cross (represented here as 5 symbols (top left, top right, bottom left, bottom right, center))
for instances of `MMSSA`, `MSMSA`, `SSMMA`, `SMSMA`.

Easier than day 3.

<hr>

# Day 5

**Part 1**: Check every page sequence against every rule, add up the middle page numbers of every sequence that fits every rule.

**Part 2**: Performing the same checks, get the incorrectly ordered sequences. For each one of those swap elements
that don't fit the rules while the sequence isn't correctly ordered, then add up the middle page numbers of every acquired sequence.

Quite a simple and elegant puzzle. Not much to say here.

<hr>

# Day 6

**Part 1**: Simulate the guard's movements until he arrives at a border of the map. For each cell visited, add it to a set.
The `len()` of the set after the end of the simulation is the answer.

**Part 2**: Try adding a wall at every possible position, simulate the guard's movements until he arrives at a border (not counted towards the answer)
or until he arrives at a cell that has already been visited while facing the same direction he was facing when visiting that cell before (counted towards the answer).

First puzzle that took me more than 100 lines of Rust to solve.

<hr>

# Day 7

**Part 1**: Consider that all the possible operations can be represented as a binary number where an unset bit is a `+` and a set bit is a `*`.
Execute all the possible sequences of operations until the result matches the needed number. Add up all the matched results.

**Part 2**: Same thing but as a base3 number. The concatenation operator is equivalent to multiplying the first operand by `10 ** operand2digits` and adding `operand2`.
For example: `12345 || 67` is equivalent to `12345 * (10 ** 2) + 67`.

Pro tips: The number of digits a number `x` has is `floor(log10(x))`. The `n`th digit of a base3 number `x` with `y` digits is `(x / 3 ** (y - n - 1)) % 3`.
(I'm actually not too sure if that last one is correct but it worked for me)

<hr>

# Day 8

**Part 1**: Find the positions of all the anntenae. Put them in a hashmap with the frequency as the key and the positions of antennae
with that frequency as the value.

For every frequency consider all possible pairs of different antennae (**order matters here**, so `(a, b)`
is not the same as `(b, a)`).

For each pair calculate the difference in rows and in columns between the first and the second antenna
and add that difference to the second antennas coordinates to get the position of the antinode that that pair generates.

If the position of the antinode that would be placed with these two antennae is within the bounds of the map,
add it to a set (which has coordinates as values) of placed antinodes. The length of that set is the answer.

**Part 2**: Do the exact same thing except instead of getting only one antinode per pair just keep adding the difference between
coordinates until you go outside the bounds of the map.

I might have overexplained this one, it is quite easy.

<hr>

# Day 9

**Part 1**: Construct an array with the initial state of the hard drive. Represent empty space as `0` and the IDs with numbers from `1` onwards
(make sure to remember that you did this later). Use *numbers* as elements of the vector, ***not characters*** (this is important since
ID numbers larger than 10 would take up more than 1 character but we still want to consider them as taking up one cell).

Then for every zero in the hard drive find the last non-empty cell and swap them. Calculate the checksum and you have your answer.

**Part 2**: Get the initial state of the hard drive the same way you did in Part 1. Make a vector with elements `(range, bool, int)`
where the `range` is the range of indexes the file takes up, `bool` is whether the file has been moved yet (this is false at the moment),
`int` is the ID of the file.

Going from the end of this new vector (`i = vec.len() - 1`) if the current range hasn't been moved check every range with indexes (`j`)
from `1` to `i `**`+ 1`**. Check the amount of free space between ranges `j` and `j - 1`. This is the difference between the start of the range
at `j` and the end of the range at `j - 1`.

If we can fit the current range into this free space, remove the range at `i` from the vector
and insert it back at index `j` with the `bool` value set to true.

If after checking all the ranges at `j` we didn't find enough free space
to fit the range at `i`, subtract one from `i` and repeat the process. This works since if we did manage to move the range, the next one to
check would still be at index `i`.

This was the first fairly complicated day for me. I managed to get stuck on an edge case where I wasn't considering a block's ability
to move in between its own start and the end of the block right before it (that's what the `+ 1` is for).

<hr>

# Day 10

**Part 1**: Find the positions of all the trailheads (zeroes). Put them in a vector. While that vector isn't empty iterate over it,
adding new points that need to be checked as a part of some trail to that same vector. If a unique position with a `9` in it is found,
add one to the result.

Here's some *pseudocode* to help illustrate the idea better:

```rust
fn part1(map) {
	let res = 0;
	// let's consider a "position" to be a tuple (row, column)
	let positions = find_zeroes(map);
	for pos in positions {
		let to_be_checked = find_next_points(map, pos);
		let found_nines = vec![];
		while !to_be_checked.is_empty() {
			let new = vec![];
			for p in to_be_checked {
				if map[p.0][p.1] == 9 {
					if !found_nines.contains(p) {
						found_nines.push(p);
					}
					continue;
				}
				new.extend(find_next_points(map, p));
			}
			to_be_checked = new;
		}
		res += found_nines.len();
	}
	return res;
}
```

`find_next_points()` simply checks the four points around the current position to see if any of them can be the next step in the path.

**Part 2**: Instead of checking for *unique* nines, just add any nines you encounter to the result.

Simple day.

<hr>

# Day 11

**Part 1**: Just simulate the stones changing 25 times.

**Part 2**: Create a hashmap with the stones number as the key and the amount of times that stone appears as the value. There's no need to
keep track of every individual stone since their order doesn't matter. Calculate how many stones of which numbers there woud be after
blinking (store this as a new hashmap). Make the old hashmap be the new hashmap. Rinse and repeat for 75 total steps.

Took me a ***while*** to come up with a solution for Part 2. The Part 2 solution would also work with Part 1, and it would be faster too!

<hr>

# Day 12

**Part 1**: Finding the positions of different shapes is simple. It's similar to a flood-fill, except you consider the letter of the shape
you're trying to find as empty space and everything else as walls.

The perimeter of a shape can be found by adding up the *"contributions"* of each position in the shape. A "contribution" is equal to `4`
minus the amount of neighbors that position has in this specific shape. The area of a shape is simply the amount of cells it contains.

**Part 2**: The amount of sides a shape has is equal to the amount of turns taken while traversing the border of this shape.
After that, we need to find the amount of sides of all the shapes enclosed inside of some shape. To do this, map the shape onto an empty
2D array, then, starting from an empty space outside of the shape, flood fill everything, considering the letter of the shape as a wall.
Then, invert the 2D array, and you will now have only the enclosed shapes (to find individual enclosed shapes use same algorithm from Part 1).

Using the same algorithm, find all the shapes, then for each shape calculate its amount of sides. Using the
process described above, recursively calculate the amounts of sides of all enclosed shapes (recursion is needed since a shape can be inside
a shape which is itself inside a shape and so on). The area calculation is the same as Part 1.

First really tough day.

<hr>

# Day 13

**Part 1** and **Part 2**: Consider the following system of equations:

```
Na * Ax + Nb * Bx = Px
Na * Ay + Nb * By = Py
```

where `Na` and `Nb` are the amount of times you need to press buttons A and B respectively,
`Ax` and `Bx` is how much the two buttons add to the X axis, `Ay` and `By` is how much the two buttons add to the Y axis,
`Px` and `Py` is what we need to reach. `Ax`, `Ay`, `Bx`, `By`, `Px` and `Py` are all given by the puzzle input, meaning we simply need to
find the two unknown terms in this system of two equations. This means that there's only one possible solution, so we don't need to worry
about minimizing the cost. Deriving `Na` and `Nb` from this system we get this:

```
Na = (Py * Bx - Px * By) / (Ay * Bx - Ax * By)
Nb = (Px * Ay - Py * Ax) / (Ay * Bx - Ax * By)
```

(notice that the denominator is the same for both of these)

Since we can't exactly press a button 3.5 times, only consider solutions where `Na` and `Nb` are both natural numbers as correct.

This was my first time ever using math to solve a programming puzzle. Also my first time encountering a situation where an `f32` didn't have
enough precision and was giving incorrect answers.

<hr>

# Day 14

**Part 1**: A robots position after 100 steps (in a field of size 101x103) can be calculated like this:

```
Rx = (Rx + Vx * 100) % 101
Ry = (Ry + Vy * 100) % 103
```

The `%` operator here means the remainder left after *euclidian division*. This is what Python's modulo operator does by default but in Rust
we need to do `.rem_euclid()` to achieve the same effect. This is needed since if `Vx` or `Vy` are negative we'd get a negative result
as a position using the normal modulo operator in Rust, which obviously can't be correct.

**Part 2**: Simply try applying the operation described above with a number of steps ranging from 1 to an arbitrarily large number
(10000 worked in my case) and search for any row in the map containing 15 or more robots next to each other. This requires some manual checking.

This was a very unusual Part 2.

<hr>

# Day 15

**Part 1**: Simulate the robots movement. Whenever the robot needs to push a box, get the position of the next empty space after the box(es).
Go back from that empty position to the robot and swap the empty space with each box and the robot.

Here's an illustration:
```
.OOO@
O.OO@
OO.O@
OOO.@
OOO@.
```

**Part 2**: Whenever the robot moves add its position and the position of any boxes it needs to move to a vector of targets.
While we haven't hit a wall or stopped adding new targets see if any of the current targets' movement will cause something else to move,
if so add the newly moved item to the vector. If we didn't encounter any roadblocks then we can move everything in the needed direction.
Clear the current positions of all targets, then put all the targets back in their new positions.

This was the first of two times I resorted to looking up someone else's solution for a Part 2 and rewriting it in Rust.
For a better explanation of Part 2 you can look at the comments I left in my code or watch the [video](https://youtu.be/aCqtVmSSkUs&t=711)
by HyperNeutrino.

<hr>

# Day 16

**Part 1**: Use dijkstras algorithm to find the cost of the shortest path.

**Part 2**: Also dijkstras, but this time we keep track of the cells we've been to and their best cost. Once we reach the end with all
possible shortest paths, backtrack to find all cells we visited. For a better explanation check out my code comments.

This was my first time ever utilizing dijkstras algorithm.

<hr>

# Day 17

**Part 1**: Emulate the instructions.

**Part 2**: Emulation won't work here, the answer is simply too large. Write out your input program instruction by instruction. Notice that
the value of register A always gets right-shifted by some number `x` (in my case `x = 3`). We know that the program runs for as many loops
as there are numbers in the initial program (since it outputs 1 number per iteration and it's supposed to output itself). This means A was
right shifted that many times.

What we can gather from this is that during the last iteration A must have had a value between `0` and `0 + (1 << x)`.
We know that the last iteration gave the last number in the program as its output. Check every A value in the range given above. If we get
the desired output, add that value to a vector of targets along with the number of the iteration this value appeared in (in this case `1`).

Let's imagine we get the number `16` as the only target after the last iteration of the program. Then `16` must have been the result of a
`>> x` operation. Using that logic, we can see that the value of A for the second to last iteration must have been between `16 << x` and
`16 << x + (1 << x)`. Continue using that same logic to get all the possible values that A could've been at the beginning of the program.

I really like this puzzle.

<hr>

# Day 18

**Part 1**: Use dijkstras algorithm.

**Part 2**: For every possible new wall position flood-fill the labyrinth to check if the exit is blocked off.

This puzzle was a nice break from a string of really difficult ones.

<hr>

# Day 19

**Part 1**: Pseudocode would explain this best:
```rust
let res = 0;
for pattern in patterns {
	let possible = vec![];
	for design in designs {
		if pattern.starts_with(design) {
			possible.push(design);
		}
	}
	if possible.is_empty() { continue; }
	while !possible.is_empty() {
		let towel = possible.pop();
		if towel == pattern { res += 1; break; }
		for design in designs {
			if pattern[towel.len()..].starts_with(design) {
				possible.push(pattern + design);
			}
		}
	}
}
return res;
```

**Part 2**: Use a recursive function that takes a towel as a parameter and a cache. The base case for the funciton would be an empty string
as a towel. If the base case is encountered, return `1` from the function.

Iterate over possible lengths (`i`) of next designs to be used (from 1 to `min(max_design_len, towel.len())`). Check if `towel[..i]` is a
possible design, if so then call the function with `towel[i..]` as input and add the result to a variable. Don't forget to save the result
to the cache to avoid redundant calculations.

The sum of results of that recursive function for all patterns is the answer.

<hr>

# Day 20

**Part 1** and **Part 2**: Calculate how long it would take to go through the race without cheats. Save the amount of time it'd take to get
to each cell in a 2D array (save each wall as `-1`). For every non-wall cell check all possible locations where the racer can end up after
cheating. If the racer would save time by taking this cheat (if `time_at_start + 100 + distance_traveled_with_cheat <= normal_time_at_end`)
then add `1` to the result.

Good puzzle, not too difficult but took me some time to figure out.

<hr>

# Day 21

**Part 1**: My solution for this part is vastly inefficient. I precomputed all the possible paths from any key to any other key on
the numerical keypad and the directional keypad, then I traced all the possible paths from numpad -> directional pad 1 -> directional pad 2
-> directional pad 3. From there I just got the minimum length path. My solution takes about 15 seconds to run on my machine using cargos
`release` mode.

**Part 2**: This is the second of two times I resorted to rewriting someone else's solution. Read
[Todd Ginsberg's post](https://todd.ginsberg.com/post/advent-of-code/2024/day21/) about how he solved it.

This is the hardest puzzle of this years AoC, it took me 8 hours to solve Part 1 and I just gave up on Part 2.

<hr>

# Day 22

**Part 1**: Simply apply the operations 2000 times.

**Part 2**: For each buyer calculate the last digits of their first 2000 secret numbers, then calculate the differences between those and
put them in a vector of vectors.

Next, find all unique four-element-long sequences of numbers in those differences and the number they lead
to, only counting the first time a sequence occurs (this is also done for each buyer, resulting in a `Vec<HashMap<[i8; 4], u8>>`).

Finally, find how many bananas each possible sequence would get you with each buyer.

This puzzle was a nice break after the hellish endeavour that was Day 21.

<hr>

# Day 23

Yeah you're just gonna have to read the code for this one. I can't remember how or why what I wrote works.

<hr>

# Day 24

**Part 1**: Simply emulate all the instructions starting with the ones where the both the input registers have known values.

**Part 2**: Here I used Python and [graphviz](https://graphviz.org/) (specifically the `dot` tool) to visualize all the instructions as a
graph. I manually looked through the graph to find any inconsistencies and that worked! I later made a solution in Rust but I can't be sure
that it will work for any input (only tested it on mine and one other persons inputs).

Neat puzzle, I enjoy having to manually dissect my input.

<hr>

# Day 25

**Part 1**: Simply match all keys against all locks to see what fits.

**Part 2**: Nonexistent!

<hr>

# Stats
|     | Part 1   |       | Part 2   |       |
|-----|----------|-------|----------|-------|
| Day | Time     | Rank  | Time     | Rank  |
| 25  | 00:16:47 | 1828  | 00:16:55 | 1496  |
| 24  | 00:36:41 | 2995  | 04:28:14 | 2520  |
| 23  | 01:42:41 | 5421  | 04:59:00 | 7502  |
| 22  | 03:59:18 | 8354  | 04:50:40 | 6579  |
| 21  | 17:50:49 | 10973 | >24h     | 9457  |
| 20  | 10:23:05 | 14813 | 11:08:56 | 10982 |
| 19  | 04:20:39 | 11182 | 06:02:50 | 11142 |
| 18  | 06:44:08 | 14089 | 07:02:41 | 13467 |
| 17  | 08:27:22 | 16085 | 18:09:45 | 13674 |
| 16  | 04:39:30 | 9446  | 13:48:23 | 13099 |
| 15  | 06:41:36 | 15944 | 10:30:30 | 12238 |
| 14  | 05:12:30 | 14862 | 05:35:46 | 11463 |
| 13  | 01:13:01 | 6765  | 03:38:26 | 8603  |
| 12  | 01:08:07 | 6914  | 05:11:14 | 8643  |
| 11  | 04:25:08 | 22711 | 06:18:10 | 17552 |
| 10  | 09:49:31 | 28606 | 09:51:31 | 27414 |
| 9   | 01:38:03 | 9763  | 08:20:35 | 17538 |
| 8   | 05:42:09 | 21393 | 06:06:43 | 20029 |
| 7   | 04:43:27 | 19888 | 06:38:22 | 23240 |
| 6   | 01:06:20 | 10553 | 01:49:38 | 6847  |
| 5   | 02:03:34 | 16290 | 03:42:36 | 18013 |
| 4   | 09:45:16 | 48870 | 09:51:47 | 42026 |
| 3   | 03:04:19 | 30246 | 04:09:07 | 29451 |
| 2   | 03:51:03 | 33377 | 05:13:54 | 28091 |
| 1   | 03:58:53 | 22134 | 04:04:10 | 20651 |

<hr>

# Final thoughts

Advent of Code this year was incredibly entertaining and useful to me as a programmer. Having given up on last years event only 13 days in,
I was determined to finish every single puzzle this year. I learned some algorithms, added a few puzzle solving techniques into my repertoire
but, most importantly, I started feeling that programming was fun again.

I do have a few issues with the puzzles. There were way too many "2D labyrinth puzzles" for my liking; some of the Part 2s did not add
to the puzzle at all, they were just "do the same thing but a million more times" which just felt tedious and uninteresting. Thankfully, the
majority of the puzzles were still great.

Overall, I enjoyed this years Advent of Code.
