<template>
  <div v-if="siteLinks[0]" class="links">
    <div class="line">
      <Icon size="20">
        <Link />
      </Icon>
      <span class="title">我的列表</span>
      <form class="search-box" id="searchForm" method="GET">
        <select id="searchEngine" name="engine">
          <option value="https://www.google.com/search">Google</option>
          <option value="https://www.bing.com/search">Bing</option>
          <option value="https://search.yahoo.com/search">Yahoo</option>
        </select>
        <input type="text" name="q" placeholder="搜索">
        <input type="submit" value="Search">
      </form>
    </div>
    <!-- 网站列表 -->

    <Swiper
      v-if="siteLinks[0]"
      :modules="[Pagination, Mousewheel]"
      :slides-per-view="1"
      :space-between="40"
      :pagination="{
        el: '.swiper-pagination',
        clickable: true,
        bulletElement: 'div',
      }"
      :mousewheel="true"
    >
      <SwiperSlide v-for="site in siteLinksList" :key="site">
        <el-row class="link-all" :gutter="20">
          <el-col v-for="(item, index) in site" :span="8" :key="item">
            <div
              class="item cards"
              :style="index < 3 ? 'margin-bottom: 20px' : null"
              @click="jumpLink(item)"
            >
              <Icon size="26">
                <component :is="siteIcon[item.icon]" />
              </Icon>
              <span class="name text-hidden">{{ item.name }}</span>
            </div>
          </el-col>
        </el-row>
      </SwiperSlide>
      <div class="swiper-pagination" />
    </Swiper>
  </div>
</template>

<script setup>
import { Icon } from "@vicons/utils";
// 可前往 https://www.xicons.org 自行挑选并在此处引入
import { Link, Blog, CompactDisc, Cloud, Compass, Book, Fire, LaptopCode, FileDownload, Cubes } from "@vicons/fa"; // 注意使用正确的类别
import { GTranslateFilled } from "@vicons/material"; // 导入翻译图标
import { mainStore } from "@/store";
import { Swiper, SwiperSlide } from "swiper/vue";
import { Pagination, Mousewheel } from "swiper";
import siteLinks from "@/assets/siteLinks.json";


const store = mainStore();

// 计算网站链接
const siteLinksList = computed(() => {
  const result = [];
  for (let i = 0; i < siteLinks.length; i += 6) {
    const subArr = siteLinks.slice(i, i + 6);
    result.push(subArr);
  }
  return result;
});

// 网站链接图标
const siteIcon = {
  Blog,
  Cloud,
  CompactDisc,
  Compass,
  Book,
  Fire,
  LaptopCode,
  FileDownload,
  Cubes,
  Link,
  GTranslateFilled,
};

// 链接跳转
const jumpLink = (data) => {
  if (data.name === "音乐" && store.musicClick) {
    if (typeof $openList === "function") $openList();
  } else {
    window.open(data.link, "_blank");
  }
};
onMounted(() => {
  console.log(siteLinks);
});

</script>

<style lang="scss" scoped>
.links {
  .line {
    margin: 2rem 0.25rem 1rem;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    animation: fade 0.5s;
    .title {
      margin-left: 8px;
      font-size: 1.15rem;
      text-shadow: 0 0 5px #00000050;
    }
  }
  .swiper {
    left: -10px;
    width: calc(100% + 20px);
    padding: 5px 10px 0;
    z-index: 0;
    .swiper-slide {
      height: 100%;
    }
    .swiper-pagination {
      position: static;
      margin-top: 4px;
      :deep(.swiper-pagination-bullet) {
        background-color: #fff;
        width: 18px;
        height: 4px;
        border-radius: 4px;
        transition: opacity 0.3s;
        &:hover {
          opacity: 1;
        }
      }
    }
  }
  .link-all {
    height: 220px;
    .item {
      height: 100px;
      width: 100%;
      display: flex;
      align-items: center;
      flex-direction: row;
      justify-content: center;
      padding: 0 10px;
      animation: fade 0.5s;

      &:hover {
        transform: scale(1.02);
        background: rgb(0 0 0 / 40%);
        transition: 0.3s;
      }

      &:active {
        transform: scale(1);
      }

      .name {
        font-size: 1.1rem;
        margin-left: 8px;
      }
      @media (min-width: 720px) and (max-width: 820px) {
        .name {
          display: none;
        }
      }
      @media (max-width: 720px) {
        height: 80px;
      }
      @media (max-width: 460px) {
        flex-direction: column;
        .name {
          font-size: 1rem;
          margin-left: 0;
          margin-top: 8px;
        }
      }
    }
    @media (max-width: 720px) {
      height: 180px;
    }
  }
}

//搜索框部分

.search-box {
    width: 100%;
    max-width: 450px;
    margin: 10px;
    position: relative;
    display: flex;
    animation: fade 0.5s;  /* 添加动画效果 */
}

.search-box select {
    padding: 15px;
    font-size: 18px;
    border: none;
    border-radius: 4px 0 0 4px;
    background: rgba(0, 0, 0, 0.4);  /* 毛玻璃效果 */
    backdrop-filter: blur(10px);  /* 毛玻璃效果 */
    -webkit-backdrop-filter: blur(10px);  /* 兼容Safari */
    color: #fff;
    outline: none;  /* 移除选择框聚焦时的默认边框 */
    appearance: none;  /* 移除默认的下拉箭头样式 */
    transition: transform 0.3s, background 0.3s;  /* 添加过渡效果 */
    text-align: center;  /* 水平居中 */
    line-height: 1.5;  /* 调整行高以实现垂直居中 */
}

.search-box select:hover {
    transform: scale(1.02);  /* 鼠标悬停时的缩放效果 */
    background: rgba(0, 0, 0, 0.5);  /* 鼠标悬停时背景色加深 */
}

.search-box input[type="text"] {
    width: calc(100% - 100px);
    padding: 10px;
    font-size: 16px;
    border: none;  /* 移除白边 */
    border-radius: 0 4px 4px 0;
    background: rgba(0, 0, 0, 0.4);  /* 毛玻璃效果 */
    backdrop-filter: blur(10px);  /* 毛玻璃效果 */
    -webkit-backdrop-filter: blur(10px);  /* 兼容Safari */
    color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    outline: none;  /* 移除输入框聚焦时的默认边框 */
    transition: transform 0.3s, background 0.3s;  /* 添加过渡效果 */
}

.search-box input[type="text"]::placeholder {
    color: #fff;  /* 设置常显文字的颜色 */
}

.search-box input[type="text"]:hover {
    transform: scale(1.02);  /* 鼠标悬停时的缩放效果 */
    background: rgba(0, 0, 0, 0.5);  /* 鼠标悬停时背景色加深 */
}

.search-box input[type="text"]:active {
    transform: scale(1);  /* 鼠标点击时恢复原始大小 */
}

.search-box input[type="submit"] {
    display: none;
}

@keyframes fade {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

</style>

<script>
export default {
  mounted() {
    document.getElementById('searchForm').addEventListener('submit', function(event) {
      event.preventDefault();
      var engine = document.getElementById('searchEngine').value;
      var query = document.querySelector('input[name="q"]').value;
      var url = engine + '?q=' + encodeURIComponent(query);
      window.location.href = url;
    });
  }
}
</script>