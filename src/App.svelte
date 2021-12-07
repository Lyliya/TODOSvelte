<script>
    import { Router, Route, Link } from "svelte-navigator"
    import ProtectedRoute from "./Components/ProtectedRoute.svelte"
    import { token } from "./store"
    import Login from "./Views/Login.svelte"
    import Task from "./Views/Task.svelte"
</script>

<Router primary={false}>
    <main>
        <ProtectedRoute path="/">
            <Task />
        </ProtectedRoute>
        <Route path="login">
            <Login />
        </Route>
    </main>
</Router>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

</style>