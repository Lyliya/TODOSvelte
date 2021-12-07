<script>
    import { token } from "../store"
    import { useNavigate } from "svelte-navigator";

    const navigate = useNavigate();

    let username;
    let password;

    async function login() {
        const body = {
            username,
            password
        }

        const res = await fetch("http://localhost:3000/user/login", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(body)
        })
        const json = await res.json();
        console.log(json)
        if (json.error) {
            
        } else {
            localStorage.setItem("access_token", json.access_token)
            $token = json.access_token
            navigate("/", { replace: true })
        }
    }
</script>

<main>
    <div class="container">
        <h1>Register</h1>
        <form>
            <label for="username">Username</label>
            <input type="text" bind:value={username} id="username" />
            <label for="password">Password</label>
            <input type="password" bind:value={password} id="password" />
        </form>
        <button on:click={login}>Login</button>
    </div>
</main>

<style>

    main {
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .container {
        color: white;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        height: 30%;
        width: 30%;
        background-color: #1c1c1c;
        border-radius: 10px;
    }

    form > input {
        margin-bottom: 20px;
    }

    button {
        border: none;
        border-radius: 10px;
        background-color: #292929;
        width: 30%;
        padding: 10px;
        color: white;
        font: inherit;
        cursor: pointer;
        outline: inherit;
        margin-right: 10px;
    }

</style>