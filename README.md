*******************
***Solution***
*******************

You can view the exact code changes needed to solve this coding challenge here: https://github.com/ScriabinOp8No12/hint-feature-11xdev/pull/1/files#diff-06b4443a9c3599744dedf37083f6e6b5346898eab14976e6ccce099ae3783b2c

This branch contains the hint feature code - you can test that the functionality works as intended in your browser.   

1. The easiest and quickest way to solve this coding challenge is to realize that the submodule code (online-go.com) has already implemented the hint feature we want to implement!  Otherwise you'll have to spend many hours looking through and testing the available methods on the Goban class.  

Let's navigate to the online-go.com website, and look for the hint button.

2. Navigate to the following link and click on any of the puzzles. Now find the hint button, right click it, then click inspect.  We will search for this in our code, so we can replicate the logic.

https://online-go.com/puzzle

Or navigate to the following puzzle:
https://online-go.com/puzzle/2625

3. The class won't help us much since it's either empty or "active", but we know that the button contains the text "Hint", let's search for Hint in our codebase.  We notice a file "Puzzle.tsx" located at online-go.com/src/views/Puzzle that looks promising!

4. We see two important functions we want to replicate, removeHints and showHint.  Unfortunately, this code was written before hooks like useState/useRef became available in React, so we will have to convert these class component patterns to functional component patterns using hooks.  

5. After we convert these, we also need to ensure we have access to the goban from the Goban class.

The states and functions should look something like this after you convert them:

```
    // near the top of the component with the other states, set to false initially so we don't see any hints by default
    const [hintsOn, setHintsOn] = useState(false); 

    const removeHints = () => {
        const goban: Goban = goban_ref.current;
        const move = goban.engine.cur_move;
        move.branches.forEach((item) => goban.deleteCustomMark(item.x, item.y, "hint", true));
        setHintsOn(false);
    };

    const showHint = () => {
        const goban: Goban = goban_ref.current;
        if (hintsOn) {
            removeHints();
        } else if (!goban.engine.cur_move.correct_answer) {
            const branches = goban.engine.cur_move.findBranchesWithCorrectAnswer();
            branches.forEach((branch) => {
                goban.setCustomMark(branch.x, branch.y, "hint", true);
            });
            setHintsOn(true);
        }
    };

```

6. Let's add the hint button, where it executes our showHint function when clicked so we can test the functionality!

According to the requirements on the main branch, we need to add it to the bottom left of the screen, in the bottom-graphic className portion:

```
<div className="bottom-graphic">
    <button onClick={showHint}>Hint</button>
</div>
``` 

7. Now we need to ensure the hint states are cleaned up properly like how they are implemented in the online-go.com code. Notice how the onUpdate() includes a removeHints() call:

```
    onUpdate() {
        this.removeHints();
        this.sync_state();
        this.forceUpdate();
    }
```

Let's replicate this in our code!  Add removeHints() to the end of the onUpdate function, like this:

```
    const onUpdate = () => {
        const mvs = decodeMoves(
            goban.engine.cur_move.getMoveStringToThisPoint(),
            goban.width,
            goban.height,
        );
        const move_string = mvs.map((p) => prettyCoordinates(p.x, p.y, goban.height)).join(",");
        console.log("Move string: ", move_string);
        // Add this removeHints method here so we clear the hints when our Goban updates, 
        // this ensures the hint marks are gone when we click on the Goban, which updates the Goban (it appears it works without this line)
        removeHints(); 
    };
```

8. We need to add these removeHints() calls to several other places, like when we click the back or next buttons.  If we don't add these removeHints(), then if we click hint then navigate to a new puzzle, we need to click the hint button twice to actually see the hints.  The reason is the local hint state is still set to true, so clicking the button the first time sets it to false, then we have to click a second time to set it back to true to see the hint marks on the Goban.

*******************
***Conclusion***
*******************

After getting assigned this feature to work on, I struggled initially to figure out how to use the existing methods on the Goban class to make the hints show up. I then realized that perhaps the online-go.com website had a similar hint feature already implemented, and I was pleased to find that there was indeed a hint feature already implemented.  I then found that code in the submodule, and mimicked the key functionality for the feature.  It still took a little while because I had to convert the class component structure to use the functional component structure with hooks.  

Remember that sometimes similar code has already been implemented somewhere else.  It's important to think about these potential time savers and shortcuts before you dive into building the feature.  

Nice job, this was a very tricky and time consuming feature to implement, see you on the next one! 

