<script>
	import { onMount } from 'svelte';
    let APIURL ="http://localhost:3000/book";
	let listBooks = [];
	let nombre = '';
	let edicion = '';
	
	onMount(() => {
	    getBooks()  
	});

	async function getBooks(){
		const res = await fetch(APIURL);
		const books = await res.json();
		listBooks = books;
	}

	async function addBook() {
		let obj = { nombre, edicion };
		const res = await fetch(APIURL, {
			 method: 'POST',
			 body: JSON.stringify(obj),
			 headers:{
                'Content-Type': 'application/json'
               }
			});
		   console.log(await res.json());
		   getBooks()
		   clearInput() 
	}

	function clearInput(){
	   nombre = '';
	   edicion = '';
	}

	async function deleteBook(id){
		const res = await fetch(APIURL+`/${id}`, { method: 'DELETE'});
	    console.log(await res.json());  
	    getBooks()         
	}

</script>

<main>
 
	<div>
		<div class="form">
			<input bind:value={nombre} placeholder="Nombre">
			<input bind:value={edicion} placeholder="Edicion">
			<button class="btn" on:click={addBook}>create</button>
		</div>
		
		{#each listBooks as book}
		<div class="card">
			<p style="color:#000">{book.nombre} - {book.edicion}</p>
			<button class="btn-delete" on:click={deleteBook(book._id)}>
		 	delete</button>
		</div>
		 {:else}
			<!-- this block renders when listBooks.length === 0 -->
			<p>loading...</p>
		{/each}
	
	</div>

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}

.btn {
	 padding: 7px 32px;
	 background-color:#9736DA;
	 text-align: center;
	 color:cornsilk;
	 border-radius: 8px;
	 font-size: 17px;
	}

.btn-delete{
	padding: 7px 13px;
	 background-color:crimson;
	 text-align: center;
	 color:cornsilk;
	 border-radius: 8px;
	 font-size: 17px;
}	

.btn:hover{
	background-color: crimson;
}	

.card {
  position: relative;
  margin: auto;
  height: 90px;
  width: 240px;
  text-align: center;
  border-radius: 2px;
  box-shadow: 0 6px 12px -3px rgba(0,0,0,.3);
  color: #fff;
  padding: 30px;
}

.card:hover{
	height: 120px;
    width: 340px;
}

.form{
  margin: auto;
  height: 110px;
  width: 240px;
  text-align: center;
  border-radius: 4px;
  box-shadow: 0 6px 12px -3px rgba(0,0,0,.3);
  color: #fff;
  padding: 30px;
  margin-bottom: 20px;
}

</style>