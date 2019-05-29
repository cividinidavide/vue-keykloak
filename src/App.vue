<template>
	<div id="app">
		<img alt="Vue logo" src="./assets/logo.png">
		<div v-if="authenticated">
			<p>Ciao {{ keycloak.tokenParsed.given_name }}, sei loggato!</p>
			<button @click="logout()">Logout</button>
		</div>
		<p v-if="!authenticated">Non sei autenticato!</p>
	</div>
</template>

<script lang="ts">
	import { Component, Vue } from 'vue-property-decorator';
	import Keycloak from 'keycloak-js';

	@Component({})
	export default class App extends Vue {
		keycloak: any;
		authenticated: any = null;

		mounted() {
			const keycloak = Keycloak('../keykloak.json');
			keycloak.init({onLoad: 'login-required'}).success(authenticated => {
				this.keycloak = keycloak;
				console.log(this.keycloak);
				this.authenticated = authenticated;
			})
		}

		logout() {
			this.keycloak.logout();
		}
	}
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
