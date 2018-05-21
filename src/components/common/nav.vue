<template>
    <div class="nav">
        <ul>
            <li>
                <i class="icon iconfont icon-goumai"></i>
                <p>收银</p>
            </li>
            <li>
                <i class="icon iconfont icon-dianpu"></i>
                <p>店铺</p>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <p>商品</p>
            </li>
            <li>
                <i class="icon iconfont icon-31huiyuanqia"></i>
                <p>会员</p>
            </li>
            <li>
                <i class="icon iconfont icon-xiugaioryijian"></i>
                <p>统计</p>
            </li>
            <li>
                <i class="icon iconfont icon-gongnengjianyi"></i>
                <p>设置</p>
            </li>
        </ul>
    </div>
</template>

<script>
export default {};
</script>

<style>
.nav{
    color: #fff;
    font-size: 10px;
    height: 100%;
    background-color: #1d8ce0;
    float: left;
    width: 100%;
}
.iconfont{
    font-size: 25px;
}
.nav ul{
    padding: 0;
    margin: 0;
}
.nav ul li{
    display: flex;
    flex-direction: column;
    justify-content: center;
    list-style: none;
    text-align: center;
    height: 70px;
    border-bottom: 1px solid #fff;
    cursor: pointer;
}
.nav ul li p{
    margin: 0;
}
</style>