<script>
  import { useNavigate } from "svelte-navigator";
  import { token } from "../store.js";

  $: isAuthenticated = $token

  const navigate = useNavigate();

  console.log($token)

  $: if (!isAuthenticated) {
    navigate("/login", {
      replace: true,
    });
  }
</script>

{#if $token}
  <slot />
{/if}
