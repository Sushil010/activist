<template>
  <Toaster :theme="$colorMode.value === 'dark' ? 'dark' : 'light'" />
  <ModalBase :modalName="modalName">
    <div class="px-2 pb-2 pt-1 lg:px-4 lg:pb-4 lg:pt-2">
      <DialogTitle class="font-display">
        <p class="responsive-h2 font-bold">
          {{ $t("components.modal_share_page.header") }}
        </p>
      </DialogTitle>
      <div class="pt-6">
        <p class="responsive-h4 font-bold">
          {{ $t("components.modal_share_page.online") }}
        </p>
        <div
          class="grid w-full grid-cols-3 grid-rows-2 content-start gap-4 pt-4 lg:gap-8 lg:pt-6"
        >
          <s-telegram
            @popup-close="nativeBehaviorOptions.onClose"
            @popup-open="nativeBehaviorOptions.onOpen"
            @popup-block="nativeBehaviorOptions.onBlock"
            @popup-focus="nativeBehaviorOptions.onFocus"
            class="focus-brand"
            :window-features="windowFeatures"
            :share-options="shareOptions"
            :use-native-behavior="useNativeBehavior"
            :native-behavior-options="nativeBehaviorOptions"
          >
            <MetaTagSocialMedia
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.TELEGRAM"
              :text="$t('components.modal_share_page.telegram')"
              iconSize="1.5em"
            />
          </s-telegram>
          <s-mastodon
            @popup-close="nativeBehaviorOptions.onClose"
            @popup-open="nativeBehaviorOptions.onOpen"
            @popup-block="nativeBehaviorOptions.onBlock"
            @popup-focus="nativeBehaviorOptions.onFocus"
            class="focus-brand"
            :window-features="windowFeatures"
            :share-options="shareOptions"
            :use-native-behavior="useNativeBehavior"
          >
            <MetaTagSocialMedia
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.MASTODON"
              :text="$t('components.modal_share_page.mastodon')"
              iconSize="1.5em"
            />
          </s-mastodon>
          <s-twitter
            @popup-close="nativeBehaviorOptions.onClose"
            @popup-open="nativeBehaviorOptions.onOpen"
            @popup-block="nativeBehaviorOptions.onBlock"
            @popup-focus="nativeBehaviorOptions.onFocus"
            class="focus-brand"
            :window-features="windowFeatures"
            :share-options="shareOptions"
            :use-native-behavior="useNativeBehavior"
          >
            <MetaTagSocialMedia
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.TWITTER"
              text="@activist_org"
              iconSize="1.5em"
            />
          </s-twitter>
          <s-email class="focus-brand" :share-options="shareOptions">
            <MetaTagSocialMedia
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.ENVELOPE"
              text="Email"
              iconSize="1.5em"
            />
          </s-email>
          <s-facebook
            @popup-close="nativeBehaviorOptions.onClose"
            @popup-open="nativeBehaviorOptions.onOpen"
            @popup-block="nativeBehaviorOptions.onBlock"
            @popup-focus="nativeBehaviorOptions.onFocus"
            class="focus-brand"
            :window-features="windowFeatures"
            :share-options="shareOptions"
            :use-native-behavior="useNativeBehavior"
          >
            <MetaTagSocialMedia
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.FACEBOOK"
              :text="$t('components.modal_share_page.facebook')"
              iconSize="1.5em"
            />
          </s-facebook>
          <div
            @click="
              copyToClipboardThenOpenURL(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl(),
                'https://signal.me/#p'
              )
            "
            @keypress.space="
              copyToClipboardThenOpenURL(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl(),
                'https://signal.me/#p'
              )
            "
            @keypress.enter="
              copyToClipboardThenOpenURL(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl(),
                'https://signal.me/#p'
              )
            "
            class="focus-brand"
            tabindex="0"
            role="button"
          >
            <MetaTagSocialMedia
              v-if="!signalContentCopied"
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.SIGNAL"
              :text="$t('components.modal_share_page.signal')"
              iconSize="1.5em"
            />
            <MetaTagSocialMedia
              v-if="signalContentCopied"
              class="text-light-accepted-green hover:text-light-accepted-green dark:text-dark-accepted-green dark:hover:text-dark-accepted-green"
              :iconName="IconMap.SQUARE_CHECK"
              :text="$t('components.modal_share_page.url_copied')"
              iconSize="1.5em"
            />
          </div>
          <div
            @click="
              copyToClipboard(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl()
              )
            "
            @keypress.space="
              copyToClipboard(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl()
              )
            "
            @keypress.enter="
              copyToClipboard(
                props?.event?.name
                  ? props?.event?.name
                  : props?.organization?.name
                    ? props?.organization?.name
                    : '',
                getCurrentUrl()
              )
            "
            class="focus-brand"
            tabindex="0"
            role="button"
          >
            <MetaTagSocialMedia
              v-if="!contentCopied"
              class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
              :iconName="IconMap.LINK"
              :text="$t('components.modal_share_page.copy_link')"
              iconSize="1.5em"
            />
            <MetaTagSocialMedia
              v-if="contentCopied"
              class="text-light-accepted-green hover:text-light-accepted-green dark:text-dark-accepted-green dark:hover:text-dark-accepted-green"
              :iconName="IconMap.SQUARE_CHECK"
              :text="$t('components.modal_share_page.url_copied')"
              iconSize="1.5em"
            />
          </div>
        </div>
      </div>
      <div class="pt-6">
        <p class="responsive-h4 font-bold">
          {{ $t("components.modal_share_page.offline") }}
        </p>
        <div
          class="grid w-full grid-cols-3 grid-rows-1 content-start gap-4 pt-4 lg:gap-8 lg:pt-6"
        >
          <ModalQRCodeBtn
            v-if="organization"
            :organization="organization"
            type="meta-tag"
          />
          <ModalQRCodeBtn v-if="group" :group="group" type="meta-tag" />
          <ModalQRCodeBtn v-if="event" :event="event" type="meta-tag" />
          <ModalQRCodeBtn
            v-if="resource"
            :resource="resource"
            type="meta-tag"
          />
          <ModalQRCodeBtn v-if="user" :user="user" type="meta-tag" />
        </div>
      </div>
      <!-- <s-facebook-messenger
        @popup-close="onClose"
        @popup-open="onOpen"
        @popup-block="onBlock"
        @popup-focus="onFocus"
        :window-features="windowFeatures"
        :share-options="shareOptions"
        :use-native-behavior="useNativeBehavior"
      >
        <MetaTagSocialMedia
          class="dark:hover:dark-distinct-text text-light-text hover:text-light-distinct-text dark:text-dark-text"
          :iconName="IconMap.MESSENGER"
          :text="$t('components.modal_share_page.messenger')"
          iconSize="1.5em"
        />
      </s-facebook-messenger> -->
    </div>
  </ModalBase>
