<template>
  <div
    class="flex flex-col bg-light-layer-0 px-4 text-light-text dark:bg-dark-layer-0 dark:text-dark-text xl:px-8"
  >
    <Head>
      <Title
        >{{ event.name }}&nbsp;{{
          $t("pages._global.tasks.tasks_lower")
        }}</Title
      >
    </Head>
    <HeaderAppPage
      :event="event"
      :header="event.name + ' ' + $t('pages._global.tasks.tasks_lower')"
      :tagline="$t('pages._global.tasks.tasks_page_tagline')"
      :underDevelopment="true"
    >
      <div class="flex space-x-2 lg:space-x-3">
        <BtnAction
          class="w-max"
          :cta="true"
          label="pages._global.tasks.new_task"
          fontSize="sm"
          :leftIcon="IconMap.PLUS"
          iconSize="1.35em"
          ariaLabel="pages._global.tasks.new_task_aria_label"
        />
      </div>
    </HeaderAppPage>
    <PagePreviewTasks />
    <!-- <div v-if="event.tasks" class="space-y-3 py-4"></div>
    <EmptyState v-else pageType="tasks" :permission="false" /> -->
  </div>
</template>

<script setup lang="ts">
import { IconMap } from "~/types/icon-map";
</script>
