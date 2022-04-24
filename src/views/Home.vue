<template>
  <div class="home">
    <h1>All Destinations</h1>
    <button @click="triggerRouterError">Trigger Router Error</button>
    <button @click="addDynamicRoute">Add Dynamic Route</button>
    <router-link to="/dynamic">Visit Dynamic Route</router-link>
    <div class="destinations">
      <router-link
        v-for="destination in destinations"
        :key="destination.id"
        :to="{
          name: 'destination.show',
          params: { id: destination.id, slug: destination.slug },
        }"
      >
        <h2>{{ destination.name }}</h2>
        <img :src="`/images/${destination.image}`" :alt="destination.name" />
      </router-link>
    </div>
  </div>
</template>

<script>
import sourceData from '@/data.json';
import { isNavigationFailure, NavigationFailureType } from 'vue-router';

export default {
  data() {
    return {
      destinations: sourceData.destinations,
    };
  },
  methods: {
    async triggerRouterError() {
      const navigationFailure = await this.$router.push('/');
      if (
        isNavigationFailure(navigationFailure, NavigationFailureType.duplicated)
      ) {
        // nav was prevented because we're already on the requested route (type duplicated)
        // false was returned inside of a navigation guard to the navigation (type aborted)
        // a new navigation took place before the current navigation could finish (type cancelled)
        // example props
        // navigationFailure.to
        // navigationFailure.from.fullPath
      } else {
        // all is well
      }
    },
    addDynamicRoute() {
      this.$router.addRoute({
        name: 'dynamic',
        path: '/dynamic',
        component: () => import('@/views/Login.vue'),
      });
      this.$router.removeRoute('dynamic');
    },
  },
};
</script>
