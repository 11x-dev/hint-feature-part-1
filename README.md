***Coding Challenge***

You've been requested to make a hint feature for the kidsgoserver.com.
For simplicity, the first version of the hint feature can just display the next correct move(s) to the user.
You do NOT have to worry about displaying the next wrong move(s) to the user.  

You may find exploring the website online-go.com, and the submodule online-go.com useful.

Requirements:

1. Hint button in the bottom left of the screen on desktop
2. Clicking the hint button shows a green square mark (or another way to indicate a correct move) on the Go board (Goban) where the correct next move(s) should be
3. Clicking on the Goban when a hint is showing will automatically clear the hint mark(s)
4. Clicking the hint button when hints are showing clears the hint mark(s)

Relevant Files/Code:

The file you'll want to add the hint button to is located at: src/views/Lessons/Puzzles.tsx

The file that contains the correct move tree(s) are located at: src/views/Lessons/Lesson8Puzzles/Capturing.tsx

In the move_tree lines, the correct move branch is in the first array, you'll need to display that coordinate on the Goban when the hint button is clicked.  

Example 1: Inside the Capturing.tsx file, you'll see this move_tree within the config portion.  You'll need to highlight the g6 coordinate/intersection on the Goban when the hint button is clicked. Remember the first array is the correct move branch.

```
move_tree: this.makePuzzleMoveTree(["g6"], ["f5g6"], 9, 9),
```

Example 2: In the following example, we have the correct move tree: e5f5f4. That means if the hint button is clicked, we want to show a mark on e5, the computer will then automatically play the next move in the move tree, which is f5, then if we click the hint button again, it will need to show f4 as the next hint

```
move_tree: this.makePuzzleMoveTree(["e5f5f4"], ["f5e5"], 9, 9),
```

A properly implemented hint feature will work like the hint feature does on [kidsgoserver.com/](https://kidsgoserver.com/learn-to-play/8/problems/capturing/1)

Remember that you do not need to include the red marks for incorrect next move(s), and are only responsible for including the correct next move(s).  