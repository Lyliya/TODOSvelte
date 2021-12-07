<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-navigator";
    import { token } from "../store"
    import Navbar from "../Components/Navbar.svelte"

    let user = {};
    let tasks = [];

    function disconnect() {
        localStorage.removeItem("access_token");
        $token = localStorage.getItem("access_token");
        navigate("/login", {replace: true});
    }

    async function getUser() {
        if (!$token) return disconnect();

        const res = await fetch("http://localhost:3000/user", {
            headers: {
                Authorization: `Bearer ${$token}`
            }
        })
        if (res.status != 200) {
            return disconnect();
        }
        user = await res.json();
        console.log(user)
    }

    async function getTasks() {
        if (!$token) return disconnect();

        const res = await fetch("http://localhost:3000/task", {
            headers: {
                Authorization: `Bearer ${$token}`
            }
        })
        if (res.status != 200) {
            return disconnect();
        }
        tasks = await res.json();
        console.log(tasks)
    }

    async function updateTask(taskId) {
        if (!$token) return disconnect();

        const task = tasks.find(e => e.id == taskId);

        console.log("Update for task", task.done)

        setTimeout(() => {
            console.log("done:", task.done)
        }, 100);

        const body = {
            title: task.title,
            done: task.done
        };

        console.log(body)

        const res = await fetch(`http://localhost:3000/task/${taskId}`, {
            method: "PUT",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${$token}`
            },
            body: JSON.stringify(body)
        });

        if (res.status != 200) {
            return disconnect();
        }

        console.log(tasks)

        // Update tasks
        getTasks();
    }

    onMount(async () => {
        console.log("onMount")
        getTasks();
        getUser();
    })
</script>

<main>
    <Navbar name={user.username}/>
        {#each tasks as task (task.id)}
            <div class="task-list">
                <input type="checkbox" bind:checked={task.done} on:change={updateTask(task.id)} />
                <p>{task.title}</p>
            </div>
        {/each}
    <button on:click={disconnect}>
        Disconnect
    </button>
</main>

<style>
    .task-list {
        display: flex;
        align-items: center;
        justify-content: center;
    }
</style>