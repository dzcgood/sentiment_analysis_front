<template>
  <div>
    <el-scrollbar max-height="1000px" height="1000px">
      <!--输入部分-->
      <div class="wrapper">
        <div class="shadow">
          <el-form :inline="true" :model="input">
            <div class="margin-bottom-0">
              <el-form-item label="微博用户ID" required>
                <el-input v-model="input.weiboId" placeholder="微博用户ID" />
              </el-form-item>
              <el-form-item label="cookie">
                <el-input v-model="input.cookie" placeholder="cookie" />
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="onSubmit">爬虫一下</el-button>
              </el-form-item>
            </div>
          </el-form>
        </div>
      </div>

      <!--用户信息界面-->
      <div class="wrapper">
        <div class="shadow">
          <el-row>
            <el-col :span="12">
              <el-row align="bottom">
                <el-col :span="4" style="height: 100px">
                  <el-avatar
                    :size="100"
                    src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
                  />
                </el-col>
                <el-col :span="20" id="user-info">
                  <el-row>
                    <span id="nickname">{{ weiboUser.nick_name }}</span>
                  </el-row>
                  <el-row id="desc">
                    <span style="margin: 0 5px 0 0"
                      >粉丝 {{ weiboUser.followers_count }}</span
                    >
                    <span>关注 {{ weiboUser.friends_count }}</span>
                  </el-row>
                </el-col>
              </el-row>
              <el-row style="padding: 20px 0 0 20px">
                <el-descriptions
                  class="margin-top"
                  :column="2"
                  size="default"
                  style="font-size: 14px"
                >
                  <el-descriptions-item :span="2">
                    <el-icon :size="15"><Document /></el-icon>
                    {{ weiboUser.description }}
                  </el-descriptions-item>
                  <el-descriptions-item>
                    <el-icon :size="15"><Clock /></el-icon>
                    {{ weiboUser.created_at }}
                  </el-descriptions-item>
                  <el-descriptions-item>
                    <el-icon :size="15"><Location /></el-icon>
                    {{ weiboUser.location }}
                  </el-descriptions-item>
                </el-descriptions>
              </el-row>
            </el-col>

            <el-col style="text-align: center" :span="12">
              <el-row :align="middle" style="margin-top:20px">
                <el-col :span="8" :offset="3">
                  <el-image
                    style="width: 150px; height: 150px;"
                    :src="imgUrlAndLabel[resultId].imgUrl"
                    fit
                  />
                </el-col>
                <el-col :span="2" style="color: #f89898; margin-top:100px">
                 {{
                    imgUrlAndLabel[resultId].label
                  }}
                </el-col>
              </el-row>

            </el-col>
          </el-row>
        </div>
      </div>

      <!--Echarts和词云-->
      <div class="wrapper">
        <div class="shadow">
          <el-row style="text-align: center">
            <el-col :span="12">
              <div class="echart" id="mychart" :style="myChartStyle"></div>
            </el-col>
            <el-col :span="12"> 我是词云 </el-col>
          </el-row>
        </div>
      </div>

      <div class="wrapper">
        <div class="shadow">
          <el-row style="text-align: center; font-weight: bold">
            <el-col :span="24"> 近期微博分析 </el-col>
          </el-row>
        </div>
      </div>

      <!--各条微博的情感分析-->
      <div v-for="(item, index) in weiboItem" :key="index">
        <!--展示部分-->
        <div class="wrapper">
          <div class="shadow">
            <!--用户信息-->
            <el-row align="bottom">
              <el-col :span="1" style="height: 50px">
                <el-avatar
                  :size="50"
                  src="https://cube.elemecdn.com/0/88/03b0d39583f48206768a7534e55bcpng.png"
                />
              </el-col>
              <el-col :span="23" id="user-info">
                <el-row>
                  <span id="nickname">优酷</span>
                </el-row>
                <el-row id="desc">
                  <span style="margin: 0 5px 0 0">timesssssssssssssssssss</span>
                  <span>desc</span>
                </el-row>
              </el-col>
            </el-row>
            <el-row style="margin: 20px 0">
              <el-col :span="22" :offset="1">
                <el-card
                  shadow="hover"
                  style="border: 2px dashed var(--el-border-color)"
                >
                  <span>{{ weiboContent }}</span>
                </el-card>
              </el-col>
            </el-row>

            <el-row align="middle" id="result">
              <el-col :span="3">情感分析结果</el-col>
              <el-col :span="1">
                <el-image
                  style="width: 50px; height: 50px"
                  :src="imgUrlAndLabel[resultId].imgUrl"
                  fit
                />
              </el-col>
              <el-col :span="2" style="color: #f89898; text-align: center">{{
                imgUrlAndLabel[resultId].label
              }}</el-col>
            </el-row>
          </div>
        </div>
      </div>
    </el-scrollbar>
  </div>
