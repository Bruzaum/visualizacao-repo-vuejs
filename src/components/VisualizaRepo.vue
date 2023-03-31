<!-- eslint-disable vue/no-multiple-template-root -->
<!-- eslint-disable no-mixed-spaces-and-tabs -->
<template>
    <link rel="stylesheet" href="https://unicons.iconscout.com/release/v4.0.8/css/line.css">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;500;700&display=swap" rel="stylesheet">

    <div>
        <div class="header">
            <h1 class="header-txt">
            <i class="uil uil-github"></i>
            <span class="header-txt--strong">Github</span> profiles
            </h1>
            <button @click="$emit('ShowView2'), $emit('resetUser')" class="change-username">CHANGE USERNAME</button>
        </div>
    </div>

    <div v-if="showRepo" class="container showRepo">
		<header class="container">
	
			<!-- InfoUser Header -->
			<div class="perfil mt-3">
				<img :src="avatar" alt="usuário" class="me-3" />
				<br />
				<div class="infoHeader">
					<h2>{{ name }}</h2>
				</div>
				<!-- Description -->
				<div class="bio">
					<span class="mt-3 mb-3">
						{{ bio }}
					</span>
				</div>
				<!-- End Description -->
			</div>
			<!-- End InfoUser Header -->
	
      	</header>
  
      <!-- MAIN -->
      	<main>
			<section class="container text-center">
				<nav class="total-repositories">
					<a class="total-repositories" href="#"  v-on:click="showRepositories(); addRepoClass();" :class="{'sublinhado': isRepoClass}">
						<h2>Repos </h2>
						<span class="total-repositories--counter">{{ totalRepositories }}</span>
					</a>

					<a class="total-repositories" href="#" v-on:click="showStar(); addStarClass();" :class="{'sublinhado': isStarClass}">
						<h2 class="starred">Starred </h2>
						<span class="total-repositories--counter">{{ totalStarRepositories }}</span>
					</a>
				</nav>		
		
				<div class="input-group mb-2">
					<input
						v-model="search"	
						class="form-control"
						placeholder="Filtre pelo nome"
					>
				</div>
		
				<!-- Repositories -->
				<div v-if="!isShowStar">
					<ul class="repositories overflow-auto">
						<li
						class="d-flex flex-column"
						v-for="(repository) in filtered_repos"
						:key="repository.name"
						>
						<div class="d-flex justify-content-between align-items-center">
							<h3 class="repoTitle">
							{{ repository.name }}
							</h3>
							<p>{{ repository.description }}</p>
						</div>
						<div class="d-flex justify-content-between align-items-center">
							<i class="uil uil-arrow"></i>
							<span> {{ repository.language }} </span>
							<i class="uil uil-code-branch"></i>
							<span> {{ repository.forks }} </span>
						</div>
						</li>
					</ul>
				</div>
				<div v-else>
					<ul class="repositories overflow-auto">
						<li
						class="d-flex flex-column"
						v-for="(repository) in filtered_star_repos"
						:key="repository.name"
						>
						<div class="d-flex justify-content-between align-items-center">
							<h3 class="repoTitle">
							{{ repository.name }}
							</h3>
							<p>{{ repository.description }}</p>
						</div>
						<div class="d-flex justify-content-between align-items-center">
							<i class="uil uil-arrow"></i>
							<span> {{ repository.language }} </span>
							<i class="uil uil-code-branch"></i>
							<span> {{ repository.forks }} </span>
						</div>
						</li>
					</ul>
				</div>
				
          		<!-- End Repositories -->
        	</section>
    	</main>
    </div> 
    <div
      	v-else
      	class="erroFind d-flex flex-column justify-content-center align-items-center"
    >
		<i class="uil uil-arrow-up"></i>
      	<h1>Usuário não Encontrado! </h1>
		<h2>Tente novamente clicando no botão indicado</h2>
    </div>
</template>
  
