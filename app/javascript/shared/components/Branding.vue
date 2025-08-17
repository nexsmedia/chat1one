<script>
import { useBranding } from 'shared/composables/useBranding';

const {
  LOGO_THUMBNAIL: logoThumbnail,
  BRAND_NAME: brandName,
  WIDGET_BRAND_URL: widgetBrandURL,
} = window.globalConfig || {};

export default {
  props: {
    disableBranding: {
      type: Boolean,
      default: false,
    },
  },
  setup() {
    const { replaceInstallationName } = useBranding();
    return {
      replaceInstallationName,
    };
  },
  data() {
    return {
      globalConfig: {
        brandName,
        logoThumbnail,
        widgetBrandURL,
      },
    };
  },
  computed: {
    brandRedirectURL() {
      try {
        const referrerHost = this.$store.getters['appConfig/getReferrerHost'];
        const baseURL = `${this.globalConfig.widgetBrandURL}?utm_source=${
          referrerHost ? 'widget_branding' : 'survey_branding'
        }`;
        if (referrerHost) {
          return `${baseURL}&utm_referrer=${referrerHost}`;
        }
        return baseURL;
      } catch (e) {
        // Suppressing the error as getter is not defined in some cases
      }
      return '';
    },
  },
};
</script>

<template>
  <div
    v-if="isPoweredByChatwoot"
    class="flex items-center justify-center w-full py-2 text-xs"
  >
    </div>
</template>
