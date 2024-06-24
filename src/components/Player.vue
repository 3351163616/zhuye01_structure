<template>
  <APlayer v-if="playList.length > 0" ref="player" :audio="playList" :autoplay="store.playerAutoplay" :theme="theme"
    :autoSwitch="false" :loop="store.playerLoop" :order="store.playerOrder" :volume="volume" :showLrc="true"
    :listFolded="listFolded" :listMaxHeight="listMaxHeight" :noticeSwitch="false" @play="onPlay" @pause="onPause"
    @timeupdate="onTimeUp" @error="loadMusicError" />
</template>

<script setup>
import { MusicOne, PlayWrong } from "@icon-park/vue-next";
import { getPlayerList } from "@/api";
import { mainStore } from "@/store";
import APlayer from "@worstone/vue-aplayer";
import { ref, computed, onMounted, nextTick, defineProps, h } from 'vue';
import { ElMessage } from 'element-plus';

const store = mainStore();

// 获取播放器 DOM
const player = ref(null);

// 歌曲播放列表
const playList = ref([]);

// 歌曲播放项
const playIndex = ref(0);

// 配置项
const props = defineProps({
  theme: {
    type: String,
    default: "#efefef",
  },
  volume: {
    type: Number,
    default: 0.7,
    validator: (value) => {
      return value >= 0 && value <= 1;
    },
  },
  songServer: {
    type: String,
    default: "netease",
  },
  songType: {
    type: String,
    default: "playlist",
  },
  songId: {
    type: String,
    default: "7452421335",
  },
  listFolded: {
    type: Boolean,
    default: false,
  },
  listMaxHeight: {
    type: Number,
    default: 420,
  },
});

const listHeight = computed(() => {
  return props.listMaxHeight + "px";
});

