<template>
  <div>
    <p @click="controlTime">时间选择器（移动端） {{time}}</p>
    <div class="time_select">
      <mt-popup v-model="bottomAlert" position="bottom">
        <div class="btn_box">
          <mt-button plain @click.native="controlTime">取消</mt-button>
          <mt-button plain type="primary" @click.native="controlTime">确认</mt-button>
        </div>
        <mt-picker :slots="slots" @change="onValuesChange"></mt-picker>
      </mt-popup>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      bottomAlert: false,
      time: '',
      date: '',
      slots: [
        {
          flex: 1,
          values: ['今天', '明天', '后天'],
          className: 'slot1',
          textAlign: 'center'
        },
        {
          divider: true,
          content: '-',
          className: 'slot2'
        },
        {
          flex: 1,
          values: [],
          className: 'slot3',
          textAlign: 'center'
        },
        {
          divider: true,
          content: '-',
          className: 'slot4'
        },
        {
          flex: 1,
          values: ['00', '10', '20', '30', '40', '50'],
          className: 'slot5',
          textAlign: 'center'
        }
      ]
    }
  },
  watch: {
    time (val) {
      // console.log("this.time", val);
      // this.$emit('spot', [val, this.date]) // 向父组件传值
      if (this.bottomAlert) {
        sessionStorage.setItem('timeDate', val)
      }
    }
  },
  methods: {
    GetDateStr (AddDayCount) {
      var dd = new Date()
      dd.setDate(dd.getDate() + AddDayCount) // 获取AddDayCount天后的日期
      var y = dd.getFullYear()
      var m = dd.getMonth() + 1 // 获取当前月份的日期
      var d = dd.getDate()
      return y + '-' + m + '-' + d
    },
    controlTime () {
      this.bottomAlert = !this.bottomAlert
      if (this.bottomAlert === false) {
        sessionStorage.setItem('timeDate', this.time)
      }
    },
    // 时间选择器
    onValuesChange (picker, values) {
      if (values[0] === '今天') {
        this.date = this.GetDateStr(0)
      } else if (values[0] === '明天') {
        this.date = this.GetDateStr(1)
      } else if (values[0] === '后天') {
        this.date = this.GetDateStr(2)
      }
      this.time = values[0] + ' ' + values[1] + ':' + values[2]
      if (picker.getSlotValue(0) === '今天') {
        var h = new Date().getHours()
        var timeToday = []
        for (var i = h + 1; i < 18; i++) {
          if (i < 10) {
            i = '0' + i
          }
          timeToday.push(i)
        }
        if (timeToday.length === 0) {
          picker.setSlotValues(1, ['下班了'])
          picker.setSlotValues(2, ['！'])
        } else {
          picker.setSlotValues(1, timeToday)
          picker.setSlotValues(2, ['00', '10', '20', '30', '40', '50'])
        }
      } else if (picker.getSlotValue(0) === '明天') {
        picker.setSlotValues(1, [
          '09',
          '10',
          '11',
          '12',
          '13',
          '14',
          '15',
          '16',
          '17'
        ])
        picker.setSlotValues(2, ['00', '10', '20', '30', '40', '50'])
      } else if (picker.getSlotValue(0) === '后天') {
        picker.setSlotValues(1, [
          '09',
          '10',
          '11',
          '12',
          '13',
          '14',
          '15',
          '16',
          '17'
        ])
        picker.setSlotValues(2, ['00', '10', '20', '30', '40', '50'])
      }
    }
  },
  mounted () {
    this.time = '选择时间'
    var h = new Date().getHours()
    for (var i = h + 1; i < 24; i++) {
      this.slots[2].values.push(i)
      // console.log(this.slots[2].values)
    }
  }
}
</script>
<style lang="less" scoped>
.mint-popup {
  border-radius: 5px 5px 0 0;
  padding: 0 12px;
}
.mint-popup-bottom {
  width: 100%;
  height: 40%;
  left: none;
}
.btn_box {
  // width: 100%;
  padding: 12px 31px;
  // margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  .mint-button--default.is-plain,
  .mint-button--primary.is-plain {
    width: 58px;
    height: 30px;
    font-size: 15px;
    cursor: pointer;
  }
  .mint-button--primary.is-plain {
    border: 1px solid #c3d826;
    color: #c3d826;
  }
}
</style>
