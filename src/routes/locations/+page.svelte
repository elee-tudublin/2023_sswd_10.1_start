<script>
	// @ts-nocheck

	export let data;

	let categories = data.categories;
	let locations = data.locations;

	/**
	 * @param id {number}
	 */
	async function filterByCat(id = 0) {
		let endpoint = '/api/locations/';

		if (id != 0) {
			endpoint = `${endpoint}category/${id}`;
		}

		console.log('api endpoint: ', endpoint);

		// Call fetch
		const response = await fetch(endpoint);

		// if resonse code 200 (ok)
		if (response.ok) {
			// get json from resonse
			const json = await response.json();
			locations = json.data;
		}
	}

	async function delete_loc(id = 0) {
		const TO_DO = true;
		if (!TO_DO) {

			if (confirm(`Permanently deleteing product with ID= ${id}\n\nAre you sure?`)) {
				// call the store function if user confirms
				const result = await fetch(`/api/locations/${id}`, {
					method: 'DELETE'
				});

				// show the result
				alert('delete result: ', result);
				console.log('delete: ', result);

				filterByCat();
			}
		} else {

			alert(`delete_loc(${id})`);
		}
	}
</script>

<!-- The HTML content of the page-->

<div class="container">
	<div class="row">
		<!-- Page Header -->
		<h2 class="mt-5">Locations from Supabase</h2>
	</div>
	{#if categories && locations}
		<div class="row">
			<div class="col-sm-2">
				<!-- Page Body Left Column (menu) -->
				<div id="categories" class="list-group">
					<button on:click={() => filterByCat(0)} class="list-group-item list-group-item-action">
						All Locations
					</button>
					{#each categories as cat}
						<button
							on:click={() => filterByCat(Number(cat.id))}
							class="list-group-item list-group-item-action"
						>
							{cat.name}
						</button>
					{/each}
				</div>
			</div>
			<!-- End category col -->

			<div class="col-sm-10">
				<!-- Page Body Right Side (Content goes here) -->
				<div id="locations">
					<table class="table table-striped table-bordered table-hover">
						<thead>
							<tr>
								<th>id</th>
								<th>name</th>
								<th>description</th>
								<th>Location (lat, long)</th>
								<th>Shared</th>
								<th>Favourite</th>
							</tr>
						</thead>
						<tbody>
							{#each locations as location}
								<tr>
									<td>{location.id}</td>
									<td>{location.name}</td>
									<td>{location.description}</td>
									<td
										><a
											href="https://www.openstreetmap.org/search?query={location.latitude}%2C{location.longitude}#map=17/{location.latitude}/{location.longitude}"
											target="_blank">{location.latitude}, {location.longitude}</a
										>
									</td>
									<td>{location.shared}</td>
									<td>{location.favourite}</td>
									<td
										><a class="btn btn-sm btn-outline-primary" href="/locations" role="button">
											<span class="bi bi-pencil-square" />
										</a>
									</td>
									<td>
										<button
											on:click={() => alert('to do: call the delete_loc(id) function')}
											class="btn btn-sm btn-outline-danger"
										>
											<span class="bi bi-trash" />
										</button>
									</td>
								</tr>
							{/each}
						</tbody>
					</table>
				</div>
			</div>
			<!-- End locations col -->
		</div>
		<!-- End Row -->
	{:else}
		<p>No data to display</p>
	{/if}
	<p><a href="/add_location">Add new location</a></p>
</div>
<!-- End Main Content-->
