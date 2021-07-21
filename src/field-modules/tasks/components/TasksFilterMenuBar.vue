<template>
  <farm-menu-bar>
    <template #left-menu>
      <router-link :to="{ name: 'tasks-all' }" tag="li">
        <icon-arrow-back/>
      </router-link>
      <li>{{ $t('日志分类筛选')}}</li>
    </template>
    <template #more-menu>
      <li @click="$emit('reset-display-filters')">{{ $t('Reset filters')}}</li>
    </template>
  </farm-menu-bar>
</template>

<script>
export default {
  name: 'TasksFilterMenuBar',
  data() {
    return {
      showMore: false,
    };
  },

};
</script>
