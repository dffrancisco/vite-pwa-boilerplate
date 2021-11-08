<script setup lang="ts">
import { ref } from 'vue'
import globalStore from '@/globalStore';
import loginStore from "@/pages/login/login"

globalStore.state.title = 'Home'

const show = ref(false);
const showPopup = () => {
	show.value = true;
};

</script>

<template>
	<!-- <van-nav-bar title="Title" left-text="Back" left-arrow @click-left="$" /> -->
	<van-cell-group inset>
		<van-field label="Text" placeholder="legal" />
		<van-field type="tel" label="Phone" />
		<van-field type="digit" label="Digit" />
		<van-field type="number" label="Number" />
		<van-field type="password" label="Password" />
	</van-cell-group>

	<van-cell-group class="mt-2" inset>
		<van-cell is-link @click="showPopup">Show Popup</van-cell>
	</van-cell-group>

	<van-popup
		v-model:show="show"
		closeable
		close-icon-position="top-right"
		round
		position="bottom"
		:style="{ height: '30%' }"
	/>
	<van-button class="ma-10" size="small" @click="loginStore.actions.logOut()" type="warning">Sair</van-button>
</template>



<route>
  {
	  meta:{
		  xico:'alves'
	  }
  }
</route>
