    <script>
        import { onMount } from "svelte";

        let canvas;
        let ctx;
        let cells = [];
        let liveCells = 0;
        let displayCount;
        let generation = 0;

        export let design;
        export let toggle;
        export let canvasWidth;
        export let canvasHeight;

        const setUpCanvas = () => {
            liveCells = 0;
            generation = 0;
            for (let i=0; i<canvasWidth; i++) {
                cells[i] = [];
                for (let j=0; j<canvasHeight; j++) {
                    cells[i][j] = 0;
                }
            }
        }

        const fillCanvas = () => {
            design.forEach((point) => {
                cells[point[0]][point[1]] = 1;
                liveCells++;
            })
            console.log(cells)
        }

        const incrementAndFill = (ctx) => {
            liveCells++;
            ctx.fill();
        }   

        const draw = () => {
            liveCells = 0;
            ctx.clearRect(0, 0, canvasWidth*7, canvasHeight*7);
            cells.forEach((row, x) => {
                row.forEach((cell, y) => {
                    ctx.beginPath();
                    ctx.rect(x*7, y*7, 7, 7);
                    cell ? incrementAndFill(ctx) : ctx.stroke();
                });
            });
            displayCount = liveCells;
        }   

        const calcNext = () => { 
                let result = [];
                generation++;
                console.log(generation);
                //Return amount of alive neighbours for a cell
                let _countNeighbours = (x, y) => {
                    let amountAlive = 0;
                    let _isFilled = (x, y) => {
                        return cells[x] && cells[x][y];
                    }
                    
                    if (_isFilled(x-1, y-1)) amountAlive++;
                    if (_isFilled(x,   y-1)) amountAlive++;
                    if (_isFilled(x+1, y-1)) amountAlive++;
                    if (_isFilled(x-1, y  )) amountAlive++;
                    if (_isFilled(x+1, y  )) amountAlive++;
                    if (_isFilled(x-1, y+1)) amountAlive++;
                    if (_isFilled(x,   y+1)) amountAlive++;
                    if (_isFilled(x+1, y+1)) amountAlive++;
                    
                    return amountAlive;
                }
                
                cells.forEach((row, x) => {
                    result[x] = [];
                    row.forEach((cell, y) => {
                        let alive = 0,
                            count = _countNeighbours(x, y);
                        if (cell > 0) {
                            alive = count === 2 || count === 3 ? 1 : 0;
                        } else {
                            alive = count === 3 ? 1 : 0;
                        }

                        result[x][y] = alive;
                    });
                });
                
                cells = result;

                draw();
            };

        const handleToggle = () => {
            toggle(calcNext)
        }
        
        onMount(() => {
            ctx = canvas.getContext('2d');
            ctx.strokeStyle = "#e1e1e1";
            ctx.fillStyle = "cadetblue";
            setUpCanvas();
            fillCanvas();
            draw();
        })
    </script>


    <style>
        #game-board {
            width: 100%;
            height: 100%;
        }
    </style>

    <p>Living Cells: {displayCount}</p>
    <p>Generation: {generation}</p>
    <button type="checkbox" on:click={handleToggle}></button>
    <canvas bind:this={canvas} id="game-board" width="812" height="812" />