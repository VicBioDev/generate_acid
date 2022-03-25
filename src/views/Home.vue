<template>
  <div>
    <div v-show="isShow == false">
      <Input v-model="name">
        <span slot="prepend">Name(*X)</span>
      </Input>
      <Input v-model="id">
        <span slot="prepend">Identify Card No</span>
      </Input>
      <Input v-model="count">
        <span slot="prepend">Count</span>
      </Input>
      <Button type="submit" @click="showToggle"
        >Show(is showing then scroll bottom and click to visable)</Button
      >
    </div>
    <div v-show="isShow">
      <div class="title">
        <span style="font-size: 12px">本查询服务由上海市健康委员会提供</span>
      </div>
      <div
        style="background: #eee; padding: 20px"
        v-for="index of report"
        :key="index"
      >
        <div>
          <Row>
            <div style="width: 100%; background: #fff">
              <div>
                <img
                  src="../assets/images/title.png"
                  style="width: 100%; height: 40px"
                />
              </div>
            </div>
          </Row>
          <div class="card">
            <Row style="height: 40px">
              <Col span="10"><span class="text">姓名</span></Col>
              <Col span="14">
                <span class="textBlack">
                  <img
                    src="../assets/images/eye.png"
                    style="margin-top: 5px; margin-bottom: -7px"
                  />{{ name }}</span
                ></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">证件类型</span></Col>
              <Col span="14"><span class="textBlack">身份证</span></Col>
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">证件号码</span></Col>
              <Col span="14"
                ><span class="textBlack">{{
                  id.substr(0, 4) +
                  '***********' +
                  id.substr(14, id.split('').length)
                }}</span></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">样本编号</span></Col>
              <Col span="14"
                ><span class="textBlack">{{ index.testNo }}</span></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text blue">采样时间</span></Col>
              <Col span="14"
                ><span class="textBlack blue">{{ index.testTime }}</span></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">检测机构</span></Col>
              <Col span="14"
                ><span class="textBlack">{{
                  hostipal[Math.floor(Math.random() * hostipal.length)]
                }}</span></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">检测时间</span></Col>
              <Col span="14"
                ><span class="textBlack">{{ index.reportTime }}</span></Col
              >
            </Row>
            <Row style="height: 40px">
              <Col span="10"><span class="text">检测项目</span></Col>
              <Col span="14"><span class="textBlack">核酸</span></Col>
            </Row>

            <hr style="border: 1px dotted; color: #ccc" />
            <br />

            <Row style="height: 40px">
              <Col span="8"><span class="text blue">检测结果</span></Col>
              <Col span="16"
                ><span class="textBlack blue">ORF1a/b阴性, N基因阴性</span></Col
              >
            </Row>
            <!-- <Row>
          <div class="hr">11</div>
        </Row> -->
          </div>
        </div>
      </div>
    </div>
    <Row>
      <Col span="24">
        <Div type="submit" @click="showToggle" style="color: #fff; height: 30px"
          >不显示</Div
        >
      </Col>
    </Row>
  </div>
</template>

<script>
import dayjs from 'dayjs';

function getRandom(num) {
  var random = Math.floor(
    (Math.random() + Math.floor(Math.random() * 9 + 1)) * Math.pow(10, num - 1)
  );
  return random;
}

export default {
  data() {
    return {
      id: '',
      name: '',
      title: 'Hello',
      count: 10,
      report: [],
      isShow: false,
      genearteShow: true,
      hostipal: [
        '上海金域医学检验所',
        '上海枫林医学检验所',
        '上海华大医学检验所',
        '上海艾迪康医学检验所',
        '上海之江医学检验所',
        '上海兰卫医学检验所',
        '上海贝瑞和康医学检验所',
        '上海中检医学检验所',
        '上海千麦博米乐医学检验所',
        '上海宝藤医学检验所',
        '上海华测艾普医学检验所'
      ]
    };
  },

  components: {},
  onLoad() {},
  methods: {
    showToggle() {
      if (this.isShow) {
        this.genearteShow = false;
      } else {
        this.genearteShow = false;
      }
      this.makeReport();
      this.isShow = !this.isShow;
    },
    makeReport() {
      for (var i = 0; i < this.count; i++) {
        // and the formula is:
        let subDay = i + 1;
        const testTime = dayjs()
          .subtract(subDay, 'day')
          .subtract(Math.floor(Math.random() * 23), 'hour')
          .subtract(Math.floor(Math.random() * 59), 'minute')
          .subtract(Math.floor(Math.random() * 59), 'second')
          .format('YYYY-MM-DD HH:mm:ss');
        const reportTime = dayjs(testTime, 'YYYY-MM-DD HH:mm:ss').add(2, 'day');

        const data = {
          testTime: testTime,
          reportTime: reportTime.format('YYYY-MM-DD'),
          testNo: getRandom(10)
        };
        this.report.push(data);
      }
    }
  },
  mounted() {
    //this.makeReport();
    this.$on('offline', () => {
      alert('You are offline! The website will not work');
    });
  }
};
</script>

<style>
.blue {
  color: #3a6ac1;
}
.grey {
  color: #cccccc;
}
.balck {
  color: #000000;
}
.card {
  padding: 20px;
  background: #fff;
}
.text {
  font-size: 16px;
}
.textBlack {
  font-size: 16px;
  text-align: right;
  float: right;
}
.title {
  text-align: center;
  size: 8px;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid;
  border-right: 1px solid;
  border-left: 1px solid;
  background-color: #fdf6c2;
  color: #ecb552;
}

.hr {
  background-image: url('../assets/images/hr.png');
  background-repeat: repeat-x;
}
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin: 200rpx auto 50rpx auto;
}

.text-area {
  display: flex;
  justify-content: center;
}
</style>
