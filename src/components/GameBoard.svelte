<script>
    import { onMount } from "svelte";

    let canvas;
    let ctx;
    let cells = [];
    let canvasWidth = 812;
    let canvasHeight = 812;
    let liveCells = 0;
    let generation = 0;
    let gameWidth = 124;
    let gameHeight = 124;
    let updateTimeout;
    let displayCount;

    const acron = [
        [72, 64],[73,64],[73,62],[75,63],[76,64],[77,64],[78,64]
            ]
    const setUpCanvas = () => {
        liveCells = 0;
        generation = 0;
        for (let i=0; i<gameWidth; i++) {
            cells[i] = [];
            for (let j=0; j<gameHeight; j++) {
                cells[i][j] = 0;
            }
        }
    }

    const fillCanvas = (designArr) => {
        designArr.forEach((point) => {
            cells[[point[0]][point[1]]] = 1;
            liveCells++;
        })
        draw();
    }

    const update = () => { 
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

    const draw = () => {
        liveCells = 0;
        ctx.clearRect(0, 0, canvasWidth, canvasHeight);
        cells.forEach((row, x) => {
            row.forEach((cell, y) => {
                ctx.beginPath();
                ctx.rect(x*7, y*7, 7, 7);
                if (cell) {
                    liveCells++;
                    ctx.fill();
                } else {
                    ctx.stroke();
                }
            });
        });
        displayCount = liveCells;
        updateTimeout = setTimeout(() => {
            update();
        }, 1);
    }
    
    onMount(() => {
        ctx = canvas.getContext('2d');
        ctx.strokeStyle = "#e1e1e1";
        ctx.fillStyle = "cadetblue";
        setUpCanvas();
        fillCanvas(acron);
    })

    
</script>


<style>
    #game-board {
        width: 100%;
        height: 100%;
    }
</style>

<p> test</p>
<canvas bind:this={canvas} id="game-board" width="812" height="812" />