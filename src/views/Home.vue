<template>
  <div class="home">
    <div class="qa_content">
      <h1>答题系统</h1>
      <div class="qa_wrap" v-for="(item,index) in qaArr" :key="index">
        <dl>
          <dt>{{index + 1}}.{{item.tigan}} {{item.useA}}</dt>
          <dd v-for="(k,i) in item.xuanxiang" :key="i">
            <span>
              <input
                type="radio"
                :name="item.t_id"
                :value="k.id"
                v-model="answerArr[index]"
                @change="getAnswer(index,i)"
                :disabled="item.isTrue"
              />
              {{k.content}}
            </span>
          </dd>
        </dl>
      </div>
    </div>
    <div class="qa_number">
      <div class="number_wrap">
        <span class="span1">实时分数</span>
        <span class="span2">{{sums}}</span>
        <div class="qa_online">
          <span class="span3" :class="el.act" v-for="(el,ins) in qaArr" :key="ins">{{ins+1}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      qaArr: [],
      answerArr: [],
      trueAnswerArr: [],
      sums: 0,
      useA: "",
      zimu: ["A", "B", "C", "D", "E", "F"]
    };
  },
  methods: {
    getAnswer(index, i) {
      console.log(this.answerArr);
      this.$set(this.qaArr[index], "useA", this.zimu[i]);
      console.log(this.qaArr);
      let sum = 0;
      this.answerArr.forEach((element, ins) => {
        this.$set(this.qaArr[index], "isTrue", "true");
        if (element == this.trueAnswerArr[ins]) {
          sum += 5;
          this.$set(this.qaArr[index], "act", "true");
        } else {
          this.$set(this.qaArr[index], "act", "false");
        }
      });
      this.sums = sum;

      if (this.sums == 100) {
        alert('给力啊铁子，100分耶')
      }
      return sum;


    }
  },
  created() {
    this.$http.get("202006.json").then(res => {
      this.qaArr = res.data;
      this.qaArr.forEach((el, index) => {
        el.daan.forEach((k, i) => {
          console.log(k.id);
          if (k.isright == "1") {
            this.trueAnswerArr[index] = k.id;
          }
        });
      });
    });
  
  },

  components: {}
};
</script>
<style lang="less">
.home {
  display: flex;
  background: #f2f2f2;
  width: 100%;

  .qa_content {
    h1 {
      text-align: center;
    }
    flex: 1;
    .qa_wrap {
      margin: 20px auto;
      width: calc(95% - 40px);
      padding: 20px;
      background: #fff;
      border-radius: 10px;
      dt {
        font-size: 16px;
        font-weight: 700;
        padding-bottom: 5px;
      }
      dd {
        padding: 5px 0;
        font-size: 14px;
        padding-left: 20px;
      }
    }
  }
  .qa_number {
    width: 120px;
    text-align: center;
    background: #fff;

    .number_wrap {
      width: 120px;
      height: 200px;
      position: fixed;
      top: calc(50% - 100px);
      right: 0;
      .qa_online {
        width: 100%;
        display: flex;
        margin-top: 20px;
        flex-wrap: wrap;
        .span3 {
          flex: 0 0 auto;
          display: inline-block;
          width: 18px;
          height: 18px;
          margin: 5px;
          border-radius: 18px;
          border: 1px solid #999;
          font-size: 10px;
          line-height: 18px;
          &.false {
            background: orange;
            color: #fff;
            border: 1px solid orange;
          }
          &.true {
            background: #2d6d4b;
            color: #fff;
            border: 1px solid #2d6d4b;
          }
        }
      }
      span {
        display: block;
        &.span1 {
          font-weight: 700;
          color: rgb(36, 35, 35);
        }
        &.span2 {
          color: red;
          font-size: 20px;
          font-weight: 700;
        }
      }
    }
  }
}
</style>
