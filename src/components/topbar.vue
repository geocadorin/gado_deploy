<script>
import { authComputed } from '@state/helpers'
import VuePerfectScrollbar from 'vue-perfect-scrollbar'

export default {
	components: {
		VuePerfectScrollbar,
	},
	props: {
		user: {
			type: Object,
			required: false,
			default: () => ({}),
		},
		isMenuOpened: {
			type: Boolean,
			default: false,
		},
	},
	data() {
		return {}
	},
	computed: {
		...authComputed,
	},
	methods: {
		toggleMenu() {
			this.$parent.toggleMenu()
		},
		toggleRightSidebar() {
			this.$parent.toggleRightSidebar()
		},
	},
}
</script>

<template>
	<!-- Topbar Start -->
	<div class="navbar navbar-expand flex-column flex-md-row navbar-custom">
		<div class="container-fluid">
			<!-- LOGO -->
			<a href="/" class="navbar-brand mr-0 mr-md-2 logo">
				<span class="logo-lg">
					<img src="@assets/images/logo.png" alt height="24" />
					<span class="d-inline h5 ml-2 text-logo">Gado de</span><span class="d-inline h5  text-logo" style="color: #ffd700;"> Ouro</span>
				</span>
				<span class="logo-sm">
					<img src="@assets/images/logo.png" alt height="24" />
				</span>
			</a>

			<ul
				class="navbar-nav bd-navbar-nav flex-row list-unstyled menu-left mb-0"
			>
				<li class>
					<button
						class="button-menu-mobile open-left disable-btn"
						:class="{ open: isMenuOpened }"
						@click="toggleMenu"
					>
						<feather type="menu" class="menu-icon align-middle"></feather>
						<feather type="x" class="close-icon"></feather>
					</button>
				</li>
			</ul>

			<ul
				class="navbar-nav flex-row ml-auto d-flex list-unstyled topnav-menu float-right mb-0"
			>
				<li class="d-none d-sm-block">
					<div class="app-search">
						<form>
							<div class="input-group">
								<input
									type="text"
									class="form-control"
									placeholder="Buscar..."
								/>
								<feather type="search" class="align-middle"></feather>
							</div>
						</form>
					</div>
				</li>

				<b-nav-item-dropdown
					id="globe-tooltip"
					right
					variant="black"
					class="dropdown d-none d-lg-block"
					no-caret
				>
					<template v-slot:button-content>
						<feather type="globe"></feather>
					</template>
					<b-tooltip target="globe-tooltip" placement="left"
						>Mudar Idioma</b-tooltip
					>
					<!-- item-->
					<b-dropdown-text href="javascript:void(0);" class="notify-item">
						<img
							src="@assets/images/flags/germany.jpg"
							alt="user-image"
							class="mr-2"
							height="12"
						/>
						<span class="align-middle">Alem??o</span>
					</b-dropdown-text>

					<!-- item-->
					<b-dropdown-text href="javascript:void(0);" class="notify-item">
						<img
							src="@assets/images/flags/italy.jpg"
							alt="user-image"
							class="mr-2"
							height="12"
						/>
						<span class="align-middle">Italiano</span>
					</b-dropdown-text>

					<!-- item-->
					<b-dropdown-text href="javascript:void(0);" class="notify-item">
						<img
							src="@assets/images/flags/spain.jpg"
							alt="user-image"
							class="mr-2"
							height="12"
						/>
						<span class="align-middle">Espanhol</span>
					</b-dropdown-text>

					<!-- item-->
					<b-dropdown-text href="javascript:void(0);" class="notify-item">
						<img
							src="@assets/images/flags/russia.jpg"
							alt="user-image"
							class="mr-2"
							height="12"
						/>
						<span class="align-middle">Russo</span>
					</b-dropdown-text>
				</b-nav-item-dropdown>

				<b-nav-item-dropdown
					id="bell-notification"
					right
					variant="white"
					class="notification-list"
					title="8 new unread notifications"
					menu-class="dropdown-lg"
				>
					<template v-slot:button-content>
						<feather type="bell" class="align-middle"></feather>
						<span class="noti-icon-badge"></span>
					</template>
					<b-tooltip target="bell-notification" placement="left"
						>1 novas notifica????es n??o lidas</b-tooltip
					>
					<!-- item-->
					<b-dropdown-text class="noti-title border-bottom pb-2" tag="div">
						<h5 class="m-0 font-size-16">
							<span class="float-right">
								<a href class="text-dark">
									<small>Limpar todas</small>
								</a> </span
							>Notifica????o
						</h5>
					</b-dropdown-text>

					<VuePerfectScrollbar v-once class="noti-scroll">
						<!-- item-->
						<b-dropdown-text
							href="javascript:void(0);"
							class="notify-item border-bottom"
						>
							<div class="notify-icon bg-primary">
								<i class="uil uil-balance-scale"></i>
							</div>
							<p class="notify-details">
								Nova pesagem registrada.
								<small class="text-muted">5 horas atr??s</small>
							</p>
						</b-dropdown-text>

						
					</VuePerfectScrollbar>
					<!-- All-->
					<b-dropdown-text
						href="javascript:void(0);"
						class="text-center text-primary notify-item notify-all border-top"
					>
						Ver Todos
						<i class="fi-arrow-right"></i>
					</b-dropdown-text>
				</b-nav-item-dropdown>

				<li
					id="setting-tooltip"
					class="dropdown notification-list"
					title="Settings"
				>
					<a
						href="javascript:void(0);"
						class="nav-link right-bar-toggle toggle-right"
						@click="toggleRightSidebar"
					>
						<feather type="settings" class="toggle-right"></feather>
					</a>
					<b-tooltip target="setting-tooltip" placement="left"
						>Configura????es</b-tooltip
					>
				</li>

				<b-nav-item-dropdown
					right
					class="notification-list align-self-center profile-dropdown"
					toggle-class="nav-user mr-0"
				>
					<template v-slot:button-content>
						<div class="media user-profile">
							<img
								src="@assets/images/users/avatar-7.jpg"
								alt="user-image"
								class="rounded-circle align-self-center"
							/>
							<div class="media-body text-left">
								<h6 class="pro-user-name ml-2 my-0">
									<span>{{ user.name }}</span>
									<span class="pro-user-desc text-muted d-block mt-1"
										>Proriet??rio</span
									>
								</h6>
							</div>
							<feather
								type="chevron-down"
								class="ml-2 align-self-center"
							></feather>
						</div>
					</template>
					<b-dropdown-item href="/pages/profile" class="notify-item p-0">
						<feather type="user" class="icon-dual icon-xs mr-2"></feather>
						<span>Minha Conta</span>
					</b-dropdown-item>

					<b-dropdown-item href="javascript:void(0);" class="notify-item p-0">
						<feather type="settings" class="icon-dual icon-xs mr-2"></feather>
						<span>Configura????es</span>
					</b-dropdown-item>

					<b-dropdown-item href="javascript:void(0);" class="notify-item p-0">
						<feather
							type="help-circle"
							class="icon-dual icon-xs mr-2"
						></feather>
						<span>Suporte</span>
					</b-dropdown-item>

					

					<b-dropdown-divider></b-dropdown-divider>

					<b-dropdown-item href="/logout" class="notify-item p-0">
						<feather type="log-out" class="icon-dual icon-xs mr-2"></feather>
						<span>Sair</span>
					</b-dropdown-item>
				</b-nav-item-dropdown>
			</ul>
		</div>
	</div>
	<!-- end Topbar -->
</template>

<style lang="scss">
.noti-scroll {
	height: 220px;
}
.ps > .ps__scrollbar-y-rail {
	width: 8px !important;
	background-color: transparent !important;
}
.ps > .ps__scrollbar-y-rail > .ps__scrollbar-y,
.ps.ps--in-scrolling.ps--y > .ps__scrollbar-y-rail > .ps__scrollbar-y,
.ps > .ps__scrollbar-y-rail:active > .ps__scrollbar-y,
.ps > .ps__scrollbar-y-rail:hover > .ps__scrollbar-y {
	width: 6px !important;
}
.button-menu-mobile {
	outline: none !important;
}
</style>
