<script>
    import { onMount } from "svelte";
    import { navigate } from "svelte-navigator";
    import { token } from "../store"
    import Navbar from "../Components/Navbar.svelte"

    let user = {};
    let tasks = [];
    let newtask = "";

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
        console.log("Update Task")
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

    async function deleteTask(taskId) {
        console.log("Delete task", taskId)
        if (!$token) return disconnect();

        const res = await fetch(`http://localhost:3000/task/${taskId}`, {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${$token}`
            }
        });

        if (res.status != 200) {
            return disconnect();
        }

        // Update tasks
        getTasks();
    }

    async function createTask() {
        if (!$token) return disconnect();

        const body = {
            title: newtask
        }

        const res = await fetch(`http://localhost:3000/task`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${$token}`
            },
            body: JSON.stringify(body)
        });

        if (res.status != 200) {
            return disconnect();
        }
        newtask = "";

        // Update tasks
        getTasks();
    }

    async function handleKeyUp(event, taskId) {
        if (event.keyCode == 13) {
            // Enter key
            console.log("Create or update task", taskId);
            if (taskId) await updateTask(taskId);
            else await createTask();
        }
    }

    onMount(async () => {
        getTasks();
        getUser();
    })
</script>

<main>
    <Navbar name={user.username}/>
        <div class="task-list">
            {#each tasks as task (task.id)}
                <div class="task">
                    <input type="checkbox" bind:checked={task.done} on:change={() => updateTask(task.id)} />
                    <input bind:value={task.title} on:keyup={() => handleKeyUp(event, task.id)} />
                    <button on:click={deleteTask(task.id)}>X</button>
                </div>
            {/each}
            <input placeholder="Add a task" bind:value={newtask} on:keyup={() => handleKeyUp(event, null)} />
        </div>
    <button on:click={disconnect}>
        Disconnect
    </button>
</main>

<style>
    .task {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    input {
        border: none;
        color: black;
        background-color: var(--body-color);
        /* width: 80%; */
    }
    input:focus {
        outline: none;
        border-bottom: 2px solid grey;
    }
</style>