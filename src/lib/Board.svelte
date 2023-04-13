<script lang="ts">
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    function onCellClicked(index: number) {
        dispatch("onCellClicked", index);
    }

    function getStyleClass(index) {
        var classTypes = "";
        if (index % dimension == 0) {
            classTypes = classTypes.concat("left ");
        }
        if (index < dimension) {
            classTypes = classTypes.concat("top ");
        }
        if (index % dimension == dimension - 1) {
            classTypes = classTypes.concat("right ");
        }
        if (index > state.length - dimension - 1) {
            classTypes = classTypes.concat("bottom ");
        }
        console.log(classTypes);
        return classTypes;
    }

    export let state: string[];
    const dimension = Math.sqrt(state.length);
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<table>
    {#each state as item, i}
        {#if i % dimension == 0}
            <tr> </tr>{/if}

        <td class="{getStyleClass(i)}" on:click="{() => onCellClicked(i)}">
            {item}
        </td>
    {/each}
</table>

<style>
    td {
        width: 150px;
        height: 150px;
        border: 1px solid #000000;
        vertical-align: middle;
        font-size: 5em;
    }
    .top {
        border-top: none;
    }
    .left {
        border-left: none;
    }
    .right {
        border-right: none;
    }
    .bottom {
        border-bottom: none;
    }
</style>
