<template>	
	<div>
		<div class="row">
			<div class="col-md-6 col-lg-6">				
				<div class="panel panel-info">
					<div class="panel-heading">
						<h3 class="panel-title">{{usuario.usuario}}</h3>
					</div>
					<div class="panel-body">
						<div class="row">
							<div class=" col-md-9 col-lg-9 "> 
								<table class="table table-user-information">
									<tbody>
										<tr>
											<td>Nombre:</td>
											<td>{{usuario.nombre}}</td>
										</tr>
										<tr>
											<td>Apellidos:</td>
											<td>{{usuario.apellidos}}</td>
										</tr>
										<tr>
											<td>Correo electrónico</td>
											<td>{{usuario.email}}</td>
										</tr>

										<tr v-if="usuario.tipo == 'vendedor'">
											<td>Ruta</td>
											<td>{{usuario.ruta}}</td>
										</tr>				<tr v-if="usuario.tipo == 'vendedor'">
											<td>Clasificación</td>
											<td>{{usuario.clasificacion}}</td>
										</tr>
										<tr v-if="usuario.tipo != 'vendedor'">
											<td>Teléfono</td>
											<td>{{usuario.telefono}}</td>
										</tr>
										<tr v-if="usuario.tipo != 'vendedor'">
											<td>Dirección envío</td>
											<td>
												{{usuario.calle}} 
												#{{usuario.numero_ext}}, {{usuario.colonia}}, {{usuario.municipio.municipio}}
												,{{usuario.estado.estado}},
												CP {{usuario.cp}}
											</td>
										</tr>												
										<td v-if="usuario.tipo != 'vendedor'">RFC</td>
										<td>{{usuario.rfc_factura}}
										</td>
										<tr v-if="usuario.tipo != 'vendedor'">

											<td>Num Cliente</td>
											<td>{{usuario.numero_cliente}}
											</td>
										</tr>


									</tbody>
								</table>
								<div class="btn-group" role="group" aria-label="Basic example">
									<button type="button" @click="onActulizarPerfil" class="btn btn-primary">Actualizar Mis pedidos</button>
									<router-link :to="{name: 'EditarPerfil'}" class="btn btn-secondary" v-if="usuario.tipo != 'vendedor'">
										Editar perfil
									</router-link>
									<router-link :to="{name: 'CambiarContrasena'}" class="btn btn-success">
										Cambiar contraseña
									</router-link>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="col-md-6 col-lg-6">
				<h3>Pedidos</h3>
				<table class="table table-striped">
					<thead>
						<th>ID</th>
						<th>Estado</th>
						<th>Pedidos</th>
						<th>Total</th>
					</thead>
					<tbody>
						<tr v-for="pedido in usuario.pedidos" :key="pedido.id">
							<td>{{pedido.id}}</td>
							<td>
								<span v-if="pedido.status==0" class="badge badge-danger">
								Cancelado</span>
								<span v-if="pedido.status==1" class="badge badge-warning">Activo</span>
								<span v-if="pedido.status==2" class="badge badge-info">Procesando</span>
								<span v-if="pedido.status==3" class="badge badge-success">Entregado</span>
							</td>
							<td>
								<ul>
									<li v-for="articulo in pedido.articulos" :key="articulo.id" class="fs10">
										{{articulo.articulo.clave}}
										{{articulo.articulo.descripcion}}
										{{articulo.articulo.cantidad}}
									</li>
								</ul>
							</td>
							<td>${{pedido.total}}</td>
						</tr>

					</tbody>
				</table>
			</div>
		</div>

	</div>
</template>

<script>
export default {

	name: 'Perfil',

	data () {
		return {

		}		
	},
	computed:{
		usuario(){
			return this.$store.getters.usuario
		}
	},
	created(){
		this.$store.dispatch('cargarMunicipios',
			{'estado':this.$store.getters.usuario.estado.id})		
		this.$store.dispatch('cargarPerfilUsuario')
	},
	methods: {
		onActulizarPerfil(){
			this.$store.dispatch('cargarPerfilUsuario')
			this.usuario = this.$store.getters.perfilUsuario
		}
	}

}
</script>

<style lang="css" scoped>


</style>