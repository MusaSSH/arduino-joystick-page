<script>
    import { onMount } from "svelte";
    import Direction from "../components/Direction.svelte";

    let ws;

    let up = false;
    let down = false;
    let right = false;
    let left = false;
    let button = false;

    onMount(() => {
        ws = new WebSocket("ws://localhost:1234")

        ws.addEventListener("message", (event) => {
            const args = event.data.split(/ +/)
            const command = args.shift()

            if(command === "joy") {
                const x = parseInt(args[0])
                const y = parseInt(args[1])
                const b = parseInt(args[2])

                up = x == 0
                down = x == 2
                right = y == 0
                left = y == 2
                button = b == 1
            }
        })

        ws.addEventListener("error", (event) => {
            console.log("error", event)
        })
    })
</script>
<style>
    .root {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
        width: 100vw;
        background-color: black;
    }
    .directions {
        display: grid;
        grid-template-columns: repeat(3, 100px);
        grid-template-rows: repeat(3, 100px);
        grid-gap: 10px;
    }
</style>

<div class="root">
    <div class="directions">
        <Direction state={(up && !down && left && !right)} />
        <Direction state={(up && !down && !left && !right)} />
        <Direction state={(up && !down && !left && right)} />
        <Direction state={(!up && !down && left && !right)} />
        <Direction state={button} />
        <Direction state={(!up && !down && !left && right)} />
        <Direction state={(!up && down && left && !right)} />
        <Direction state={(!up && down && !left && !right)} />
        <Direction state={(!up && down && !left && right)} />
    </div>
</div>