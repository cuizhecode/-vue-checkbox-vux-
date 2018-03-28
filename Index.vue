<template src="./index.html"></template>
<script>
import { Checklist } from "vux";
export default {
  name: "selectBuyer",
  data() {
    return {
      fullValues: [],
      checkboxData: [
        {
          id: "1",
          value: "苹果4S",
          price: 110
        },
        {
          id: "2",
          value: "苹果5C",
          price: 250
        },
        {
          id: "3",
          value: "苹果6P",
          price: 340
        }
      ],
      selectedData: [],
      totalPrice: 0.00,
      isAllChecked: true,
      checkBox: {
        checked: false,
        items: {}
      }
    };
  },
  mounted() {
    this.checkboxData.forEach((item, index) => {
      this.selectedData.push(item.id);
      this.$set(this.checkBox.items, index, !this.checkBox.checked);
    });
  },
  computed: {
    totalPurchasers() {
      return this.selectedData.length;
    },
    calculatedTotal() {
      this.totalPrice = 0.00;
      this.selectedData.map((item1, index) => {
        let curItem1 = item1;
        this.checkboxData.map((item2, index) => {
          if(this.checkboxData[index].id == curItem1){
            this.totalPrice += this.checkboxData[index].price;
          }
        });
      });
      return this.totalPrice;
    }
  },
  methods: {
    //全选点击事件
    checkedAll() {
      let trueNum = 0;
      let checkBoxNum = 0;
      Object.keys(this.checkBox.items).forEach(key => {
        checkBoxNum++;
        if (this.checkBox.items[key] === true) {
          trueNum++;
        }
      });
      if (trueNum != 0) {
        if (
          trueNum != this.checkboxData.length ||
          checkBoxNum < this.checkboxData.length
        ) {
          this.checkboxDataMap(!this.checkBox.checked);
        } else {
          this.checkboxDataMap(this.checkBox.checked);
        }
      } else {
        this.checkboxDataMap(!this.checkBox.checked);
      }
    },
    //遍历改变checkbox状态
    checkboxDataMap(checked) {
      let checkboxDataId = [];
      this.checkboxData.forEach((item, index) => {
        this.checkBox.items[index] = checked;
        checkboxDataId.push(item.id);
      });
      if (checked == true) {
        this.selectedData = this.arrayMerging(
          this.selectedData,
          checkboxDataId
        );
      } else {
        this.selectedData.splice(0, this.selectedData.length);
      }
    },
    //input输入框change事件
    handleInputChange(e) {
      setTimeout(() => {
        if (this.selectedData.indexOf(e.target.value) > -1) {
          this.remove(this.selectedData, e.target.value);
        } else {
          this.selectedData.push(e.target.value);
        }
        if (this.selectedData.length < this.checkboxData.length) {
          this.isAllChecked = false;
        } else {
          this.isAllChecked = true;
        }
      }, 0);
    },
    //数组删除
    remove(arr, val) {
      var index = arr.indexOf(val);
      if (index > -1) {
        arr.splice(index, 1);
      }
    },
    //数组合并去重
    arrayMerging(arr1, arr2) {
      var arr = arr1.concat();
      for (var i = 0; i < arr2.length; i++) {
        if (arr.indexOf(arr2[i]) === -1) {
          arr.push(arr2[i]);
        }
      }
      return arr;
    }
  },
  components: {
    Checklist
  },
  metaInfo() {
    return {
      title: "选择购卡学生"
    };
  }
};
</script>
<style src="./index.less" lang="less"></style>