<script>
	export default {
		name: 'VisualizaRepo',
		emits: ['ShowView2', 'resetUser'],
		props: { user: String },
		data() {
		return {
			showRepo: Boolean,
			view: false,
			Auser: String,
			search: "",
			avatar: null,
			stars: null,
			bio: null,
			name: null,
			totalRepositories: null,
			totalStarRepositories: null,
			alert: null,
			repositories: [],
			starsRepositories: [],
			isRepoClass: true,
			isStarClass: false,
			isShowStar: false
		}
		},
		methods: {
		addRepoClass: function() {
			this.isRepoClass = true;
			this.isStarClass = false;
		},
		addStarClass: function() {
			this.isRepoClass = false;
			this.isStarClass = true;
		},
		showStar: function() {
			this.isShowStar = true;
		},
		showRepositories: function() {
			this.isShowStar = false;
		},
		async geTinfo() {
			this.Auser = this.user
			const url = `https://api.github.com/users/${this.Auser}`
			await fetch(url)
			.then(response => response.json())
			.then(infoGit => {
				this.avatar = infoGit.avatar_url
				this.bio = infoGit.bio
				this.name = infoGit.name
				this.totalRepositories = infoGit.public_repos
				this.stars = infoGit.starred_url
			})
			const urlRepositories = `https://api.github.com/users/${this.Auser}/repos`
			const urlStarredRepositories = `https://api.github.com/users/${this.Auser}/starred`
			await fetch(urlRepositories)
			.then(response => response.json())
			.then(infoRepositories => {
				this.repositories = infoRepositories
				console.log(this.repositories.index)
			})
			await fetch(urlStarredRepositories)
			.then(response => response.json())
			.then(infoRepositories => {
				this.starsRepositories  = infoRepositories
			})
			this.totalStarRepositories = this.starsRepositories.length
			this.checkRepo()
		},
		checkRepo() {
			if (
			this.totalRepositories === 0 ||
			this.totalRepositories === undefined
			) {
			this.showRepo = false
			} else {
			this.showRepo = true
			}
		}
		},
		created() {
		this.geTinfo()
		},
		computed:{
			filtered_repos() {
				return this.repositories.filter((item) => {
					return item.name.toLowerCase().includes(this.search.toLowerCase())
				})
			},
			filtered_star_repos() {
				return this.starsRepositories.filter((item) => {
					return item.name.toLowerCase().includes(this.search.toLowerCase())
				})
			},
		}
	}
</script>
  
<style scoped>
	* {
		box-sizing: border-box;
		list-style-type: none;
		margin: 0;
		padding: 0;
		text-decoration: none;
		font-family: "Open Sans",Arial,Helvetica;
	}

	.showRepo {
		display: flex;
	}
  .header {
        background-color: #5c646d;
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 80px;
        padding: 0 2rem 0 2rem;
        min-width: 100%;
    }
    .header-txt {
        letter-spacing: 1px;
        font-weight: 400;
        color: #fff;
        font-size: 1.8rem;
    }
    .header-txt--strong{
        font-weight: 700;
        color: #fff;
        font-size: 2rem!important;
    }
    .uil-github{
        margin-right: 0.2em;
        font-weight: 700;
        color: #fff;
        font-size: 2.5rem!important;
    }
    .change-username{
        background-color: #5c646d;
        color: #fff;
        border-color: #fff;
        border-radius: 8px;
        padding: 0.3rem;
        text-align:end;
		font-size: 0.6rem;
    }
    .change-username:hover {
        cursor: pointer;
        color: orange;
        border-color: orange;
    }
	/* HEADER */
	header {
		margin-top: 20px;
		max-width: 25%;
		min-height: 35%;
		max-height: 50%;
	}

	div.perfil {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	div.perfil img {
		width: 210px;
		height: 210px;
		border-radius: 50%;
		border: none;
	}
	div.infoHeader {
		font-weight: 700;
		font-size: 0.93rem;
	}
	header div.bio {
		padding: 0.1rem 0.8rem;
		text-align: center;
		font-size: 0.88rem;
	}
	/*END HEADER */

	/* MAIN */
	main {
		width: 70%;
		padding: 2rem;
	}
	.sublinhado {
		border-bottom: 5px solid #E36209;
	}
	.total-repositories {
		display: flex;
		align-items: center;
	}
	.total-repositories h2 {
		font-size: 1.5rem;
		color: #586069;
	}
	.total-repositories--counter {
		margin-left: 3px;
		background-color: #e3e3e3;
		border: 1px solid transparent;
		border-radius: 2rem;
		display: inline-block;
		line-height: 18px;
		min-width: 25px;
		padding: 0 6px;
		text-align: center;
		font-size: 1rem;
		font-weight: 900;
		color: #586069;
	}
	.starred {
		margin-left: 30px;
	}
	section .repositories {
		margin-top: 24px;
	}
	section .repositories li {
		border-bottom: 1px solid #E1E4E8;
		padding: 8px;
		color: #586069;
		min-height: 15vh;
	}

	section .repositories p {
		font-size: 0.9rem;
		font-weight: 400;
		margin-block-start: 1em;
		margin-block-end: 1em;
		margin-inline-start: 0px;
		margin-inline-end: 0px;
	}

	.repoTitle {
		font-weight: 500;
		color: #389adb;
	}

	.uil-arrow {
		margin-right: 3px;
	}

	.uil-code-branch {
		margin-left: 30px;
		margin-right: 3px;
	}
	/* END MAIN */
	.erroFind {
		justify-content: center;
		height: 80vh;
		margin: 0 auto;
		text-align: center;
	}

	.uil-arrow-up{
		font-size: 5rem!important;
		display: flex;
		justify-content: flex-end;
		margin-right: 2.5rem;
	}

	.form-control {
		height: 40px;
		padding: 1rem;
		margin-top: 25px;
		min-width: 40%;
		border-radius: 6px;
	}
</style>