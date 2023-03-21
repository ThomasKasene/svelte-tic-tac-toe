<script>
    import { createEventDispatcher } from 'svelte';
    import Square from './Square.svelte';

    const dispatch = createEventDispatcher();

    export let height = 3;
    export let width = 3;
    export let currentPlayer = null;

    let board;
    let totalSquareCount;
    let unmarkedSquaresCount;

    $: resize(height, width);

    function resize(newHeight, newWidth) {
        board = [... new Array(newHeight)].map(row => new Array(newWidth).fill(null));
        totalSquareCount = newHeight * newWidth;
        unmarkedSquaresCount = totalSquareCount;
    }

    export function getUnmarkedSquareCount() {
        return unmarkedSquaresCount;
    }

    export function reset() {
        for (let i = 0; i < board.length; i++) {
            for (let j = 0; j < board[i].length; j++) {
                board[i][j].reset();
            }
        }
        unmarkedSquaresCount = height * width;
    }

    function markSquare(rowIndex, cellIndex) {
        if (!board[rowIndex][cellIndex].isClaimed()) {
            unmarkedSquaresCount--;
            dispatch('squareclicked', createSquareClickedDetails(rowIndex, cellIndex));
        }
    }

    function createSquareClickedDetails(rowIndex, cellIndex) {
        return {
            markedSquare: board[rowIndex][cellIndex],
            verticalStreak: resolveVerticalStreak(rowIndex, cellIndex),
            horizontalStreak: resolveHorizontalStreak(rowIndex, cellIndex),
            diagonalStreak: resolveDiagonalStreak(rowIndex, cellIndex)
        };
    }

    function resolveVerticalStreak(rowIndex, cellIndex) {
        let count = 1;
        for (let i = rowIndex - 1; i >= 0; i--) {
            if (board[i][cellIndex].isOwnedBy(currentPlayer)) {
                count++;
            } else {
                break;
            }
        }
        for (let i = rowIndex + 1; i < height; i++) {
            if (board[i][cellIndex].isOwnedBy(currentPlayer)) {
                count++;
            } else {
                break;
            }
        }
        return count;
    }

    function resolveHorizontalStreak(rowIndex, cellIndex) {
        let count = 1;
        for (let i = cellIndex - 1; i >= 0; i--) {
            if (board[rowIndex][i].isOwnedBy(currentPlayer)) {
                count++;
            } else {
                break;
            }
        }
        for (let i = cellIndex + 1; i < width; i++) {
            if (board[rowIndex][i].isOwnedBy(currentPlayer)) {
                count++;
            } else {
                break;
            }
        }
        return count;
    }

    function resolveDiagonalStreak(rowIndex, cellIndex) {
        let topLeftToBottomRightCount = 1;
        for (let i = rowIndex - 1, j = cellIndex - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j].isOwnedBy(currentPlayer)) {
                topLeftToBottomRightCount++;
            } else {
                break;
            }
        }
        for (let i = rowIndex + 1, j = cellIndex + 1; i < height && j < width; i++, j++) {
            if (board[i][j].isOwnedBy(currentPlayer)) {
                topLeftToBottomRightCount++;
            } else {
                break;
            }
        }

        let bottomLeftToTopRightCount = 1;
        for (let i = rowIndex + 1, j = cellIndex - 1; i < height && j >= 0; i++, j--) {
            if (board[i][j].isOwnedBy(currentPlayer)) {
                bottomLeftToTopRightCount++;
            } else {
                break;
            }
        }
        for (let i = rowIndex - 1, j = cellIndex + 1; i >= 0 && j < width; i--, j++) {
            if (board[i][j].isOwnedBy(currentPlayer)) {
                bottomLeftToTopRightCount++;
            } else {
                break;
            }
        }
        return Math.max(topLeftToBottomRightCount, bottomLeftToTopRightCount);
    }
</script>

{#key totalSquareCount}
<div class="boardWrapper">
    {#each board as row, rowIndex}
        {#each row as cell, cellIndex}
            <div style:grid-row-start={rowIndex + 1} style:grid-column-start={cellIndex + 1}>
                <Square
                        bind:this={board[rowIndex][cellIndex]}
                        on:click={() => markSquare(rowIndex, cellIndex)}
                        --bg-color-hover={currentPlayer != null ? currentPlayer.backgroundColorHover : ''}/>
            </div>
        {/each}
    {/each}
</div>
{/key}

<style>
    .boardWrapper {
        display: grid;
        grid-auto-columns: 100px;
        grid-auto-rows: 100px;
    }

    .boardWrapper div {
        padding: 4px;
    }
</style>