<template>
  <div>
    <!--输入部分-->
    <div class="input">
      <div class="wrapper">
        <el-row id="info" align="middle">
          <el-col :span="5">
            <span>请输入一段需要分析的文本：</span>
          </el-col>

          <el-col :span="2">
            <el-button type="primary" text @click="changeInput"
              >换一个示例</el-button
            >
          </el-col>
        </el-row>

        <el-row id="textarea">
          <el-col :span="24">
            <el-input
              type="textarea"
              v-model="input"
              :placeholder="placeholder[curIndex]"
              rows="4"
              maxlength="140"
              show-word-limit
            />
          </el-col>
        </el-row>
        <el-row justify="space-between" align="middle" id="do_analysis">
          <el-col :span="4" style="color: #c8c9cc">
            <span>最多输入</span>
            <span style="color: #eebe77">140</span>
            <span>字</span>
          </el-col>

          <el-col :span="2">
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

    <!--历史记录部分-->
    <div id="history">
      <div class="input">
        <div class="wrapper">
          <el-row>
            <el-col :span="12">
              <span>历史分析：</span>
            </el-col>
            <el-col :span="12" v-if="tableData.length == 0">
              <span>无相关记录</span>
            </el-col>
          </el-row>
        </div>
      </div>

      <div class="input" v-if="tableData.length > 0">
        <div class="wrapper">
          <el-row>
            <el-table
              :data="tableData"
              style="width: 100%"
              height="450"
              tableLayout="fixed"
              stripe
            >
              <el-table-column fixed prop="date" label="Date" />
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
    const placeholder = reactive([
      "今天天气真好，心情舒畅！",
      "一出门摔了一跤，真是服了。",
      "明天会下雨。",
    ]);
    const curIndex = ref(0);
    const input = ref("");

    function changeInput() {
      curIndex.value = (curIndex.value + 1) % placeholder.length;
      input.value = "";
    }

    function analysis() {
      if (input.value == "") {
        input.value = placeholder[curIndex.value];
      }
      console.log(input.value);
    }

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
    return {
      input,
      placeholder,
      curIndex,
      changeInput,
      analysis,
      tableData,
      imgUrlAndLabel,
      resultId,
      removeOneHistory
    };
  },
};
</script>

<style scoped>
.input {
  width: 90%;
  margin: 15px 5%;
}
#info {
  text-align: left;
  margin-bottom: 5px;
}

.wrapper {
  box-shadow: var(--el-box-shadow-light);
  width: 100%;
  padding: 10px;
  text-align: left;
}

#textarea {
  margin-bottom: 10px;
}

#do_analysis {
  border-bottom: 1px solid var(--el-border-color);
  padding-bottom: 10px;
}

#result {
  margin: 10px 0 0 0;
}

#history {
  width: 100%;
  border-top: 1px solid var(--el-border-color);
  padding-top: 10px;
}
</style>
