<template>
  <footer id="footer" :class="store.footerBlur ? 'blur' : null">
    <Transition name="fade" mode="out-in">
      <div v-if="!store.playerState || !store.playerLrcShow" class="power">
        <span>
          Copyright&nbsp;&copy;
          <span v-if="siteStartDate?.length >= 4" class="site-start">
            {{ siteStartDate.substring(0, 4) }}
            -
          </span>
          {{ fullYear }}
          <a :href="siteUrl">{{ siteAnthor }}</a>
        </span>
        <!-- 以下信息请不要修改哦 -->
        <span class="hidden">
          &amp;&nbsp;Made&nbsp;by
          <a :href="config.github" target="_blank">
            {{ config.author }}
          </a>
        </span>
        <!-- 站点备案 -->
        <a v-if="showIcp && siteIcp" :href="siteIcpUrl" target="_blank">
          &amp;
          {{ siteIcp }}
        </a>
        <a v-if="showMoeIcp && siteMoeIcp" :href="siteMoeIcpUrl" target="_blank">
          &amp;
          {{ siteMoeIcp }}
        </a>
      </div>
      <div v-else class="lrc">
        <Transition name="fade" mode="out-in">
          <div class="lrc-all" :key="store.getPlayerLrc">
            <music-one theme="filled" size="18" fill="#efefef" />
            <span class="lrc-text text-hidden" v-html="store.getPlayerLrc" />
            <music-one theme="filled" size="18" fill="#efefef" />
          </div>
        </Transition>
      </div>
    </Transition>
  </footer>
</template>

<script setup>
import { MusicOne } from "@icon-park/vue-next";
import { mainStore } from "@/store";
import config from "@/../package.json";

const store = mainStore();
const fullYear = new Date().getFullYear();

// 加载配置数据
const siteStartDate = ref(import.meta.env.VITE_SITE_START);
const siteIcp = ref(import.meta.env.VITE_SITE_ICP);
const siteMoeIcp = ref(import.meta.env.VITE_SITE_MOE_ICP);
const siteIcpUrl = ref(import.meta.env.VITE_SITE_ICP_URL);
const siteMoeIcpUrl = ref(import.meta.env.VITE_SITE_MOE_ICP_URL);
const siteAnthor = ref(import.meta.env.VITE_SITE_FOOTER_ANTHOR);
const siteIcpDomain = import.meta.env.VITE_SITE_ICP_DOMAIN;
const siteMoeIcpDomain = import.meta.env.VITE_SITE_MOE_ICP_DOMAIN;
const host = window.location.hostname;
const showIcp = computed(() => siteIcpDomain && host.endsWith(siteIcpDomain));
const showMoeIcp = computed(() => siteMoeIcpDomain && host.endsWith(siteMoeIcpDomain));
const siteUrl = computed(() => {
  const url = import.meta.env.VITE_SITE_URL;
  if (!url) return "https://ooo.ozo.ooo";
  // 判断协议前缀
  if (!url.startsWith("http://") && !url.startsWith("https://")) {
    return "//" + url;
  }
  return url;
});
</script>

<style lang="scss" scoped>
#footer {
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  height: 46px;
  line-height: 46px;
  text-align: center;
  z-index: 0;
  font-size: 14px;
  .power {
    animation: fade 0.3s;
  }
  .lrc {
    padding: 0 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    .lrc-all {
      width: 98%;
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      .lrc-text {
        margin: 0 8px;
      }
      .i-icon {
        width: 18px;
        height: 18px;
        display: inherit;
      }
    }
  }
  &.blur {
    backdrop-filter: blur(10px);
    background: rgb(0 0 0 / 25%);
    font-size: 16px;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.15s ease-in-out;
  }
  @media (max-width: 720px) {
    font-size: 0.85rem;
    &.blur {
      font-size: 0.85rem;
    }
  }
  @media (max-width: 480px) {
    .hidden {
      display: none;
    }
  }
}
</style>