</template>

<script>
import { ref, reactive, onMounted } from "vue";
import * as echarts from "echarts";

export default {
  setup() {
    const input = reactive({
      weiboId: "",
      cookie: "",
    });

    function onSubmit() {
      console.log(input);
    }

    function analysis() {
      console.log(weiboContent.value);
    }

    const weiboUser = reactive({
      nick_name: "DzcGood",
      followers_count: 100,
      friends_count: 3,
      location: "北京 海淀区",
      description: "小米董事长，金山软件董事长。业余爱好是天使投资。",
      created_at: "2010-05-31 23:07:59",
    });

    const weiboContent = ref(
      "wow女神又有新片了！！！[送花花][送花花]开机现场的状态也太好了吧，红色卫衣真是红气养人，承包女神的这个笑容了[爱你][抱一抱]算起来已经有快4年没在影院大银幕上看到舒淇了，而且这次还是犯罪悬疑题材，咱就是一整个狠狠期待！希望能早日在影院看到这部《黎明时分见》，迫不及待想看到银幕上的女神舒淇了！"
    );

    const imgUrlAndLabel = reactive([
      {
        imgUrl: require("@/assets/happy.svg"),
        label: "积极",
      },
      {
        imgUrl: require("@/assets/mad.svg"),
        label: "愤怒",
      },
      {
        imgUrl: require("@/assets/sad.svg"),
        label: "悲伤",
      },
      {
        imgUrl: require("@/assets/fear.svg"),
        label: "恐惧",
      },
      {
        imgUrl: require("@/assets/surprised.svg"),
        label: "惊奇",
      },
      {
        imgUrl: require("@/assets/neutral.svg"),
        label: "无情绪",
      },
    ]);

    const weiboItem = reactive([
      {
        imgUrl: "",
        nickName: "",
        time: "",
        desc: "",
        resultId: 0,
      },
      {
        imgUrl: "",
        nickName: "",
        time: "",
        desc: "",
        resultId: 0,
      },
      {
        imgUrl: "",
        nickName: "",
        time: "",
        desc: "",
        resultId: 0,
      },
      {
        imgUrl: "",
        nickName: "",
        time: "",
        desc: "",
        resultId: 0,
      },
      {
        imgUrl: "",
        nickName: "",
        time: "",
        desc: "",
        resultId: 0,
      },
    ]);

    const resultId = ref(2);

    // 以下是echart相关
    const mychart = {};
    const pieData = [
      {
        value: 1,
        name: "积极",
      },
      {
        value: 2,
        name: "愤怒",
      },
      {
        value: 3,
        name: "悲伤",
      },
      {
        value: 1,
        name: "恐惧",
      },
      {
        value: 1,
        name: "惊奇",
      },
      {
        value: 2,
        name: "无情绪",
      },
    ];

    const pieName = [];
    const myChartStyle = { float: "left", width: "100%", height: "300px" }; //图表样式

    onMounted(() => {
      for (let i = 0; i < pieData.length; i++) {
        pieName[i] = pieData[i].name;
      }
      const option = {
        legend: {
          // 图例
          data: pieName,
          right: "0%",
          top: "30%",
          orient: "vertical",
        },
        title: {
          // 设置饼图标题，位置设为顶部居中
          text: "各类情绪占比",
          top: "0%",
          left: "center",
        },
        series: [
          {
            type: "pie",
            label: {
              show: true,
              formatter: "{b} : {c} ({d}%)", // b代表名称，c代表对应值，d代表百分比
            },
            left: "0%",
            radius: "30%", //饼图半径
            data: pieData,
          },
        ],
      };

      const myChart = echarts.init(document.getElementById("mychart"));
      myChart.setOption(option);
      //随着屏幕大小调节图表
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    });

    return {
      input,
      onSubmit,
      weiboContent,
      analysis,
      imgUrlAndLabel,
      resultId,
      weiboItem,
      weiboUser,
      pieName,
      myChartStyle,
    };
  },
};
</script>

<style scoped>
.wrapper {
  width: 90%;
  margin: 15px 5%;
}

.shadow {
  box-shadow: var(--el-box-shadow-light);
  width: 100%;
  padding: 10px;
  text-align: left;
}

.margin-bottom-0 .el-form-item {
  margin-bottom: 0 !important;
}

#user-info .el-row {
  padding-left: 25px;
}

#nickname {
  font-size: 20px;
  height: 35px;
  font-weight: bold;
}

#desc {
  font-size: 15px;
  color: #939393;
}

#result {
  margin: 10px 0 0 0;
}
</style>
