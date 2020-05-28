<template>
  <div class="main-contain">
    <div class="custom-space"></div>
    <div class="tip">
      {{ name }}
    </div>
    <div class="serarch-contain">
      <div class="search-keys">
        <div class="item" v-for="(o, index) in items" v-bind:key="index">
          <div class="name-label">
            <label class="name">{{ o.key }}</label>
          </div>
          <input class="content" v-model="serarchKeys[o.key]" :placeholder="o.desc" />
        </div>
      </div>
      <div class="tool-bar">
        <div @click="clearKeys">清空</div>
        <div @click="search">搜索</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Helper',
  components: {},
  data() {
    return {
      name: 'github搜索助手',
      items: [],
      serarchKeys: {},
    };
  },
  created: function() {
    const keys = [
      {
        key: 'name',
        desc: '名称',
      },
      {
        key: 'stars',
        desc: '>星数',
      },
      {
        key: 'language',
        desc: '语言',
      },
      {
        key: 'user',
        desc: '作者',
      },
      {
        key: 'pushed',
        desc: '>提交日期YYYY-MM-DD',
      },
      {
        key: 'created',
        desc: '>创建日期YYYY-MM-DD',
      },
      {
        key: 'fork',
        desc: '>fork数',
      },
      {
        key: 'readme',
        desc: 'readme文件',
      },
      {
        key: 'size',
        desc: '>项目大小kb',
      },
      {
        key: 'license',
        desc: '开源条款',
      },
    ];

    this.items = keys;
    const m = {};
    keys.forEach(el => {
      m[el.key] = null;
    });
    this.serarchKeys = m;

    this.addStatistics();

    this.hideStatistics();
  },
  methods: {
    genQuery: function() {
      const query = [];
      Object.keys(this.serarchKeys).forEach(el => {
        const val = this.serarchKeys[el];
        if (!val) return;
        if (['name', 'readme'].includes(el)) {
          query.push(`in:${el} ${val}`);
        }
        switch (el) {
          case 'stars':
            query.push(`stars:>${val}`);
            break;
          case 'fork':
            query.push(`fork:>${val}`);
            break;
          case 'language':
            query.push(`language:${val}`);
            break;
          case 'user':
            query.push(`user:${val}`);
            break;
          case 'size':
            query.push(`size:>${val}`);
            break;
          default:
            break;
        }
      });

      return {
        query: encodeURIComponent(query.join(' ')),
        content: query.join(' '),
      };
    },
    search: function() {
      const { query, content } = this.genQuery();
      // eslint-disable-next-line no-undef
      // _czc.push(['_trackEvent', '插件搜索', `${content}`]);
      const href = `https://github.com/search?q=${query}`;
      window.open(href, '_blank');
    },
    clearKeys: function() {
      Object.keys(this.serarchKeys).forEach(key => {
        this.serarchKeys[key] = null;
      });
    },
    addStatistics: function() {
      // document.write(
      //   unescape(
      //     "%3Cspan id='cnzz_stat_icon_1278856908'%3E%3C/span%3E%3Cscript src='../../' type='text/javascript'%3E%3C/script%3E"
      //   )
      // );
      // var _czc = _czc || []
      // 绑定siteid，请用您的siteid替换下方"XXXXXXXX"部分
      // eslint-disable-next-line no-undef
      // _czc.push(['_setAccount', '1278856908']);
    },
    hideStatistics: function() {
      // const s = document.getElementById('cnzz_stat_icon_1278856908');
      // s.style.display = 'none';
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.main-contain {
  text-align: center;
}
.custom-space {
  height: 20px;
}
.tip {
  font-size: 22px;
  font-weight: 400;
}

.serarch-contain {
  width: 40%;
  min-width: 700px;
  max-width: 800px;
  margin: 0 auto;
  margin-top: 20px;
}
.search-keys {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.item {
  margin: 10px 0;
  font-size: 18px;
  display: flex;
}
.name-label {
  width: 100px;
  display: inline-flex;
  justify-content: flex-end;
  align-content: center;
  align-items: center;
  margin: 0 10px;
}

.content {
  outline-style: none;
  border: 1px solid #ccc;
  border-radius: 3px;
  padding: 6px 6px;
  width: 200px;
}

input {
  font-size: 14px;
}

input:focus {
  border-color: #66afe9;
  outline: 0;
  -webkit-box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(102, 175, 233, 0.6);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(102, 175, 233, 0.6);
  font-size: 16px;
}

.tool-bar {
  display: flex;
  justify-content: center;
}
.tool-bar > div {
  margin: 10px;
  border: solid 1px rgba(102, 175, 233, 0.6);
  width: 50px;
  height: 30px;
  line-height: 30px;
  border-radius: 5px;
  cursor: pointer;

  -moz-user-select: none; /*火狐*/
  -webkit-user-select: none; /*webkit浏览器*/
  -ms-user-select: none; /*IE10*/
  -khtml-user-select: none; /*早期浏览器*/
  user-select: none;
}

.tool-bar > div:active {
  font-size: 19px;
}
#cnzz_stat_icon_1278856908 {
  display: none;
  width: 0;
  height: 0;
}
.ad {
  display: flex;
  width: 70%;
  align-content: center;
  justify-content: center;
  border: solid 1px rgba(0, 0, 0, 0.075);
  margin: 0 auto;
  margin-top: 120px;
}
.ad > div {
  display: flex;
  background-color: rgba(247, 239, 239, 0.945);
}
.fimage {
  width: 70%;
}
.fdesc {
  width: 30%;
  flex-direction: column;
}
.fruit {
  width: 100%;
}
.desc {
  margin: 10px 10px;
  line-height: 30px;
}
.desc span {
  color: #c9531c;
}
.qrcode {
  margin: 0px 20px;
}
.qrcode img {
  width: 100%;
}
</style>
