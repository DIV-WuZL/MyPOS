<template>
  <div id="leftNav">
    <ul>
      <li>
        <i class="icon iconfont icon-goumai"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-31dianpu"></i>
        <div>店铺</div>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <div>商品</div>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <div>会员</div>
      </li>
      <li>
        <i class="icon iconfont icon-31paixingbang"></i>
        <div>统计</div>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <div>设置</div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
    data() {
      return {
        
      }
    },
}
</script>

<style>
  #leftNav{
    color: #fff;
    font-size: 10px;
    height: 100%;
    width: 4%;
    background-color: #1D8CE0;
    float: left;
  }
  .iconfont{
    font-size: 24px;
  }
  #leftNav ul{
    margin: 0;
    padding: 0;
  }
  #leftNav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #ffffff;
    padding: 10px;
  }
  
</style>