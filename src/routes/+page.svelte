<script lang="ts">
	let ToDoList: string[] = $state([]);
	let newTodo: string = $state('');
	let editingIndex: number | null = $state(null);

	let showAlert: boolean = $state(false);

	const todoComplete = (index: number) => {
		ToDoList = [...ToDoList.slice(0, index), ...ToDoList.slice(index + 1)];
		showAlert = true;
		setTimeout(() => {
			showAlert = false; // Hide the alert after a duration
		}, 2500); // Adjust the duration as needed
	};

	const addToDo = (): void => {
		if (newTodo === '') {
			return console.log('No message');
		} else {
			ToDoList.push(newTodo);
			// ToDoList = [...ToDoList, newTodo]; reactive also
			newTodo = '';
		}
	};

	const deleteToDo = (index: number): void => {
		ToDoList = [...ToDoList.slice(0, index), ...ToDoList.slice(index + 1)];
	};
</script>

<main class="flex flex-col items-center justify-center h-screen">
	{#if showAlert}
		<div role="alert" class="alert alert-success w-96">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				class="w-6 h-6 stroke-current shrink-0"
				fill="none"
				viewBox="0 0 24 24"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
				/></svg
			>
			<span class="text-xl font-bold">Congrats my silbe kana!</span>
		</div>
	{/if}

	<div class="flex flex-col w-2/5 my-4 overflow-auto border card h-1/2">
		<h1 class="p-3 text-xl font-extrabold text-center">Todo List</h1>
		<!-- <div class="divider"></div> -->

		{#if ToDoList.length === 0}
			<div class="flex items-center justify-center h-full">
				<h1 class="text-4xl font-bold">To Do is Clear :)</h1>
			</div>
		{:else}
			<ol class="list-decimal">
				{#each ToDoList as todo, i}
					<li class="flex items-center justify-between p-4">
						{#if editingIndex === i}
							<input
								type="text"
								class="w-full max-w-xs input input-bordered input-primary"
								bind:value={ToDoList[i]}
							/>
						{:else}
							<p class="text-lg font-semibold">
								<span class="text-md">{i + 1}.</span>

								{todo}
							</p>
						{/if}

						<div class="items-center flex-0">
							{#if editingIndex === i}
								<div class="tooltip tooltip-success" data-tip="Confirm Changes">
									<button
										on:click={() => (editingIndex = null)}
										class="btn-square btn hover:text-green-300"
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="24"
											height="24"
											viewBox="0 0 24 24"
											><path
												fill="currentColor"
												fill-rule="evenodd"
												d="M12 21a9 9 0 1 0 0-18a9 9 0 0 0 0 18m-.232-5.36l5-6l-1.536-1.28l-4.3 5.159l-2.225-2.226l-1.414 1.414l3 3l.774.774z"
												clip-rule="evenodd"
											/></svg
										>
									</button>
								</div>
							{:else}
								<div class="tooltip tooltip-primary" data-tip="Edit Todo">
									<button
										on:click={() => (editingIndex = i)}
										class="btn-square btn hover:text-blue-300"
									>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											width="24"
											height="24"
											viewBox="0 0 24 24"
											><path
												fill="currentColor"
												d="m18.988 2.012l3 3L19.701 7.3l-3-3zM8 16h3l7.287-7.287l-3-3L8 13z"
											/><path
												fill="currentColor"
												d="M19 19H8.158c-.026 0-.053.01-.079.01c-.033 0-.066-.009-.1-.01H5V5h6.847l2-2H5c-1.103 0-2 .896-2 2v14c0 1.104.897 2 2 2h14a2 2 0 0 0 2-2v-8.668l-2 2z"
											/></svg
										>
									</button>
								</div>
							{/if}

							<div class="tooltip tooltip-success" data-tip="Todo Done">
								<button on:click={() => todoComplete(i)} class="btn btn-square hover:text-green-300"
									><svg
										xmlns="http://www.w3.org/2000/svg"
										width="24"
										height="24"
										viewBox="0 0 1024 1024"
										><path
											fill="currentColor"
											d="M704 192h160v736H160V192h160.064v64H704zM311.616 537.28l-45.312 45.248L447.36 763.52l316.8-316.8l-45.312-45.184L447.36 673.024zM384 192V96h256v96z"
										/></svg
									></button
								>
							</div>

							<div class="tooltip tooltip-error" data-tip="Delete Todo">
								<button on:click={() => deleteToDo(i)} class="btn btn-square hover:text-red-300">
									<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 48 48"
										><path
											fill="currentColor"
											d="M20 10.5v.5h8v-.5a4 4 0 0 0-8 0m-2.5.5v-.5a6.5 6.5 0 1 1 13 0v.5h11.25a1.25 1.25 0 1 1 0 2.5h-2.917l-2 23.856A7.25 7.25 0 0 1 29.608 44H18.392a7.25 7.25 0 0 1-7.224-6.644l-2-23.856H6.25a1.25 1.25 0 1 1 0-2.5zm4 9.25a1.25 1.25 0 1 0-2.5 0v14.5a1.25 1.25 0 1 0 2.5 0zM27.75 19c-.69 0-1.25.56-1.25 1.25v14.5a1.25 1.25 0 1 0 2.5 0v-14.5c0-.69-.56-1.25-1.25-1.25"
										/></svg
									>
								</button>
							</div>
						</div>
					</li>
				{/each}
			</ol>
		{/if}
	</div>

	<div class="flex items-center justify-center gap-2">
		<input
			type="text"
			placeholder="Type here"
			class="w-full max-w-xs input input-bordered input-primary"
			bind:value={newTodo}
		/>
		<button class="btn btn-success" on:click={addToDo}>Add Todo </button>
	</div>
</main>
