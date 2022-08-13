<script>
	import { v4 as uuidv4 } from 'uuid'
	import Noty from 'noty'

	let editStatus = false

	let products = [
		{
			id: uuidv4(),
			name:'Hp pavilion',
			description:'Pc Gamer',
			category:'laptop',
			imgURL:'https://w7.pngwing.com/pngs/966/253/png-transparent-laptop-gaming-computer-lenovo-personal-computer-laptop-electronics-netbook-computer.png'
		},
		{
			id: uuidv4(),
			name:'Mouse HP',
			description:'Mouse Gamer',
			category:'peripherials',
			imgURL:'https://img2.freepng.es/20180609/oh/kisspng-computer-mouse-peripheral-laser-mouse-gamer-button-5b1be4eff15d74.7099632015285547359886.jpg'
		},
		{
			id: uuidv4(),
			name:'Teclado Dell',
			description:'Teclado Gamer',
			category:'peripherials',
			imgURL:'https://w7.pngwing.com/pngs/976/260/png-transparent-computer-keyboard-bg-teclado-gaming-r-force-macbook-pro-membrane-keyboard-gaming-keypad-force-de-laplace-computer-keyboard-computer-input-device.png'
		}
	]

	let product = {
		id:'',
		name:'',
		description:'',
		category:'',
		imgURL:''
	}

	const cleanProduct = ()=>{
		product = {
			id:'',
			name:'',
			description:'',
			category:'',
			imgURL:''
		}
	}

	const submitHandler = ()=>{
		if (editStatus){
			updateProduct()
		}else{
			addProduct()
		}
	}

	const addProduct = ()=>{
		let newProduct = {
			id: uuidv4(),
			name: product.name,
			description:product.description,
			category: product.category,
			imgURL: product.imgURL
		}
		products = products.concat(newProduct)

		new Noty({
			theme: 'sunset',
			type: 'success',
			layout: 'bottomRight',
			timeout: 3500,
			text: 'Se ha agregado un nuevo producto.'
		}).show()

		cleanProduct()
	}

	const deleteProduct = id =>{
		products = products.filter(product => product.id !== id)
		new Noty({
			theme: 'sunset',
			type: 'success',
			layout: 'bottomRight',
			timeout: 3500,
			text: `Se eliminó el producto correctamente.`,
		}).show()

		cleanProduct()
	}

	const editProduct = editProduct =>{
		product = editProduct
		editStatus = true
	}

	const updateProduct = ()=>{
		let index = products.findIndex(p=>p.id === product.id)
		products[index] = product

		new Noty({
			theme: 'sunset',
			type: 'success',
			layout: 'bottomRight',
			timeout: 3500,
			text: `El producto ${product.name} ha sido actualizado.`
		}).show()

		editStatus = false
		cleanProduct()
	}

</script>

<main>
	<div class="container p-3">
		<div class="row">

			<!-- Columna Izquierda -->
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault = {submitHandler}>
							<div class="form-group">
								<input bind:value={product.name} id="name" type="text" placeholder="Name product" class="form-control">
							</div>
							<div class="form-group">
								<textarea bind:value={product.description} id="description" placeholder="Description" rows="3" class="form-control"></textarea>
							</div>
							<div class="form-group">
								<select bind:value={product.category} id="category" class="form-control">
									<option value="laptops">Laptops</option>
									<option value="peripherials">Peripherials</option>
									<option value="servers">Servers</option>
								</select>
							</div>
							<div class="form-group">
								<input bind:value={product.imgURL} id="imgURL" type="url" placeholder="https://upload.wikimedia.org/wikipedia/commons/6/65/No-Image-Placeholder.svg" class="form-control">
							</div>
							<button class="btn btn-primary"> {#if !editStatus} Save Product {:else} Update Product {/if} </button>
						</form>
					</div>
				</div>
			</div>

			<!-- Columna Derecha -->
			<div class="col-md-6">
				<ul>
					{#each products as {id, name, description, category, imgURL}, i}

						<div class="card mb-2">
							<div class="row mb-2">
								<div class="col-md-4">
									{#if imgURL}
										<img src="{imgURL}" alt="" class="img-fluid p-2">
									{:else}
										<img src="img/noProduct.png" alt="" class="img-fluid p-2">
									{/if}
									
								</div>
								<div class="col-md-8">
									<div class="card-body">
										<h5>
											  <strong>{name}</strong>	
											  <span><small>-{category}</small></span>
										</h5>										
										<p class="card-text">{description}</p>	
									</div>
									<button on:click={deleteProduct(id)} class="btn btn-danger">Delete</button>
									<button on:click={editProduct({id, name, description, category, imgURL})} class="btn btn-success">Edit</button>
								</div>
							</div>
						</div>

					{/each}
				</ul>
			</div>

		</div>
	</div>

</main>

<style>
</style>