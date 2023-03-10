<template>
  <div>
    <!--输入部分-->
    <div class="wrapper">
      <div class="shadow">
        <el-form :inline="true" :model="input">
          <div class="margin-bottom-0">
            <el-form-item label="微博ID" required>
              <el-input v-model="input.weiboId" placeholder="微博ID" />
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
            <el-card shadow="hover" style="border: 2px dashed var(--el-border-color);">
              <span>{{ weiboContent }}</span>
            </el-card>
          </el-col>
        </el-row>

        <el-row justify="space-between" align="middle" id="do_analysis">

          <el-col :span="2" :offset="22">
            <el-button type="primary" @click="analysis">开始分析</el-button>
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

    <!--历史部分-->
    <div id="history">
      <div class="wrapper">
        <div class="shadow">
          <el-row>
            <el-col :span="12">
              <span>爬取记录：</span>
            </el-col>
            <el-col :span="12" v-if="tableData.length == 0">
              <span>无相关记录</span>
            </el-col>
          </el-row>
        </div>
      </div>

      <div class="wrapper" v-if="tableData.length > 0">
        <div class="shadow">
          <el-row>
            <el-table
              :data="tableData"
              style="width: 100%"
              height="450"
              tableLayout="fixed"
              stripe
            >
              <el-table-column fixed prop="date" label="Date" />
              <el-table-column fixed prop="weiboId" label="weiboID" />
              <el-table-column prop="content" label="Content" />
              <el-table-column prop="result" label="Result" />
              <el-table-column fixed="right" label="Operation" width="120"  type="index">
                <template #default="scope">
                    <el-button type="info" icon="Delete" circle  @click="removeOneHistory(scope.$index)"/>
                </template>
              </el-table-column>
            </el-table>
          </el-row>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive } from "vue";
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

    const weiboContent = ref('wow女神又有新片了！！！[送花花][送花花]开机现场的状态也太好了吧，红色卫衣真是红气养人，承包女神的这个笑容了[爱你][抱一抱]算起来已经有快4年没在影院大银幕上看到舒淇了，而且这次还是犯罪悬疑题材，咱就是一整个狠狠期待！希望能早日在影院看到这部《黎明时分见》，迫不及待想看到银幕上的女神舒淇了！')

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

    const resultId = ref(4);

    const tableData = reactive([
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅!",
        result: "sad",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
      {
        date: "2016-05-03",
        content: "今天天气真好，心情舒畅！",
        result: "happy",
      },
    ]);

    function removeOneHistory(index) {
        console.log(index)
        tableData.splice(index, 1)
    }
    return { input, onSubmit, weiboContent, analysis, imgUrlAndLabel, resultId, tableData, removeOneHistory};
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
  padding-left: 10px;
}

#nickname {
  font-size: 14px;
  height: 25px;
  font-weight: bold;
}

#desc {
  font-size: 12px;
  color: #939393;
}

#do_analysis {
  border-bottom: 1px solid var(--el-border-color);
  padding-bottom: 10px;
}

#result {
  margin: 10px 0 0 0;
}
</style>
