# 通过一个单选框联动选择的例子来对比vue与jquery的代码量和编程思路的区别

* jquery核心为dom操作，jquery自身已经封装了大部分dom操作的方法，所以代码编写看起来相对更加简洁。

html代码
```html
<div id="checkboxTable">
    <div>
        <input type="checkbox"  name="c_test" value="one1-1"/>
        <input type="checkbox"  name="c_test" value="one2-1"/>
    </div>
    <div>
        <input type="checkbox"  name="c_test" value="one1-2"/>
        <input type="checkbox"  name="c_test" value="one2-2"/>
    </div>
</div>
```

js代码
```javascript 1.5
  $(function () {
    $("#checkboxTable div").each(function(key,val){
      $(val).find("input").each(function (k1,v1) {
        if(k1==0){
            $(v1).change(function(){
              $(this).parent().find("input").prop("checked",$(this).prop("checked"));
            });
        }
      })
    })
  })
```

* vue核心操作为数据，从一开始就进行数据准备，到后期数据操作虽然产生了大量的代码，但是代码自身更加简单，完全是对数据对象进行操作。并且在操作过程中直接完成了数据获取的功能不需要单独再获取已选数据。

html代码
```html
<div id="checkboxTable">
    <div v-for="pced in pickeds">
        <input type="checkbox" :value="pced[0]" v-model="picked" @change="checkLine(pced[0])" />
        <input type="checkbox" :value="pced[1]" v-model="picked1"/><br>
    </div>
</div>
```


js代码
```javascript 1.5
 new Vue({
    el: '#checkboxTable',
    data: {
      pickeds :[["one1-1","one2-1"],["one1-2","one2-2"]],
      picked: [],
      picked1: []
    },
    methods:{
      checkLine:function(nowVal){
        var pickedChecked1 = new Array();

        var isIn = false;

        for(var i=0;i<this.picked.length;i++){
          if(nowVal===this.picked[i]){
              isIn = true;
              for(var a=0;a<this.pickeds.length;a++){
                if(this.pickeds[a][0]===this.picked[i]){
                  pickedChecked1.push(this.pickeds[a][1])
                  break;
                }
              }
          }
        }

        var nowVal1 = "";
        for(var a=0;a<this.pickeds.length;a++){
          if(this.pickeds[a][0]===nowVal){
            nowVal1 = this.pickeds[a][1];
            break;
          }
        }

        for(var i=0;i<this.picked1.length;i++){
          if(isIn) {
            pickedChecked1.push(this.picked1[i]);
          }else{
            if(nowVal1!=this.picked1[i]) {
              pickedChecked1.push(this.picked1[i]);
            }
          }
        }

        this.picked1 = pickedChecked1

        console.info(this.picked1);
      }
    }
  })
```
