# Element-ui

## 一、安装

```shell
npm i element-ui -D
```

## 二、引入

#### 1.开发模式完整引入 

在 main.js 中写入以下内容：

```js
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

#### 2.生产模式通过CDN加载 

**index.html** 记得锁定版本号

```html
<!-- 引入ele样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui@2.3.6/lib/theme-chalk/index.css">
<!-- 引入ele组件库 -->
<script src="https://unpkg.com/element-ui@2.3.6/lib/index.js"></script>
```

**vue.config.js**

```js
module.exports = {
  chainWebpack: config => {
    config.when(process.env.NODE_ENV === 'production', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-prod.js')
      config
        .set('externals', {
          vue: 'Vue',
          'vue-router': 'VueRouter',
          axios: 'axios',
          "element-ui": "element-ui"
        })
    })
    // 开发模式
    config.when(process.env.NODE_ENV === 'development', config => {
      config
        .entry('app')
        .clear()
        .add('./src/main-dev.js')
    })
  }
}
```

## 三、使用笔记

### 01. Layout 布局

- 基本概念：一行通过分割为24栅格栏进行布局，如果要占满一行如下： 

  ```xml
  <!-- el-row 表示一行 -->
  <!-- el-col 表示一列 -->
  <el-row>
     <el-col :span="24">24</el-col>
  </el-row>
  ```

- 如何设置18个栅格栏位，那么会留出6个位置是空白区域； 

  ```xml
  <el-row>
      <el-col :span="18">18</el-col>
  </el-row>
    
  .el-row {
        background-color: #42b983;
        margin: 20px 0;
  }
  .el-col {
        background-color: pink;
        color: white;
        padding: 10px 0;
  }
  ```

- 如果想要四列占满24个栅格栏位，那么每列占6个栅格栏位； 

  ```xml
  <el-row>
      <el-col :span="6">6</el-col>
      <el-col :span="6">6</el-col>
      <el-col :span="6">6</el-col>
      <el-col :span="6">6</el-col>
  </el-row>
  
  .el-col {
      background-color: pink;
      color: white;
      padding: 10px 0;
      border-right: 1px solid #eee;
      box-sizing: border-box;  /* 边框不占位 */
  }
  ```

- 手册里 `el-col` 子元素还有一层 `div`，它可以被 `:gutter` 设置为间隔； 

  ```xml
  <el-row :gutter="10">
       <el-col :span="6"><div style="background-color: pink">6</div></el-col>
       <el-col :span="6"><div style="background-color: pink">6</div></el-col>
       <el-col :span="6"><div style="background-color: pink">6</div></el-col>
       <el-col :span="6"><div style="background-color: pink">6</div></el-col>
  </el-row>
  ```

   PS: 通过上面可以实现各种各样的混合布局，总计算值小于等于24即在一行内；

- 使用 `type="flex"`来设置栏位的对齐方式，可以用过 `justify` 来设置方向； 

  ```xml
  <el-row type="flex" justify="centet"></el-row>
  ```

- 通过使用 `:offset=n` 来设置栏位之间的偏移量； 

  ```ruby
  <el-col :span="6" :offset="1"> 
  ```

### 02. Container 布局

- 如果我们想要快速布局一个类后台布局的样式，可直接使用Container布局；

  - `<el-container> `是外部布局容器；
  - `<el-header> `表示头；
  - `<el-footer>` 表示脚；
  - `<el-main>` 表示主体；
  - `<el-aside> `表示侧栏；

- 布局的方式手册提供了好几种，我们就使用最常用的一种即可，类后台方式： 

  ```xml
  <el-container>
     <el-header> header </el-header>
     <el-container>
         <el-aside> aside </el-aside>
         <el-main> main </el-main>
     </el-container>
     <el-footer> footer </el-footer>
  </el-container>
  ```

### 03. Basic 组件规范

- 纯理论，了解关于颜色、字体、边框等规范要求：
  - 颜色：主色、辅助色、中性色；
  - 字体：字体、字号、行高；
  - 边框：实线、虚线、圆角、投影；

### 04. Icon 图标 和 Link文字

##### 1. Icon 图标

- UI 提供了一套非常丰富的图标系统，基本上可以满足所有正常场景下的要求；

- 一般来说，具体使用采用<i>标签来直接插入，当然也可以通过按钮和链接插入；

  ```jsx
  <i class="el-icon-edit" style="margin-right:20px"></i>
  <el-button type="primary" class="el-icon-search"></el-button>
  ```

##### 2. Link文字

- Link 文字即超链接，提供了以下几种颜色方案：

  ```xml
  <el-link type="primary">链接</el-link>
  <el-link type="danger">链接</el-link>
  <el-link type="success">链接</el-link>
  <el-link type="warning">链接</el-link>
  <el-link type="info">链接</el-link>
  ```

-  `disabled` 属性，能让链接禁用；悬停可以通过 `:underline="false"` 关闭；

  ```xml
  <el-link type="warning" disabled>链接</el-link>
  <el-link type="info" :underline="false">链接</el-link>
  ```

- Link 文字也支持使用 icon 图标的功能；

  ```csharp
  <el-link type="success" class="el-icon-edit">链接</el-link>
  ```

### 05. Button 按钮

- Button 按钮和上节的 Link 文字一样，有固定的几个样式：

  ```xml
  <el-button type="primary">按钮</el-button>
  <el-button type="danger">按钮</el-button>
  <el-button type="success">按钮</el-button>
  <el-button type="warning">按钮</el-button>
  <el-button type="info">按钮</el-button>
  ```

- 使用 `plain` 属性，可以实现镂空效果； 使用 `disabled` 属性，可以禁用；

  ```xml
  <el-button type="warning" plain>按钮</el-button>
  <el-button type="info" disabled>按钮</el-button>
  ```

- 使用 `type="text"` 可以将 button 转成纯文本按钮；

  ```xml
  <el-button type="text">文字按钮</el-button>
  ```

- 使用 `` 可以将多个按钮设置成组合按钮；

  ```xml
  <el-button-group>
        <el-button type="primary" class="el-icon-arrow-left">上一页</el-button>
        <el-button type="primary">下一页<i class="el-icon-arrow-right"></i></el-button>
  </el-button-group>
  ```

- 使用 `:loading="true"` 实现加载中效果；

  ```xml
  <el-button type="primary" :loading="true">加载中</el-button>
  ```

- 使用 `size="small"` 等属性，实现三种大小；

  ```xml
  <!-- size: medium / small / mini -->
  <el-button type="primary" size="samll">按钮</el-button>
  ```

### 06. Radio 和 Checkbox

##### 1. Radio 单选框

- 常用的单选框，具体如下：

  ```kotlin
  <el-radio v-model="sex" label="1">男</el-radio>
  <el-radio v-model="sex" label="2">女</el-radio>
  
  data() {
      return {
          sex: '1'
      }
  }
  ```

  PS: 这里 v-model 和 sex 进行双向绑定，label 相当于 value，且和 sex 值一致则首选；

- 使用群组单选框，可以将双向绑定置顶操作，具体如下：

  ```jsx
  <el-radio-group v-model="city">
      <el-radio :label="1">北京</el-radio>
      <el-radio :label="2">上海</el-radio>
      <el-radio :label="3">广州</el-radio>
  </el-radio-group>
  
  data() {
    return {
        city: 1
    }
  }
  ```

  PS：禁用设置：`disabled`; 边框设置：`border`；

- 可以使用 `el-radio-button` 实现按钮式单选框，`size` 可以设置大小；

  ```xml
  <el-radio-button label="1">北京</el-radio-button>
  ```

  PS：可以再群组 `el-radio-button` 设置 `size` 属性，值为：`medium`、`small`、`mini` ；

- 单选框或群组单选都具有 `change` 事件，改变后即可触发；

  ```jsx
  <el-radio-group v-model="city" @change="radioChange">
      <el-radio :label="1">北京</el-radio>
      <el-radio :label="2">上海</el-radio>
      <el-radio :label="3">广州</el-radio>
  </el-radio-group>
  
  methods: {
    // 方法
    radioChange(res) {
        console.log('结果 --- ', res);
    }
  }
  ```

##### 2. Checkbox 复选框

- 和单选框基本类似，复选框具体如下：

  ```jsx
  <el-checkbox-group v-model="checklist">
        <el-checkbox label="音乐"></el-checkbox>
        <el-checkbox label="体育"></el-checkbox>
        <el-checkbox label="电脑"></el-checkbox>
  </el-checkbox-group>
  
  return {
      checklist: ['体育', '电脑']
  }
  ```

  PS：单一复选框，比如协议确定，直接使用 `el-checkbox` 不需要群组；

- 使用 `:max` / `:min` , 可选择最大/最小的勾选数量；

  ```ruby
  <el-checkbox-group v-model="checklist" :max="2">
  ```

- 和单选框一样，支持使用 `el-checkbox-button` 样式；

  ```xml
  <el-checkbox-button label="音乐"></el-checkbox-button>
  ```

- 事件和 `radio` 一样，支持 `change` ，具体如下：

  ```csharp
  <el-checkbox-group v-model="checklist" @change="radioChange">
  ```

  PS：其余和单选框类似，注意 `button` 样式和普通样式的属性区别；

### 07. Input 输入框

- 基础 Input 输入框，带双向绑定；

  ```kotlin
  <el-input v-model="input" placeholder="请输入内容"></el-input>
  
  data() {
    return {
        input: ''
    }
  }
  ```

- 使用 `clearable` 属性，提供框内清空按钮；使用 `show-password` 实现密码框；

  ```xml
  <el-input v-model="input" placeholder="请输入内容" clearable ></el-input>
  <el-input v-model="input" placeholder="请输入内容" show-password></el-input>
  ```

- 使用 `maxlength` 属性，设置输入限制；

  ```xml
  <el-input v-model="input" placeholder="请输入内容" maxlength="6" show-word-limit></el-input>
  ```

- 使用 `prefix-icon` 设置前缀内置图标；使用 `suffix-icon` 设置后缀内置图标；

  ```xml
  前置图标：<el-input placeholder="请输入内容" prefix-icon="el-icon-search"></el-input>
  后置图标：<el-input placeholder="请输入内容" suffix-icon="el-icon-edit"></el-input>
  ```

  ps：支持 `slot` 方式，具体如下

  ```xml
  <el-input>
      <i slot="suffix" class="el-input__icon el-icon-date"></i>
  </el-input>
  ```

- 使用 `type="textarea"`，可以将输入框设置成文本域；

  ```xml
  <el-input v-model="input" placeholder="请输入内容" type="textarea"></el-input>
  ```

  ps：使用属性 `autosize` 可自动扩展其高度；

- 使用 ` 可以实现复合型输入库；

  ```xml
  <el-input>
      <template slot="prepend">http://</template>
      <template slot="append">.com</template>
  </el-input>
  ```

  ps：和之前的表单一样可以使用 size 来设置尺寸；

