*******************
***Solution***
*******************

NOTE: You can view the changes required for the solution here: https://github.com/ScriabinOp8No12/hint-feature-11xdev/pull/1/files#diff-06b4443a9c3599744dedf37083f6e6b5346898eab14976e6ccce099ae3783b2c

1. The easiest and quickest way to solve this coding challenge is to realize that the submodule code (online-go.com) has already implemented the hint feature we want to implement.

Let's navigate to the online-go.com website, and look for the hint button

2. Navigate to the following link and click on any of the puzzles. Now find the hint button, right click it, then click inspect.  We will search for this in our code, so we can replicate the logic.

https://online-go.com/puzzle

Or navigate to the following puzzle:
https://online-go.com/puzzle/2625

3. The class won't help us much since it's either empty or "active", but we know that the button contains the text "Hint", let's search for Hint in our codebase.  We notice a file "Puzzle.tsx" located at online-go.com/src/views/Puzzle that looks promising!

4. We see two important functions we want to replicate, removeHints and showHint.  Unfortunately, this code was written before hooks like useState/useRef became available, so we will have to convert these class component patterns to functional component patterns using hooks.  

5. After we convert these, we also need to ensure we have access to the goban from the Goban class

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

6. Let's add the hint button to our JSX so we can test the button!  The best place is in the bottom left of the screen where the audio button is in lesson 1-7:

<div id="Lesson" className="bg-blue">
    <div className="landscape-top-spacer">
        <div className="lesson-title">
            {/* 0 indexed puzzleNumber */}
            {displaySectionName} Problem {puzzleNumber + 1}: Blue to play
        </div>
    </div>
    <div id="Lesson-bottom-container">
        <div id="left-container">
            <div className="explanation-text" onClick={cancel_animation_ref.current}>
                {/* {text} */}
                <div className="puzzle-sections">
                    {/* <h3>Blue to play</h3> */}
                    <h3>Problem Sections</h3>
                    {sectionList}
                </div>
            </div>
            <!-- Add the hint button here!  This will show a hint button in the bottom left corner of the screen -->
            <div className="bottom-graphic">
                <button onClick={showHint}>Hint</button>
            </div>
        </div>

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
        // Add this removeHints method here so we clear the hints when our Goban updates, this ensures the hint marks are gone when we click the Goban (it appears it might work without this line)
        removeHints(); 
    };
```

8. We need to add these removeHints() calls to several other places, like when we click the back or next buttons.  If we don't add these removeHints(), then if we click hint then navigate to a new puzzle, we need to click the hint button twice to actually see the hints.  The reason is the local hint state is still set to true, so clicking the button the first time sets it to false, then we have to click a second time to set it back to true to see the hint marks on the Goban.






