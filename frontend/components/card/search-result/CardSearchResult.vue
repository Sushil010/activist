<template>
  <div
    class="card-style flex flex-col justify-center px-3 py-4 md:grow md:flex-row md:justify-start md:py-3 lg:px-5"
  >
    <!-- MARK: fds -->
    <div class="relative flex w-full flex-col md:flex-row">
      <div class="flex w-full justify-center md:w-fit">
        <NuxtLink
          v-if="organization || group || event || user"
          :to="localePath(linkURL)"
          :aria-label="$t(ariaLabel)"
        >
          <div
            v-if="organization || group || event"
            class="h-min w-max rounded-md border border-light-section-div bg-light-layer-0 dark:border-dark-section-div dark:bg-dark-layer-0"
          >
            <img
              v-if="organization && imageURL"
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              :src="imageURL"
              :alt="
                $t('components.card_search_result.organization_img_alt_text') +
                ' ' +
                organization.name
              "
            />
            <img
              v-if="group && imageURL"
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              :src="imageURL"
              :alt="
                $t('components.card_search_result.organization_img_alt_text') +
                ' ' +
                group.organization.name
              "
            />
            <div
              v-else-if="(organization || group) && !imageURL"
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              class="flex items-center justify-center text-light-text dark:text-dark-text"
            >
              <Icon :name="IconMap.ORGANIZATION" class="h-[75%] w-[75%]" />
            </div>
            <div
              v-else-if="event"
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              class="flex items-center justify-center"
            >
              <ImageEvent
                v-if="event && eventType"
                :eventType="eventType"
                :imgURL="imageURL"
                :alt="
                  $t('components.card_search_result.event_img_alt_text', {
                    entity_name: name,
                  })
                "
              />
            </div>
          </div>
          <div
            v-if="user && !imageURL"
            class="w-fit rounded-full border border-light-section-div bg-light-layer-0 dark:border-dark-section-div dark:bg-dark-layer-0"
          >
            <div
              v-if="!imageURL"
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              class="flex items-center justify-center fill-light-text dark:fill-dark-text"
            >
              <Icon class="h-[75%] w-[75%]" :name="IconMap.PERSON" />
            </div>
          </div>
        </NuxtLink>
        <a
          v-else-if="resource"
          :href="linkURL"
          target="_blank"
          :aria-label="$t(ariaLabel)"
        >
          <div
            class="h-min rounded-md border border-light-section-div bg-light-layer-0 dark:border-dark-section-div dark:bg-dark-layer-0"
          >
            <div
              :class="{
                'h-[150px] w-[150px]': isReduced,
                'h-[200px] w-[200px]': !isReduced,
              }"
              class="flex items-center justify-center fill-light-text dark:fill-dark-text"
            >
              <Icon :name="IconMap.RESOURCE" class="h-[75%] w-[75%]" />
            </div>
          </div>
        </a>
      </div>
      <div class="flex-col space-y-2 pt-3 md:grow md:pl-4 md:pt-0 lg:pl-6">
        <div class="flex flex-col justify-between md:flex-row">
          <div class="flex items-center justify-center space-x-2 md:space-x-4">
            <NuxtLink
              v-if="organization || group || event || user"
              :to="localePath(linkURL)"
              :aria-label="$t(ariaLabel)"
            >
              <h2 class="responsive-h3 font-bold">
                {{ name }}
              </h2></NuxtLink
            >
            <a
              v-else-if="resource"
              :href="linkURL"
              target="_blank"
              :aria-label="$t(ariaLabel)"
              ><h2 class="responsive-h3 font-bold">
                {{ name }}
              </h2></a
            >
            <MenuSearchResult
              v-if="organization"
              class="max-md:absolute max-md:right-0 max-md:top-0"
              :organization="organization"
            />
            <MenuSearchResult
              v-if="group"
              class="max-md:absolute max-md:right-0 max-md:top-0"
              :group="group"
            />
            <MenuSearchResult
              v-if="event"
              class="max-md:absolute max-md:right-0 max-md:top-0"
              :event="event"
            />
            <MenuSearchResult
              v-if="resource"
              class="max-md:absolute max-md:right-0 max-md:top-0"
              :resource="resource"
            />
            <MenuSearchResult
              v-if="user"
              class="max-md:absolute max-md:right-0 max-md:top-0"
              :user="user"
            />
          </div>
          <div
            v-if="aboveMediumBP"
            class="flex items-center space-x-3 lg:space-x-5"
          >
            <MetaTagLocation v-if="location" :location="location" />
            <MetaTagVideo
              v-if="onlineLocation"
              :link="onlineLocation"
              label="components.card_search_result.view_video"
            />
            <MetaTagDate v-if="event && event.id != ''" :date="date" />
          </div>
        </div>
        <div class="flex flex-col space-y-3 md:flex-row md:space-y-0">
          <div
            v-if="!aboveMediumBP"
            class="flex items-center justify-center space-x-4"
          >
            <MetaTagLocation v-if="location" :location="location" />
            <MetaTagVideo
              v-if="onlineLocation"
              :link="onlineLocation"
              label="components.card_search_result.view_video"
            />
            <MetaTagDate v-if="event && event.id != ''" :date="date" />
          </div>
          <div
            class="flex justify-center space-x-3 md:justify-start lg:space-x-4"
          >
            <MetaTagOrganization
              v-for="(o, i) in organizations"
              :key="i"
              :organization="o"
            />
            <!-- <MetaTagMembers
              :members="members"
              label="components.card_search_result.members"
            />
            <MetaTagSupporters
              :supporters="supporters"
              label="components.card_search_result.supporters_lower"
            />
            <MetaTagStars
              :stars="stars"
              label="components.card_search_result.label"
            /> -->
          </div>
        </div>
        <!-- <div v-if="!isReduced" class="flex justify-center md:justify-start">
          <ShieldTopic v-for="(t, i) in topics" :key="i" :topic="t" />
        </div> -->
        <div v-if="organization || group">
          <p v-if="organization">@{{ organization.org_name }}</p>
          <p v-if="group">@{{ group.group_name }}</p>
        </div>
        <p
          class="line-clamp-4 justify-center md:line-clamp-4 md:justify-start md:px-0 md:py-0 lg:line-clamp-5"
        >
          {{ description }}
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import useBreakpoint from "~/composables/useBreakpoint";
import { useLinkURL } from "~/composables/useLinkURL";
import type { User } from "~/types/auth/user";
import type { Resource } from "~/types/content/resource";
import type { Group } from "~/types/entities/group";
import type { Organization } from "~/types/entities/organization";
import type { Event } from "~/types/events/event";
import { IconMap } from "~/types/icon-map";

