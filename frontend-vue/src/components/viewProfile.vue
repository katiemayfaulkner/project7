<template>
	<div>
		<section class="container view-profile">
			<router-link to="/home">
				<p> Return </p>
			</router-link>
			<div class="content">
				<h1> My account: </h1> 

				<div class="profile">
					<div class="user-img"> 
						<img :src="user.userImg" v-if="user.userImg">
					</div>
					<div>
						<p class="title">First name:</p>
						<p> {{user.firstName}}</p>
					</div>
					<div>
						<p class="title">Last name:</p>
						<p> {{user.lastName}} </p>
					</div>
					<div>
						<p class="title">Email:</p>
						<p> {{user.email}}</p>
					</div>
				</div>

				<div class="profile-btns">
					<input type="button" value="Logout" @click="logoutUser()">
					<router-link to="/edit-profile">
						<input type="button" value="Edit profile">
					</router-link>
					<input type="button" value="Delete account" @click="deleteUser()">
				</div>
			</div>
		</section>
	</div>
</template>

<script>

// import the promise-based library used with Node.js + your browser to make asynchronous Js HTTP requests
import axios from 'axios'; 

export default {
	name: 'viewProfile',
	data() {
		return {
			user: {},
		};
	},
	methods: {	

		getUser() {

			let userId = JSON.parse(window.localStorage.getItem('user')).userId;

			axios.get("http://localhost:3000/user/" + userId)
			.then(res => {

				this.user = res.data;				
			})
			.catch(error => {
				console.error(error);
			})
		},

		logoutUser() {
			this.$store.commit('RESET');
			localStorage.clear();
			this.$router.push({ name: "Welcome" });
			console.log('You have successfully been logged out.')
		},

		deleteUser() {
			let userId = JSON.parse(window.localStorage.getItem('user')).userId;

			axios.delete('http://localhost:3000/user/' + userId,
			).then(res => {	
				this.logoutUser();
				console.log('Your account has successfully been deleted!');
			})
			.catch(function (err) {
				console.log("Error", err);
			})
		},
	},
	beforeMount() {
		this.getUser()
	}
}
</script>

<style lang="scss">

.view-profile{
	margin-top: 30px;

	a{
		text-decoration: none;
		color: black;
		font-size: 18px;

		p{
			margin-left: 20px;
			font-weight: 500;
		}

	}
	
	.content{
		padding: 25px;
		width: 100%;
		height: auto;
		background-color: #f2f2f2;
		border-radius: 12px;
		box-shadow: rgba(35, 35, 65, 0.25) 0px 6px 12px -2px, rgba(3, 3, 3, 0.418) 0px 3px 7px -3px;

		h1{
			font-size: 22px;
			font-weight: 700px;	
		}

		.profile {
			margin: 30px 0;

			.user-img {
				border: 2px solid black;
				width: 64px;
				height: 64px;
				border-radius: 50%;
				overflow: hidden;
				margin-bottom: 15px;
			
				img {
					width: 100%;
					height: 100%;
					object-fit: cover;

				}
			}

			div {
				display: flex;
				flex-wrap: wrap;
				color: black;
				
				.title {
					font-weight: 500;
				}

				p {
					padding-right: 10px;
					font-size: 1.2rem;
					font-weight: 100;
				}
			}
		}

		.profile-btns {
			input {
				padding: 10px;
				margin: 0 10px 15px 0;
				border-radius: 12px;
				border: none;
				background-color: #091F43;
				font-weight: 500;
				color: white;
				font-size: 17px;
			}
		}
	}
}

@media only screen and (max-width: 770px) {
	.view-profile {
		.content {
			padding: 15px;

			.profile-btns {
				display: flex;
				flex-wrap: nowrap;

				input {
					font-size: 16px;
					margin: 0 5px 15px 0;
				}
			}
		}
	}
}

</style>