// 初始化播放器
onMounted(async () => {
  nextTick(async () => {
    try {
      const [neteaseList, tencentList] = await Promise.all([
        getPlayerList(props.songServer, props.songType, props.songId).catch(err => {
          console.error('网易云音乐加载失败', err);
          return [];
        }),
        getPlayerList(import.meta.env.VITE_SONG_SERVER_2, props.songType, import.meta.env.VITE_SONG_ID_2).catch(err => {
          console.error('腾讯音乐加载失败', err);
          return [];
        })
      ]);

      const localMusic = [
        {
          name: "Catch My Breath",
          artist: "Kelly Clarkson",
          url: "music/Catch My Breath.mp3",
          lrc: "music/Catch My Breath.lrc",
        }, {
          name: "Call of Silence",
          artist: "Clear Sky",
          url: "music/Call of Silence (.Clear Sky Remix).mp3",
          lrc: "music/Call of Silence (.Clear Sky Remix).lrc",
        }, {
          name: "クリームソーダとシャンデリア",
          artist: "Akie秋绘、早凉",
          url: "music/クリームソーダとシャンデリア.mp3",
          lrc: "music/クリームソーダとシャンデリア.lrc",
        }, {
          name: "Love Story",
          artist: "Taylor Swift",
          url: "music/Love Story.mp3",
          lrc: "music/Love Story.lrc",
        }, {
          name: "心做し",
          artist: "双笙",
          url: "music/心做し.mp3",
          lrc: "music/心做し.lrc",
        },
        {
          name: "春娇与志明",
          artist: "街道办GDC、欧阳耀莹",
          url: "music/春娇与志明.mp3",
          lrc: "music/春娇与志明.lrc",
        },
        {
          name: "The truth that you leave",
          artist: "Pianoboy高至豪",
          url: "music/The truth that you leave.mp3",
          lrc: "music/The truth that 你 leave.lrc",
        },
        {
          name: "凄美地",
          artist: "郭顶",
          url: "music/凄美地.mp3",
          lrc: "music/凄美地.lrc",
        },
        {
          name: "安和桥",
          artist: "宋冬野",
          url: "music/安和桥.mp3",
          lrc: "music/安和桥.lrc",
        },
        {
          name: "悬溺",
          artist: "葛东琪",
          url: "music/悬溺.mp3",
          lrc: "music/悬溺.lrc",
        }, {
          name: "悪魔の子",
          artist: "ヒグチアイ",
          url: "music/悪魔の子.mp3",
          lrc: "music/悪魔の子.lrc",
        },
        {
          name: "白月光与朱砂痣",
          artist: "大籽",
          url: "music/白月光与朱砂痣.mp3",
          lrc: "music/白月光与朱砂痣.lrc",
        }, {
          name: "偏爱",
          artist: "张芸京",
          url: "music/偏爱.mp3",
          lrc: "music/偏爱.lrc",
        },
        {
          name: "我用什么把你留住",
          artist: "福禄寿FloruitShow",
          url: "music/我用什么把你留住.mp3",
          lrc: "music/我用什么把你留住.lrc",
        },
        {
          name: "精卫",
          artist: "30年前，50年后",
          url: "music/精卫.mp3",
          lrc: "music/精卫.lrc",
        }, {
          name: "Heaven",
          artist: "Ailee",
          url: "music/Heaven.mp3",
          lrc: "music/Heaven.lrc",
        },

        {
          name: "收敛",
          artist: "不够",
          url: "music/收敛.mp3",
          lrc: "music/收敛.lrc",
        },
        {
          name: "我会等",
          artist: "承桓",
          url: "music/我会等.mp3",
          lrc: "music/我会等.lrc",
        },
        {
          name: "在你的身边",
          artist: "盛哲",
          url: "music/在你的身边.mp3",
          lrc: "music/在你的身边.lrc",
        }, {
          name: "起风了",
          artist: "周深",
          url: "music/起风了.mp3",
          lrc: "music/起风了.lrc",
        },
        {
          name: "我是如此相信",
          artist: "周杰伦",
          url: "music/我是如此相信.mp3",
          lrc: "music/我是如此相信.lrc",
        },

        {
          name: "Everywhere We Go",
          artist: "陈冠希、MC仁、厨房仔、应采儿",
          url: "music/Everywhere We Go.mp3",
          lrc: "music/Everywhere We Go.lrc",
        },
        {
          name: "Shadow Of The Sun (Adventure Club Remix)",
          artist: "Max Elto、Adventure Club",
          url: "music/Shadow Of The Sun (Adventure Club Remix).mp3",
          lrc: "music/Shadow Of The Sun (Adventure Club Remix).lrc",
        },
        {
          name: "Take Me Hand",
          artist: "DAISHI DANCE、Cécile Corbel",
          url: "music/Take Me Hand.mp3",
          lrc: "music/Take Me Hand.lrc",
        }, {
          name: "Stars Align",
          artist: "R3HAB、蔡依林",
          url: "music/Stars Align.mp3",
          lrc: "music/Stars Align.lrc",
        }, {
          name: "Wake (Studio)",
          artist: "Hillsong Young & Free",
          url: "music/Wake (Studio).mp3",
          lrc: "music/Wake (Studio).lrc",
        }, {
          name: "Sunburst",
          artist: "Tobu、Itro",
          url: "music/Sunburst.mp3",
          lrc: "music/Sunburst.lrc",
        },
      ];

      store.musicIsOk = true;
      playList.value = [...localMusic, ...neteaseList, ...tencentList];

      if (playList.value.length === 0) {
        ElMessage({
          message: "没有可播放的音乐",
          grouping: true,
          icon: h(PlayWrong, {
            theme: "filled",
            fill: "#efefef",
          }),
        });
      } else {
        console.log("音乐加载完成");
        console.log(playList.value);
        console.log(playIndex.value, playList.value.length, props.volume);
      }
    } catch (err) {
      console.error(err);
      store.musicIsOk = false;
      ElMessage({
        message: "播放器加载失败",
        grouping: true,
        icon: h(PlayWrong, {
          theme: "filled",
          fill: "#efefef",
        }),
      });
    }
  });
});