const props = defineProps<{
  organization?: Organization;
  group?: Group;
  event?: Event;
  resource?: Resource;
  user?: User;
  isReduced?: boolean;
  isPrivate?: boolean;
}>();

const aboveMediumBP = useBreakpoint("md");

const i18n = useI18n();
const localePath = useLocalePath();
const { linkURL } = useLinkURL(props);

const ariaLabel = computed<string>(() => {
  if (props.organization) {
    return i18n.t(
      "components.card_search_result.navigate_to_organization_aria_label"
    );
  } else if (props.group) {
    return i18n.t("components._global.navigate_to_group_aria_label");
  } else if (props.event) {
    return i18n.t("components.card_search_result.navigate_to_event_aria_label");
  } else if (props.resource) {
    return i18n.t(
      "components.card_search_result.navigate_to_resource_aria_label"
    );
  } else if (props.user) {
    return i18n.t("components.card_search_result.navigate_to_user_aria_label");
  } else {
    return "";
  }
});

const date = computed<string>(() => {
  if (props.event) {
    return props.event.startTime;
  } else if (props.resource && props.resource.creationDate) {
    return props.resource.creationDate;
  } else {
    return "";
  }
});

const description = computed<string>(() => {
  if (props.organization && props.organization.description) {
    return props.organization.description;
  } else if (props.group && props.group.description) {
    return props.group.description;
  } else if (props.event && props.event.description) {
    return props.event.description;
  } else if (props.resource && props.resource.description) {
    return props.resource.description;
  } else if (props.user && props.user.description) {
    return props.user.description;
  } else {
    return "";
  }
});

const eventType = computed<"action" | "learn">(() => {
  if (props.event) {
    return props.event.type;
  } else {
    return "learn";
  }
});

const imageURL = computed<string>(() => {
  if (props.organization && props.organization.iconURL) {
    return props.organization.iconURL;
  } else if (props.group && props.group.organization.iconURL) {
    return props.group.organization.iconURL;
  } else if (props.event && props.event.iconURL) {
    return props.event.iconURL;
  } else if (props.user && props.user.iconURL) {
    return props.user.iconURL;
  } else {
    return "";
  }
});

const location = computed<string>(() => {
  if (props.organization) {
    return props.organization.location;
  } else if (props.group) {
    return props.group.location;
  } else if (props.event && props.event.offlineLocation) {
    return props.event.offlineLocation;
  } else if (props.resource && props.resource.location) {
    return props.resource.location;
  } else if (props.user && props.user.location) {
    return props.user.location;
  } else {
    return "";
  }
});

// const members = computed<number>(() => {
//   if (props.organization && props.organization.members) {
//     return props.organization.members.length;
//   } else if (props.group && props.group.members) {
//     return props.group.members.length;
//   } else {
//     return 0;
//   }
// });

const name = computed<string>(() => {
  if (props.organization) {
    return props.organization.name;
  } else if (props.group) {
    return props.group.name;
  } else if (props.event) {
    return props.event.name;
  } else if (props.resource) {
    return props.resource.name;
  } else if (props.user) {
    return props.user.name;
  } else {
    return "";
  }
});

const onlineLocation = computed<string>(() => {
  if (props.event && props.event.onlineLocationLink) {
    return props.event.onlineLocationLink;
  } else {
    return "";
  }
});

const organizations = computed<Organization[]>(() => {
  if (props.group) {
    return [props.group.organization];
  } else if (props.event) {
    return props.event.organizations;
  } else if (props.resource) {
    return [props.resource.organization];
  } else {
    return [];
  }
});

// const stars = computed<number>(() => {
//   if (props.resource && props.resource.starers) {
//     return props.resource.starers.length;
//   } else {
//     return "";
//   }
// });

// const supporters = computed<number>(() => {
//   if (props.organization && props.organization.supporters) {
//     return props.organization.supporters.length;
//   } else if (props.group && props.group.supporters) {
//     return props.group.supporters.length;
//   } else if (props.event && props.event.supporters) {
//     return props.event.supporters.length;
//   } else if (props.user && props.user.supporters) {
//     return props.user.supporters.length;
//   } else {
//     return 0;
//   }
// });

// const topics = computed<string[]>(() => {
//   if (props.organization) {
//     return props.organization.topics;
//   } else if (props.group) {
//     return props.group.topics;
//   } else if (props.event) {
//     return props.event.topics;
//   } else if (props.resource && props.resource.topics) {
//     return props.resource.topics;
//   } else if (props.user && props.user.topics) {
//     return props.user.topics;
//   } else {
//     return [""];
//   }
// });
</script>
