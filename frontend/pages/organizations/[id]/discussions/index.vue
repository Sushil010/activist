<template>
  <div
    class="flex flex-col bg-light-layer-0 px-4 text-light-text dark:bg-dark-layer-0 dark:text-dark-text xl:px-8"
  >
    <Head>
      <Title
        >{{ organization.name }}&nbsp;{{
          $t("pages._global.discussions_lower")
        }}</Title
      >
    </Head>
    <HeaderAppPage
      :organization="organization"
      :header="organization.name + ' ' + $t('pages._global.discussions_lower')"
      :tagline="$t('pages.organizations.discussions.index.tagline')"
      :underDevelopment="true"
    >
      <div class="flex space-x-2 lg:space-x-3">
        <BtnRouteInternal
          v-if="aboveMediumBP"
          class="block w-max"
          :cta="true"
          linkTo="/"
          label="pages._global.new_discussion"
          fontSize="sm"
          :leftIcon="IconMap.PLUS"
          iconSize="1.35em"
          ariaLabel="pages._global.new_discussion_aria_label"
        />
      </div>
    </HeaderAppPage>
    <PagePreviewDiscussion />
    <!-- <div v-if="organization.discussions" class="space-y-6 pb-6 pt-3 md:pt-4">
      <CardDiscussion
        v-for="(d, i) in organization.discussions"
        :key="i"
        :isPrivate="false"
        :discussion="d"
      />
    </div>
    <EmptyState v-else pageType="discussions" :permission="false" /> -->
  </div>
</template>

<script setup lang="ts">
import useBreakpoint from "~/composables/useBreakpoint";
import { IconMap } from "~/types/icon-map";

const aboveMediumBP = useBreakpoint("md");

const idParam = useRoute().params.id;
const id = typeof idParam === "string" ? idParam : undefined;

const organizationStore = useOrganizationStore();
await organizationStore.fetchByID(id);

const { organization } = organizationStore;
</script>
