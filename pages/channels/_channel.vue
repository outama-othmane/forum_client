<template>
	<div class="w-full pt-10 md:pt-20 pr-12 pb-5 pl-12">
		<div class="w-full">
			<div class="flex pb-1 mb-4 border-b border-solid border-gray-200">
				<h3 class="m-0 tracking-tight font-medium mb-1 text-lg text-gray-900">
					{{ 
					filters.filter((e) => e.name == filter)[0].value 
						 || 'Discussions' }} 
					in 
					{{ channelName }}
				</h3>
			</div>
		</div>
		<div>
			<select class="inline-block text-white bg-gray-500 py-2 px-3 rounded transition-all duration-200 ease-in-out border-none outline-none text-sm" v-model="filter" v-on:change="filteredPosts">
				<template v-for="option in filters">
					<option :value="option.name" :key="option.name">{{ option.value }}</option>
				</template>
			</select>
		</div>
		<div class="mt-8">
			<template v-for="discussion in discussions">
				<Discussion :discussion="discussion" :key="'discussion' + discussion.id" />
			</template>
			<template v-if="$fetchState.pending">
				<template v-for="i in (discussions.length > 0) ? 1 : 5">
					<ShadowDiscussion :key="'shadow' + i" />
				</template>
			</template>
		</div>
		<template v-if="!($fetchState.pending || page == lastPage)">
			<div class="flex w-full justify-center mt-6">
				<a href="#" class="inline-block bg-gray-500 text-white px-3 py-2 rounded transition-all duration-200 ease-in-out hover:bg-opacity-75" @click.prevent="loadMore">Load more</a>
			</div>
		</template>
		<template v-if="discussions.length > 0 && page == lastPage && !$fetchState.pending">
			<alert class="w-full relative mt-4">You reach all the discussions.</alert>
		</template>
		<template v-if="discussions.length <= 0 && !$fetchState.pending && lastPage == 1">
			<alert class="w-full relative mt-4">No discussions found.</alert>
		</template>
		<div v-observe-visibility="(discussions.length > 0 && !$fetchState.pending) ? lazyLoadDiscussions : false"></div>
	</div>
</template>

<script>
import Alert from '~/components/Cards/Alert'
import Discussion from '~/components/Discussions/Discussion'
import ShadowDiscussion from '~/components/Shadows/ShadowDiscussion'
import { mapGetters } from 'vuex'
export default {
	data() {
		return {
			discussions: [],
			page: 1,
			lastPage: 1,
			filter: 'latest',
			filtersValue: {
				latest: 'latest=true',
				activity: 'activity=desc',
				closed: 'closed=true',
				opened: 'closed=false',
				no_posts: 'no_posts=true',
			}
		}
	},
	components: {
		Discussion,
		ShadowDiscussion,
		Alert
	},
	computed: {
		...mapGetters({
			channels: 'channels'
		}),
		filters() {
			return [
				{ name: 'latest', value: 'Latest' },
				// { name: 'popular_this_week', value: 'Popular this week' },
				{ name: 'activity', value: 'Popular all time' },
				{ name: 'closed', value: 'Closed' },
				{ name: 'opened', value: 'Opened' },
				{ name: 'no_posts', value: 'No replies yet' },
			]
		},
		channelName() {
			let channel = this.channels.filter((c) => c.slug == this.$route.params.channel)
			if (channel.length > 0) {
				return channel[0].name
			}
			return this.$route.params.channel
		}
	},
	async fetch() {
		try {
			let discussions = await this.$axios.$get(`discussions?page=${this.page}&${this.filtersValue[this.filter]}&channel=${this.$route.params.channel}`)
			this.discussions.push(...discussions.data)
			this.lastPage = discussions.meta.last_page
		} catch(e) {
			this.$nuxt.error({ statusCode: 500, message: 'Server error!' })
		}
	},
	fetchOnServer: false,
	methods: {
		async loadMore() {
			if (this.lastPage <= this.page) { return }
			this.page++

			this.$fetch()
		},
		lazyLoadDiscussions(isVisible) {
			if (!isVisible) { return }
			if (this.page >= this.lastPage) { return }

			this.page++
			this.$fetch()
		},
		async filteredPosts() {
			this.page = 1
			this.discussions = []
			this.$fetch()
 		}
	},

	head() {
		return {
			title: `${this.channelName || 'Loading'} discussions | ${process.env.APP_NAME}`
		}
	},
};
</script>