<script>
    export let name;

    import { navigate } from "svelte-navigator";

    function disconnect() {
        localStorage.removeItem("access_token");
        navigate("/login", {replace: true});
    }
</script>

<main>
    <header>
        <p>{name}</p>
        <button id="disconnect" on:click={disconnect}>
            Disconnect
        </button>
    </header>
</main>

<style>
    p {
        margin: 0;
        padding: 0;
        font-size: 28px;
        font-weight: bolder;
        margin-right: 10px;
    }

    header {
        background-color: #121212;
        color: white;
        height: 60px;
        display: flex;
        align-items: center;
        justify-content: flex-end;
    }

    #disconnect {
        margin: 0;
        border: none;
        border-radius: 10px;
        background-color: #1c1c1c;
        height: 80%;
        padding: 10px;
        color: white;
        font: inherit;
        cursor: pointer;
        outline: inherit;
        margin-right: 10px;
    }
</style>