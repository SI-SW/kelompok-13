<script>
import { mapActions, mapState } from 'pinia';
import d$todo from '@/stores/dashboard/todo';

export default{
    nama: 'ToDo',
    computed: {
        ...mapActions(d$todo, ['g$list']),
    },
    methods: {
        ...mapActions(d$todo, ['a$list']),
        async getList() {
            try {
                await this.a$list();
            } catch (e) {
                console.error('method getList error', e);
            }
        }
    },
    async created(){
        await this.getList();
    },
};
</script>
<template>
<div>
    <ol>
        <li v-for="(item, index) in g$list" :key="index">{{ item }}</li>
    </ol>
</div>
</template>