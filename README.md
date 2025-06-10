*******************
***Solution***
*******************

1. The easiest and quickest way to solve this coding challenge is to realize that the submodule code (online-go.com) has already implemented the hint feature we want to implement.

Let's navigate to the online-go.com website, and look for the hint button

2. Navigate to the following link and click on any of the puzzles. Now find the hint button, right click it, then click inspect.  We will search for this in our code, so we can replicate the logic.

https://online-go.com/puzzle

Or navigate to the following puzzle:
https://online-go.com/puzzle/2625

3. The class won't help us much since it's either empty or "active", but we know that the button contains the text "Hint", let's search for Hint in our codebase.  We notice a file "Puzzle.tsx" located at online-go.com/src/views/Puzzle that looks promising!

4. We see two important functions we want to replicate, removeHints and showHint.  Unfortunately, this code was written before hooks like useState/useRef became available, so we will have to convert these class component patterns to functional component patterns using hooks.  

5. After we convert these, we need to ensure we have access to the goban from the Goban class, so we can use a ref

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

6. Now we need to ensure the states are cleaned up properly like how it's implemented in the online-go.com code.  First, let's add a removeHints(); call to the end of the onUpdate function, like this:

```
    const onUpdate = () => {
        const mvs = decodeMoves(
            goban.engine.cur_move.getMoveStringToThisPoint(),
            goban.width,
            goban.height,
        );
        const move_string = mvs.map((p) => prettyCoordinates(p.x, p.y, goban.height)).join(",");
        console.log("Move string: ", move_string);
        // Add this removeHints method here so we clear the hints when our Goban updates, this ensures the hint marks are gone when we click the Goban (it appears we might not need this, but it's safer to have this logic here)
        removeHints(); 
    };
```

You can view the changes needed for a possible solution here: 