<script>
    export let names;
    let editingUser = null;

    function startEditing(user) {
        editingUser = { ...user };
    }

    function cancelEditing() {
        editingUser = null;
    }
</script>

<div class="mt-10 pt-10 w-full max-w-xl p-12 mx-auto rounded-lg shadow-xl dark:bg-white/10 bg-white/30 ring-1 ring-gray-900/5 backdrop-blur-lg">
    <div class="flex items-center justify-between mb-4">
        <div class="space-y-1">
            <h2 class="text-xl font-semibold">List of Users</h2>
            <p class="text-sm text-gray-500">
                Fetched {names.length} users
            </p>
        </div>
    </div>
    <div class="divide-y divide-gray-900/5">
        {#each names as user (user.id)}
            <div class="flex items-center justify-between py-3">
                {#if editingUser && editingUser.id === user.id}
                    <form method="POST" action="?/update" class="flex items-center space-x-4">
                        <input type="hidden" name="id" value={editingUser.id} />
                        <input type="text" name="name" value={editingUser.name} class="p-1 border rounded" />
                        <input type="email" name="email" value={editingUser.email} class="p-1 border rounded" />
                        <button type="submit" class="bg-green-500 hover:bg-green-700 text-white font-bold py-1 px-2 rounded">Save</button>
                        <button type="button" on:click={cancelEditing} class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-1 px-2 rounded">Cancel</button>
                    </form>
                {:else}
                    <div class="flex items-center space-x-4">
                        <div class="flex">
                            <p class="font-medium pt-1 leading-none">{user.name}</p>
                            <p class="font-medium pl-5 text-gray-500 pt-0">{user.email}</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-2">
                        <button type="button" on:click={() => startEditing(user)}>
                            <img class="w-4" src="./edit-pen.svg" alt="edit" />
                        </button>
                        <form method="POST" action="/profiles?/delete">
                            <input type="hidden" name="id" value={user.id}>
                            <button type="submit">
                                <img class="w-4" src="./trash-can.svg" alt="delete" />
                            </button>
                        </form>
                    </div>
                {/if}
            </div>
        {/each}
    </div>
</div>
