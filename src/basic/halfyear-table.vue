<template>
  <table @click="handleHalfyearTableClick"  class="el-halfyear-table el-month-table">
    <tbody>
    <tr>
      <td :class="getCellStyle(0)">
        <a class="cell">上半年</a>
      </td>
      <td :class="getCellStyle(1)">
        <a class="cell">下半年</a>
      </td>
    </tr>
    </tbody>
  </table>
</template>

<script type="text/babel">
  import { hasClass } from 'element-ui/src/utils/dom';

  export default {
      props: {
          disabledDate: {},
          date: {},
          halfyear: {
              type: Number
          }
      },
      methods: {
          getCellStyle(halfyear) {
              const style = {};

              var year = this.date.getFullYear();
              var date = new Date(0);
              date.setFullYear(year);
              date.setMonth(halfyear * 6);
              date.setHours(0);
              var nextHalfyear = new Date(date);
              if (halfyear === 0) {
                  nextHalfyear.setMonth(halfyear * 6 + 6);
              } else {
                  nextHalfyear.setMonth(0);
                  nextHalfyear.setFullYear(year + 1);
              }

              // 如果有一天可选，那么该项可选
              var flag = false;
              if (typeof this.disabledDate === 'function') {

                  while (date < nextHalfyear) {
                      if (this.disabledDate(date)) {
                          date = new Date(date.getTime() + 8.64e7);
                          flag = true;
                      } else {
                          flag = false;
                          break;
                      }
                  }
              }

              style.disabled = flag;
              style.current = this.halfyear === halfyear;

              return style;
          },

          handleHalfyearTableClick(event) {
              const target = event.target;
              if (target.tagName !== 'A') return;
              if (hasClass(target.parentNode, 'disabled')) return;
              const column = target.parentNode.cellIndex;
              this.$emit('pick', column);
          }
      }
  };
</script>

<style lang="scss">
.el-halfyear-table {
    .cell {
        margin: 0 auto;
    }
}
</style>