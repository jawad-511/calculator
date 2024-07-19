<script lang="ts">
    import Delete from "$lib/icons/Delete.svelte";
    import Minus from "$lib/icons/Minus.svelte";
    import Division from "$lib/icons/Division.svelte";
    import Multiplication from "$lib/icons/Multiplication.svelte";
    import Addition from "$lib/icons/Addition.svelte";

    let equation: string = "";
    let errorMessage: string = "";

    function addToEquation(val: string) {
        equation += val;
    }

    function backspace() {
        equation = equation.substring(0, equation.length - 1);
        switch (equation.substring(equation.length - 3, equation.length)) {
            case " / ":
            case " * ":
            case " + ":
            case " - ":
                equation = equation.substring(0, equation.length - 3);
                break;

            default:
                equation = equation.substring(0, equation.length - 1);
        }
    }

    function clear() {
        equation = "";
    }

    function solve() {
        try {
           let answer = eval?.(equation);
           if(answer == undefined) throw SyntaxError;
           equation = answer
        } catch (error) {
            errorMessage = "Invalid Equation"; // Set the error message
            let output = document.getElementById("output");
            if (output) {
                output.classList.add("bg-red-500");
                setTimeout(() => {
                    output.classList.remove("bg-red-500");
                    errorMessage = ""; // Clear the error message after 1 second
                    equation = ""; // Clear the equation
                    output.focus(); // Refocus on the output element
                }, 1000);
            }
        }
    }
</script>

<svelte:head>
    <title>Page</title>
</svelte:head>

<div
    class="bg-white min-h-[30rem] w-[20rem] rounded-3xl
    grid grid-cols-4 gap-1 p-6 font-semibold text-3xl shadow-2xl"
>
    <div
        id="output"
        class="
            bg-blue-600
            rounded-xl
            col-span-4
            min-h-12
            flex items-center
            px-4
            mb-2
            text-white
            text-lg
            break-all
            transition-all
            "
    >
        {errorMessage || equation}
    </div>
    <button
        on:click={clear}
        class="bg-slate-950 rounded-full w-14 h-14 text-white">AC</button
    >
    <button
        on:click={() => backspace()}
        class="bg-slate-100 rounded-full w-14 h-14 text-gray-600"
    >
        <Delete />
    </button>
    <button
        on:click={() => addToEquation(" / 100 ")}
        class="bg-slate-100 rounded-full w-14 h-14 text-gray-600">%</button
    >
    <button
        on:click={() => addToEquation(" + ")}
        class="bg-green-500 rounded-full w-14 h-14 text-white"
    >
        <Addition />
    </button>
    <button on:click={() => addToEquation("7")} class="text-gray-600">7</button>
    <button on:click={() => addToEquation("8")} class="text-gray-600">8</button>
    <button on:click={() => addToEquation("9")} class="text-gray-600">9</button>
    <button
        on:click={() => addToEquation(" - ")}
        class="bg-red-500 rounded-full w-14 h-14 text-white"
    >
        <Minus />
    </button>
    <button on:click={() => addToEquation("4")} class="text-gray-600">4</button>
    <button on:click={() => addToEquation("5")} class="text-gray-600">5</button>
    <button on:click={() => addToEquation("6")} class="text-gray-600">6</button>

    <button
        on:click={() => addToEquation(" / ")}
        class="bg-blue-700 rounded-full w-14 h-14 text-white"
    >
        <Division />
    </button>

    <button on:click={() => addToEquation("1")} class="text-gray-600">1</button>
    <button on:click={() => addToEquation("2")} class="text-gray-600">2</button>
    <button on:click={() => addToEquation("3")} class="text-gray-600">3</button>
    <button
        on:click={() => addToEquation(" * ")}
        class="bg-yellow-400 rounded-full w-14 h-14 text-white"
    >
        <Multiplication />
    </button>
    <button on:click={() => addToEquation(" . ")} class="text-gray-600"
        >.</button
    >
    <button on:click={() => addToEquation("0")} class="text-gray-600">0</button>
    <button
        on:click={solve}
        class="bg-slate-100 rounded-full col-span-2 text-gray-600">=</button
    >
</div>
