<template>
  <main class="main">
    <Hero
      :item="featured" />

    <ListingCarousel
      v-if="popular && popular.results.length"
      :title="popularTitle"
      :view-all-url="popularUrl"
      :items="popular" />

    <ListingCarousel
      v-if="topRated && topRated.results.length"
      :title="topRatedTitle"
      :view-all-url="topRatedUrl"
      :items="topRated" />

    <ListingCarousel
      v-if="onAir && onAir.results.length"
      :title="onAirTitle"
      :view-all-url="onAirUrl"
      :items="onAir" />

    <ListingCarousel
      v-if="airingToday && airingToday.results.length"
      :title="airingTodayTitle"
      :view-all-url="airingTodayUrl"
      :items="airingToday" />
  </main>
</template>

<script>
import { getTvShows, getTvShow, getListItem } from '~/api';
import Hero from '~/components/Hero';
import ListingCarousel from '~/components/ListingCarousel';

export default {
  components: {
    Hero,
    ListingCarousel,
  },

  head () {
    return {
      title: 'Series',
      meta: [
        { hid: 'og:title', property: 'og:title', content: 'Series' },
        { hid: 'og:url', property: 'og:url', content: `${process.env.FRONTEND_URL}${this.$route.path}` },
      ],
    };
  },

  computed: {
    popularTitle () {
      return getListItem('series', 'popular').title;
    },

    popularUrl () {
      return { name: 'series-category-name', params: { name: 'popular' } };
    },

    topRatedTitle () {
      return getListItem('series', 'top_rated').title;
    },

    topRatedUrl () {
      return { name: 'series-category-name', params: { name: 'top_rated' } };
    },

    onAirTitle () {
      return getListItem('series', 'on_the_air').title;
    },

    onAirUrl () {
      return { name: 'series-category-name', params: { name: 'on_the_air' } };
    },

    airingTodayTitle () {
      return getListItem('series', 'airing_today').title;
    },

    airingTodayUrl () {
      return { name: 'series-category-name', params: { name: 'airing_today' } };
    },
  },

  async asyncData ({ error }) {
    try {
      const popular = await getTvShows('popular');
      const topRated = await getTvShows('top_rated');
      const onAir = await getTvShows('on_the_air');
      const airingToday = await getTvShows('airing_today');
      const featured = await getTvShow(popular.results[0].id);

      return { popular, topRated, onAir, airingToday, featured };
    } catch {
      error({ statusCode: 504, message: 'Data not available' });
    }
  },
};
</script>