- 可以通过输入库激活或输入的方式匹配数据列表内的相关信息；

  ```ruby
  <el-autocomplete v-model="autocomplete" placeholder="请输入内容" :fetch-suggestions="querySearch"></el-autocomplete>
  ```

   ps:   querySearch 详细见 [Element UI 文档](https://links.jianshu.com/go?to=https%3A%2F%2Felement.eleme.cn%2F%23%2Fzh-CN%2Fcomponent%2Finput%23yuan-cheng-sou-suo)
   ps：事件，输入库有：focus、blur、select；自动完成有：select、change；

### 08. InputNumber

- 基础 InputNumber 计数器，带双向绑定；

  ```kotlin
  <el-input-number v-model="inum"></el-input-number>
  
  data() {
    return {
        inum: 1
    }
  }
  ```

- 使用 `:max` / `:min` 限制最大值和最小值，支持 `change` 事件；

  ```ruby
  <el-input-number v-model="inum" @change="inputChange" :min="1" :max="10"></el-input-number>
  
  inputChange(value) {
     console.log(value);
  }
  ```

   ps：设置 `disabled` 禁用；设置 `:step=1` 为增减步长；设置 `:precision=2` 为小数点精度； 

  ```ruby
  <el-input-number v-model="inum" @change="inputChange" :step='1' :precision='2'></el-input-number>
  ```

- 使用 `controls-position` 可设置增减按钮的位置；

  ```xml
  <el-input-number value="inum" controls-position="right"></el-input-number>
  ```

- 事件支持：`change`、`blur`、`foucs`，方法支持：`focus`、`select`（区别无回调）

### 09. Select 选择器

- 基础 select 选择器：

  ```csharp
  <el-select v-model="value" placeholder="请选择">
        <el-option v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value">
        </el-option>
  </el-select>
  
  data() {
    return {
        value: '',
        options: [
            {
                value: 1,
                label: '北京'
            },{
                value: 2,
                label: '上海'
            },{
                value: 3,
                label: '广州'
            }
        ]
     }
  }
  ```

- 有两种禁用：1. 在 `el-select` 设置； 2. 在 `el-option` 设置；

  ```xml
  <el-select v-model="value" placeholder="请选择" disabled>  
  <!-- 或 -->
  <el-option :disabled="item.value == 1"></el-option>
  ```

- 使用 `clearable` 属性，清空选择的项目；使用 `multiple`，实现多选项目；

  ```csharp
  <el-select v-model="value" placeholder="请选择" clearable multiple >
  ```

- 使用 `filterable` 属性，执行选项搜索；

  ```csharp
  <el-select v-model="value" placeholder="请选择" filterable>
  ```

- 关于对应的属性、事件，基本和之前的类似；

  ```csharp
  <el-select v-model="value" placeholder="请选择" size="mini" @change="change">
  ```

### 10. Cascader 选择器

- 基础 cascader 选择器：

  ```csharp
  <el-cascader :options="options" v-model="value"> </el-cascader>
  
  data() {
      return {
          value: 1,
          options: [{
              value: 1,
              label: "北京",
              children: [{
                value: 11,
                label: '北京二级11'
               },{
                value: 12,
                label: '北京二级12'
               },{
                value: 13,
                label: '北京二级13'
               }]
             }  
          ]
      }
  }
  ```

- 默认是通过 `click` 点击来实现菜单的展开，我们也可以设置为 `hover`；

  ```xml
  <el-cascader v-model="value"
          :options="options"
          :props="{ expandTrigger: 'hover' }">
  </el-cascader>
  ```

- 使用 `disabled` 和 `clearable`，禁用和清空，和 Select 选择器一样；

  ```xml
  <el-cascader v-model="value"
          :options="options" clearable disabled>
  </el-cascader>
  ```

- 使用 `:show-all-levels` 来限制获取到的是最后一级的内容，设置 false 即可；

  ```xml
  <el-cascader v-model="value"
          :options="options"
          :show-all-levels="false">
  </el-cascader>
  ```

- 使用 `:props` 来设置复选框多选；

  ```ruby
  <el-cascader v-model="value"
          :options="options"
          :show-all-levels='false'
          :props="props"></el-cascader>
  
  data() {
    return {
        props: {
           multiple: true
        }
    }
  }
  ```

### 11. Switch 开关

- 基本用法

  ```jsx
  <el-switch v-model="value"
      active-color="#13ce66"
      inactive-color="#ff4949">
  </el-switch>
  
  <script>
    export default {
      data() {
        return {
            value: true
        }
      }
    };
  </script>
  ```

- 除了 Boolean 类型，也可以是 `String` 或 `Number` 类型；

  ```csharp
  <el-switch v-model="value"
        active-color="#13ce66"
        inactive-color="#ccc"
        active-value="100"
        nactive-value="0"
        @change="switchChange"></el-switch>
  
  data() {
    return {
      value: '0',
    };
  },
  
  methods: {
    switchChange(value) {
      console.log(value);
    },
  }
  ```

### 12. Slider 滑块

- 基本用法：

  ```kotlin
  <el-slider v-model="value"></el-slider>
  
  data() {
    return {
        value: 0
    }
  }
  ```

- 使用 `:show-tooltip` 实现隐藏文本提示； `:format-tootip` 实现自定义格式化；

  ```ruby
  <el-slider v-model="value" :show-tooltip="false"></el-slider>
  <el-slider v-model="value1" :format-tooltip="formatTooltip"></el-slider>
  
  methods: {
    formatTooltip(value) {
      return value / 100;
    }
  }
  ```

- 使用 `:step` 实现离散效果，在使用 `show-stops` 实现断点效果；

  ```xml
  <el-slider v-model="value3" :step='10'></el-slider>
  <el-slider v-model="value3" :step='10' show-stops ></el-slider>
  ```

- 使用 `show-input` 可自带输入框；

  ```xml
  <el-slider v-model="value" show-input></el-slider>
  ```

- 使用 `range` 实现范围选择；

  ```ruby
  <el-slider v-model="value" range :min='1' :max='10'></el-slider>
  
  value: [4, 6]
  ```

### 13. TimePicker 时间选择

- 选择一个固定时间点，具体如下：

  ```bash
  <el-time-select v-model="value"
      :picker-options= "{
           start: '08:30',
           step: '00:15',
           end: '18:30'
       }" placeholder="选择时间">
  </el-time-select>
  
  data() {
    return {
      value: ''
    }
  }
  ```

- 选择一个任意时间点，具体如下：

  ```xml
  <el-time-picker v-model="value1" arrow-control
      :picker-options= "{
          selectableRange: '18:00:00 - 20:00:00'
      }"
      placeholder="选择时间">
  </el-time-picker>
  ```

  ps：`arrow-control` 属性可开启箭头选择，而 `selectableRange` 是限制时间范围；

- 丰富的任意时间范围，具体如下：

  ```xml
  <el-time-picker v-model="value2" 
       is-range
       range-separator='至'
       start-placeholder='开始时间'
       end-placeholder='结束时间'>
  </el-time-picker>
  ```

  ps：`is-range` 开启丰富的任意时间范围，其它字面意思；
   ps：属性方法和之前组件类似。

### 14. DatePicker 日期选择

- 选择一个基础的日期，如下：

  ```kotlin
  <el-date-picker v-model="value" placeholder="选择日期"></el-date-picker>
  
  data() {
    return {
      value: ''
    }
  }
  ```

- 可以设置一个 `type` 属性，设置获取的值：

  ```xml
  <el-date-picker v-model="value" type='date' placeholder="选择日期"></el-date-picker>
  ```

  ps：`type` 设置为 `datetime`，可以获取到日期+时间，其它的可参考文档说明；

- 自定义快捷选项的日期选择，具体如下：

  ```ruby
  <el-date-picker v-model="value" type="datetime" placeholder="选择日期" :picker-options="getPicker"></el-date-picker>
  
  data() {
  return {
    value: '',
    // 属性对象名是自定义的
    getPicker: {
        // shortcuts 是固定名称，标识设置快捷栏
        shortcuts: [{
          text: '今天',
          onClick(picker) {
            picker.$emit('pick', new Date());
          }
         }, {
          text: '昨天',
          onClick(picker) {
            const date = new Date();
            date.setTime(date.getTime() - 3600 * 1000 * 24);
            picker.$emit('pick', date);
          }
         }, {
          text: '一周前',
          onClick(picker) {
            const date = new Date();
            date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
            picker.$emit('pick', date);
          }
        }]
      }
    }
  }
  ```

- 选定一个范围日期，具体如下：

  ```xml
  <el-date-picker v-model="value2" 
       type='monthrange'
       range-separator='至'
       start-placeholder='开始月份'
       end-placeholder='结束月份'>
  </el-date-picker>
  ```

- 选定一个日期，并进行格式化，具体如下：

  ```xml
  <el-date-picker v-model="value" 
                  type='datetime'
                  placeholder='选择日期'
                  format="yyyy 年 MM 月 dd 日"
                  :picker-options="getPicker">
  </el-date-picker>
  ```

### 15. Form

#### 1.form

`model`:表单的数据对象， 

```
<el-form :model="form" label-width="80px">
</el-form>
```

` label-width `:表单域标签的宽度

`rules`: 表单的验证规则

` inline `:行内表单模式

#### 2.form-item

`prop`:表单域 model 字段，在使用 validate、resetFields 方法的情况下，该属性是必填的 ， 传入 Form 组件的 `model` 中的字段 

`label`:标签文本

```vue
<el-form-item label="用户名" prop="username">
    <el-input v-model="form.username"></el-input>
</el-form-item>

<el-form-item label="密码" prop="password">
    <el-input type="password" v-model="form.password"></el-input>
</el-form-item>
```



### 16. Card卡片

 通过`shadow`属性设置卡片阴影出现的时机：`always`、`hover`或`never`。 

```
<el-card shadow="hover"><el-card>
```

### 17. NavMenu 导航菜单

最外层

```vue
<el-menu
	class="el-menu-vertical-demo"
    background-color="#232d3b" //直接设置菜单背景色
    text-color="#c0cad9" //设置字体色
    @open="handleOpen"
    @close="handleClose"
>
```

没有子级的菜单`menu-item`

```vue
<el-menu-item
	index="menu.path"
    v-for="menu in noChildren"
    :key="menu.path"
>
    <i :class="`el-icon-${menu.icon}`"></i>
    <span slot="title">{{ menu.label }}</span>
</el-menu-item>
```

有子级的菜单`sub-menu`

```vue
<el-submenu
	index="menu.path"
	v-for="menu in hasChildren"
	:key="menu.path"
>
    //父级菜单
    <template slot="title">
		<i :class="`el-icon-${menu.icon}`"></i>
		<span>{{ menu.label }}</span>
    </template>
    //子级菜单
    <el-menu-item
		index="menuitem.path"
		v-for="menuitem in menu.children"
		:key="menuitem.path"
    >
		<i :class="`el-icon-${menuitem.icon}`"></i>
        <span>{{ menuitem.label }}</span>
    </el-menu-item>
</el-submenu>
```

open和close：`sub-menu`展开和关闭的回调	

参数：`index`: 打开的 sub-menu 的 index， 

​				`indexPath`: 打开的 sub-menu 的 index path

```js
handleOpen (key, keyPath) {
    console.log(key, keyPath)
},
handleClose (key, keyPath) {
	console.log(key, keyPath)
},
```







