const onPlay = () => {
  console.log("播放");
  playIndex.value = player.value.aplayer.index;
  store.setPlayerState(player.value.audioRef.paused);
  store.setPlayerData(playList.value[playIndex.value].name, playList.value[playIndex.value].artist);
  ElMessage({
    message: store.getPlayerData.name + " - " + store.getPlayerData.artist,
    grouping: true,
    icon: h(MusicOne, {
      theme: "filled",
      fill: "#efefef",
    }),
  });
};

const onPause = () => {
  store.setPlayerState(player.value.audioRef.paused);
};

const onTimeUp = () => {
  let lyrics = player.value.aplayer.lyrics[playIndex.value];
  let lyricIndex = player.value.aplayer.lyricIndex;
  if (!lyrics || !lyrics[lyricIndex]) {
    return;
  }
  let lrc = lyrics[lyricIndex][1];
  if (lrc === "Loading") {
    lrc = "歌词加载中";
  } else if (lrc === "Not available") {
    lrc = "歌词加载失败";
  }
  store.setPlayerLrc(lrc);
};

const playToggle = () => {
  player.value.toggle();
};

const changeVolume = (value) => {
  player.value.setVolume(value, false);
};

const changeSong = (type) => {
  type === 0 ? player.value.skipBack() : player.value.skipForward();
  nextTick(() => {
    player.value.play();
  });
};

const toggleList = () => {
  player.value.toggleList();
};

const loadMusicError = () => {
  let notice = "";
  if (playList.value.length > 1) {
    notice = "播放歌曲出现错误，播放器将在 2s 后进行下一首";
  } else {
    notice = "播放歌曲出现错误";
  }
  ElMessage({
    message: notice,
    grouping: true,
    icon: h(PlayWrong, {
      theme: "filled",
      fill: "#EFEFEF",
      duration: 2000,
    }),
  });
  console.error(
    "播放歌曲: " + player.value.aplayer.audio[player.value.aplayer.index].name + " 出现错误",
  );
};

defineExpose({ playToggle, changeVolume, changeSong, toggleList });
</script>

<style lang="scss" scoped>
.aplayer {
  width: 80%;
  border-radius: 6px;
  font-family: "HarmonyOS_Regular", sans-serif !important;

  :deep(.aplayer-body) {
    background-color: transparent;

    .aplayer-pic {
      display: none;
    }

    .aplayer-info {
      margin-left: 0;
      background-color: #ffffff40;
      border-color: transparent !important;

      .aplayer-music {
        flex-grow: initial;
        margin-bottom: 2px;
        overflow: initial;

        .aplayer-title {
          font-size: 16px;
          margin-right: 6px;
        }

        .aplayer-author {
          color: #efefef;
        }
      }

      .aplayer-lrc {
        text-align: left;
        margin: 7px 0 6px 6px;
        height: 44px;
        mask: linear-gradient(#fff 15%,
            #fff 85%,
            hsla(0deg, 0%, 100%, 0.6) 90%,
            hsla(0deg, 0%, 100%, 0));
        -webkit-mask: linear-gradient(#fff 15%,
            #fff 85%,
            hsla(0deg, 0%, 100%, 0.6) 90%,
            hsla(0deg, 0%, 100%, 0));

        &::before,
        &::after {
          display: none;
        }

        p {
          color: #efefef;
        }

        .aplayer-lrc-current {
          font-size: 0.95rem;
          margin-bottom: 4px !important;
        }
      }

      .aplayer-controller {
        display: none;
      }
    }
  }

  :deep(.aplayer-list) {
    margin-top: 6px;
    height: v-bind(listHeight);
    background-color: transparent;

    ol {
      &::-webkit-scrollbar-track {
        background-color: transparent;
      }

      li {
        border-color: transparent;

        &.aplayer-list-light {
          background: #ffffff40;
          border-radius: 6px;
        }

        &:hover {
          background: #ffffff26 !important;
          border-radius: 6px !important;
        }

        .aplayer-list-index,
        .aplayer-list-author {
          color: #efefef;
        }
      }
    }
  }
}
</style>
