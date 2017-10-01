<template>
    <city :hotCity="hotCity" v-on:selected="selectedCity"></city>
</template>

<script>
    import city from '../components/city.vue';

    export default {
        data() {
            return {
                hotCity: [
                    {
                        name: '乌镇', //选择城市页面显示用
                        sname: '',    //景点列表匹配搜索结果用
                        sid: '',      //景点列表匹配搜索结果用
                        cname: '乌镇' //首页显示用、查找景点列表用
                    }, {
                        name: '上海',
                        sname: '',
                        sid: '',
                        cname: '上海'
                    }, {
                        name: '杭州',
                        sname: '',
                        sid: '',
                        cname: '杭州'
                    }, {
                        name: '成都',
                        sname: '',
                        sid: '',
                        cname: '成都'
                    }, {
                        name: '丽江',
                        sname: '',
                        sid: '',
                        cname: '丽江'
                    }, {
                        name: '广州',
                        sname: '',
                        sid: '',
                        cname: '广州'
                    }, {
                        name: '大理',
                        sname: '',
                        sid: '',
                        cname: '大理'
                    }, {
                        name: '长沙',
                        sname: '',
                        sid: '',
                        cname: '长沙'
                    }, {
                        name: '九寨沟',
                        sname: '九寨沟',
                        sid: '506',
                        cname: '九寨沟县',
                    }, {
                        name: '凤凰',
                        sname: '凤凰古城',
                        sid: '1503',
                        cname: '凤凰县',
                    }, {
                        name: '泸沽湖',
                        sname: '丽江泸沽湖',
                        sid: '3634',
                        cname: '丽江',
                    }
                ]
            }
        },
        components: {
            city
        },
        methods: {
            selectedCity(city){
                this.$store.commit('setCityName', city)
                this.$router.go(-1);
            }
        }
    }

</script>

<style lang="less" scoped>
    .page-view{
        height: 100%;
    }
</style>
