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
    <form>
        <label for="username">Username</label>
        <input type="text" bind:value={username} id="username" />
        <label for="password">Password</label>
        <input type="password" bind:value={password} id="password" />
    </form>
    <button on:click={login}>Login</button>
</main>

<style></style>