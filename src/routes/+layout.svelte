<script>
    import todolistIcon from '$lib/assets/checklist.png';
    import './layout.css'
</script>

<svelte:head>
    <link rel="icon" href={todolistIcon} />
    <title>To Do WebApp</title>
</svelte:head>

<slot />
