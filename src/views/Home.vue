<template>
  <div class="home">
    <div class="block">
      <span class="demonstration">默认</span>
      <el-date-picker
        v-model="value"
        type="daterange"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期"
        :picker-options="pickerOptions"
      >
      </el-date-picker>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      value: "",
      // 可选择的日期最小值
      smallDate: "",
      // 可选择的日期最大值
      bigDate: "",
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      // 初始化方法：设置开始日期为一周前，结束日期为当前日期
      var maxDate = new Date();
      let date = new Date();
      var minDate = date.setDate(date.getDate() - 6);
      this.value = [minDate, maxDate];
    },
  },
  computed: {
    pickerOptions() {
      let that = this;
      return {
        disabledDate: (date) => {
          // 默认（最大和最小可选日期都为空）是点击选择日期的操作，开始设定不可选择的日期为当前日期之后的日期
          if (that.smallDate == "" && that.bigDate == "") {
            return date >= new Date();
          }
          // 通过第一次点击的日期去设定最大可选日期和最小可选日期
          var maxDate = new Date(that.bigDate || new Date());
          var minDate = new Date(that.smallDate);
          return date >= maxDate || date < minDate;
        },
        onPick: function ({ maxDate, minDate }) {
          if (!maxDate) {
            // 第一次点击日期后的执行逻辑
            let currDate = new Date(minDate);
            maxDate = currDate.setDate(currDate.getDate() + 30);
            that.bigDate = maxDate >= new Date() ? new Date() : maxDate;
            let currDate2 = new Date(minDate);
            that.smallDate = new Date(
              currDate2.setDate(currDate2.getDate() - 29)
            );
          } else {
            // 点击完两个日期之后的执行逻辑
            that.smallDate = "";
            that.bigDate = "";
          }
        },
      };
    },
  },
};
</script>