</template>

<script setup lang="ts">
import { SEmail, SFacebook, SMastodon, STelegram, STwitter } from "vue-socials";
import ModalBase from "~/components/modal/ModalBase.vue";
import type { User } from "~/types/auth/user";
import type { BtnAction } from "~/types/btn-props";
import type { Resource } from "~/types/content/resource";
import type { Group } from "~/types/entities/group";
import type { Organization } from "~/types/entities/organization";
import type { Event } from "~/types/events/event";
import { IconMap } from "~/types/icon-map";
import { toast, Toaster } from "vue-sonner";
import { useI18n } from "vue-i18n";

const props = defineProps<{
  cta: BtnAction["cta"];
  organization?: Organization;
  group?: Group;
  event?: Event;
  resource?: Resource;
  user?: User;
}>();

const { t } = useI18n();
const modalName = "ModalSharePage";

const getEntityType = () => {
  if (props.organization) {
    return setEntityInfo(props.organization);
  } else if (props.group) {
    return setEntityInfo(props.group);
  } else if (props.event) {
    return setEntityInfo(props.event);
  }
};

const setEntityInfo = (
  data: typeof props.organization | typeof props.group | typeof props.event
) => {
  if (!data) return;
  return {
    subject: `Share ${data.name}!`,
    body: `Check out ${data.name}!`,
    url: getCurrentUrl(),
    text: `Check out ${data.name}!`,
  };
};

// Function to grab the url to the base id of the entity to share.
const getCurrentUrl = () => {
  if (props.organization) {
    return `${BASE_FRONTEND_URL}/organizations/${props.organization.id}`;
  } else if (props.group) {
    return `${BASE_FRONTEND_URL}/organizations/${props.group.organization.id}/groups/${props.group.id}`;
  } else if (props.event) {
    return `${BASE_FRONTEND_URL}/events/${props.event.id}`;
  } else if (props.resource) {
    return props.resource.resourceURL;
  } else if (props.user) {
    return `${BASE_FRONTEND_URL}/users/${props.user.id}`;
  }
  const url = window.location.href;
  return url.substring(0, url.lastIndexOf("/"));
};

const shareOptions = {
  url: getCurrentUrl() || `${BASE_FRONTEND_URL}`,
  text: getEntityType()?.text || "Check this out!",
  quote: getEntityType()?.text || "Check this out!",
  hashtags: ["activism", "organizing"],
  hashtag: "#activism, #organizing",
  via: "activist_org",
  mail: "",
  cc: [""],
  bcc: [""],
  subject: getEntityType()?.subject || "Share this!",
  body:
    `${getEntityType()?.body}   ${getEntityType()?.url}` || "Check this out!",
  redirectUri: "https://www.domain.com/",
  domain: "https://mas.to",
  // appId: 842766727626496, Facebook Messenger App ID
};

const useNativeBehavior = false;
const contentCopied = ref(false);
const signalContentCopied = ref(false);

// No specific actions should be taken on these events, but we can customize the behavior if needed.
const nativeBehaviorOptions = {
  onClose: () => {},
  onOpen: () => {},
  onBlock: () => {},
  onFocus: () => {},
};

const windowFeatures = {};

const copyToClipboard = async (name: string, url: string) => {
  try {
    await navigator.clipboard.writeText(url);
    contentCopied.value = true;
    setTimeout(() => {
      contentCopied.value = false;
    }, 2000);
  } catch (error) {
    console.error(`Could not copy text: ${error}`);
    contentCopied.value = false;
  }
};

const copyToClipboardThenOpenURL = async (
  name: string,
  url: string,
  redirectURL?: string
) => {
  try {
    await navigator.clipboard.writeText(url);
    signalContentCopied.value = true;
    toast(t("components.modal_share_page.opening_signal"));
    setTimeout(() => {
      signalContentCopied.value = false;
      if (redirectURL) {
        window.open(redirectURL, "_blank");
      }
    }, 2000);
  } catch (error) {
    console.error(`Could not copy text: ${error}`);
    signalContentCopied.value = false;
  }
};
</script>
