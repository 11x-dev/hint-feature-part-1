*******************
***Initial Setup***
*******************

Getting setup locally only takes a few minutes!

1. Clone the repo

```
git clone https://github.com/ScriabinOp8No12/hint-feature-11xdev.git
```

2. Navigate to the root of the project:

```
cd hint-feature-11xdev
```

3. Install packages and start the frontend server:

```
yarn install && npm run dev
```

4. View the website in your browser

```
http://localhost:18888/
```

************************
***Estimated Time***
************************

Estimated time for this feature is 2 - 8 hours

************************
***Hints and Solution***
************************

If you need a hint or want to see a possible solution, navigate to this document [here](/Hints-And-Solution.md)

************************
***Coding Challenge***
************************

The real codebase uses a submodule that is located at online-go.com, which includes another submodule at online-go.com/submodules/goban. To ensure nothing breaks when those submodules are updated, the code has been manually added. The main online-go.com submodule can be found at https://github.com/online-go/online-go.com

Your task is to make a hint feature for the kidsgoserver.com

For simplicity, the first version of the hint feature can just display the next correct move(s) to the user. 

You do NOT have to worry about displaying the next wrong move(s) to the user.  

You may find exploring the website online-go.com, and the submodule code located within online-go.com useful.

************************
**Requirements**
************************

1. Create a hint button in the bottom left of the screen on desktop
2. Clicking the hint button shows a green square mark on the Go board (Goban) where the correct next move(s) should be
3. Clicking on the Goban when a hint is showing will automatically clear the hint mark(s)
4. Clicking the hint button when hints are showing clears the hint mark(s)
5. Navigating to a different problem/puzzle with the back/next buttons removes the hint marks

A properly implemented hint feature will work like the hint feature does on [kidsgoserver.com](https://kidsgoserver.com/learn-to-play/8/problems/capturing/1)

Remember that you do not need to include the red marks for the incorrect next moves. You are only responsible for including the green marks for the correct next move(s).  

This was a real bug from a real production codebase!  Feel free to use any resources you want on this coding challenge, have fun!  

************************
**Relevant Files/Code**
************************

The file you'll want to add code to is located at: src/views/Lessons/Puzzles.tsx

The file that contains the correct move tree is located at: src/views/Lessons/Lesson8Puzzles/Capturing.tsx

In the move_tree lines, the correct move branch is in the first array, you'll need to display that coordinate on the Goban when the hint button is clicked.  

Example 1: Inside the Capturing.tsx file, you'll see "move_tree" within the config portion.  You'll need to highlight the g6 coordinate/intersection on the Goban when the hint button is clicked. Remember the first array is the correct move branch.

```
move_tree: this.makePuzzleMoveTree(["g6"], ["f5g6"], 9, 9),
```

Example 2: In the following example, we have the correct move tree: e5f5f4. That means if the hint button is clicked, we want to show a mark on e5, the computer will then automatically play the next move in the move tree, which is f5. All marks should now be cleared from the Goban. Then if we click the hint button again, it will need to show f4 as the next hint.  

```
move_tree: this.makePuzzleMoveTree(["e5f5f4"], ["f5e5"], 9, 9),
```

Again, please try the hint feature on the website if you are confused on what you are supposed to do: [kidsgoserver.com](https://kidsgoserver.com/learn-to-play/8/problems/capturing/1)
