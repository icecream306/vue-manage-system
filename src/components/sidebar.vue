<template>
    <div class="sidebar">
        <el-menu
            class="sidebar-el-menu"
            :default-active="onRoutes"
            :collapse="sidebar.collapse"
            background-color="#324157"
            text-color="#bfcbd9"
            active-text-color="#20a0ff"
            unique-opened
            router
        >
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-sub-menu :index="item.index" :key="item.index" v-permiss="item.permiss">
                        <template #title>
                            <el-icon>
                                <component :is="item.icon"></component>
                            </el-icon>
                            <span>{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-sub-menu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                                v-permiss="item.permiss"
                            >
                                <template #title>{{ subItem.title }}</template>
                                <el-menu-item v-for="(threeItem, i) in subItem.subs" :key="i" :index="threeItem.index">
                                    {{ threeItem.title }}
                                </el-menu-item>
                            </el-sub-menu>
                            <el-menu-item v-else :index="subItem.index" v-permiss="item.permiss">
                                {{ subItem.title }}
                            </el-menu-item>
                        </template>
                    </el-sub-menu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index" v-permiss="item.permiss">
                        <el-icon>
                            <component :is="item.icon"></component>
                        </el-icon>
                        <template #title>{{ item.title }}</template>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { useSidebarStore } from '../store/sidebar';
import { useRoute } from 'vue-router';

const items = [
    {
        icon: 'Odometer',
        index: '/dashboard',
        title: '系统首页',
        permiss: '1',
    },
    {
        icon: 'Calendar',
        index: '1',
        title: '交易系统',
        permiss: '2',
        subs: [
            {
                index: '/table',
                title: '货权管理',
                permiss: '2',
            },
            {
                index: '/export',
                title: '资产交易',
                permiss: '2',
            },
            {
                index: '/detail',
                title: '货权详情',
                permiss: '2',
            },
        ],
    },
    {
        icon: 'DocumentCopy',
        index: '/scan',
        title: '区块浏览器',
        permiss: '3',
    },
    {
        icon: 'Edit',
        index: '3',
        title: '表单相关',
        permiss: '4',
        subs: [
            {
                index: '/form',
                title: '基本表单',
                permiss: '5',
            },
            {
                index: '/upload',
                title: '文件上传',
                permiss: '6',
            },
            {
                index: '4',
                title: '三级菜单',
                permiss: '7',
                subs: [
                    {
                        index: '/editor',
                        title: '富文本编辑器',
                        permiss: '8',
                    },
                    {
                        index: '/markdown',
                        title: 'markdown编辑器',
                        permiss: '9',
                    },
                ],
            },
        ],
    },
    {
        icon: 'Setting',
        index: '/icon',
        title: '自定义图标',
        permiss: '10',
    },
    {
        icon: 'PieChart',
        index: '/charts',
        title: 'schart图表',
        permiss: '11',
    },
    {
        icon: 'Operation',
        index: '6',
        title: '系统管理',
        permiss: '13',
        subs: [
            {
                index:'/userManage',
                title:'用户管理',
                permiss:'13'
            },
            {
                index:'/permission',
                title:'权限管理',
                permiss:'13'
            }
        ]
    },
    {
        icon: 'Setting',
        index: '7',
        title: '加工平台',
        permiss: '15',
        subs: [
            {
                index: '/processing',
                title: '加工管理',
                permiss: '15',
            },
            {
                index: 'processingMarket',
                title: '加工市场',
                permiss: '15',
            },
            {
                index: 'processingOwned',
                title: '我的加工',
                permiss: '15',
            }
        ]
    },
    {
        icon: 'CoffeeCup',
        index: '/donate',
        title: '支持作者',
        permiss: '14',
    },
    {
        icon: 'CoffeeCup',
        index: '5',
        title: '贷款管理',
        permiss: '15',
        subs: [
            {
                index: '/loans',
                title: '贷款查看',
                permiss:'15',
            },
            {
                index: '/loansReview',
                title: '贷款审核',
                permiss:'15',
            },
        ]
    },
    {
        icon: 'CoffeeCup',
        index: '/insurance',
        title: '保险管理',
        permiss: '16', 
    },
    {
        icon: 'CoffeeCup',
        index: '/express',
        title: '物流管理',
        permiss: '16',
    }
    
];

const route = useRoute();
const onRoutes = computed(() => {
    return route.path;
});

const sidebar = useSidebarStore();
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}
</style>
