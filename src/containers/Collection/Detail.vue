<template>
	<div class="Poem">
		<div class="App__inner App__inner--padded">

			<section v-if="isPoemLoaded">

				<h2 v-text="poem.title"/>

				<p
					v-if="poem.text"
					v-text="poem.text"/>

				<p v-if="poem.image">
					<img thumbnail :src="poem.image"/>
				</p>

				<iframe
					v-if="poem.video"
					:src="poem.video"
					width="640"
					height="360"
					frameborder="0"
					webkitallowfullscreen
					mozallowfullscreen
					allowfullscreen/>

			</section>

			<app-loader v-else/>

		</div>
	</div>
</template>


<script>
import { mapGetters, mapActions } from 'vuex';
import AppLoader from '../../components/AppLoader';

export default {
	components: {
		AppLoader
	},
	computed: {
		...mapGetters({
			posts: 'post/posts'
		}),
		poem() {
			return this.posts[this.$route.params.id];
		},
		isPoemLoaded() {
			if (!this.poem) return false;
			return true;
		}
	},
	methods: {
		...mapActions({
			fetchPoems: 'post/fetchList'
		})
	},
	created() {
		// #Todo: look at improving loading so we don't load all poems to load a specific poem's page
		this.fetchPoems();
	}
};
</script>


<style lang="scss" scoped>

@import '../../settings';

.Poem{

	img, iframe{
		@media all and (max-width: $Mobile-Width) {
			max-width: 100%;
		}
	}

}

</style>
