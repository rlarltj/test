<template>
<div>
    <h1 style="color:#05499f; margin-top:48px">“내가 필요하면, 친구도 필요하다!”</h1>
    <div style="line-height:190%">
        <h3>냥디즈(NYANGdiz)에 방문해주신 여러분께 감사 인사드립니다😎 <br>
        냥디즈(NYANGdiz)는 교내에서 필요한 공동 구매 및 해외 직구에 대한 펀딩을 개설할 수 있도록 한양대학교 학우 여러분을 연결하는 사이트입니다.<br>
        이곳에서 판매자는 구매에 필요한 사람들을 모을 수 있고, 소비자는 양질의 제품을 더 저렴한 가격에 구매할 수 있습니다.<br>

        운영진의 목적은 단 하나! 여러분들이 원하는 제품을 합리적인 가격에 구매할 수 있도록 돕는 것입니다.<br>

        한양대학교 학우 여러분들과 함께 만들어가는 냥디즈(NYANGdiz)🐾 많은 이용 부탁드립니다~💙</h3>
    </div>
    <router-link to="shop" @click="$emit('makeTrue', true)"><button>상품 보러가기</button></router-link>
</div>
    
</template>

<script>
export default{
    
}
</script>