<template>
    <div id="app">
        <div class="office">
            <Map 
                @clickTable="onClickTable" 
                @clickOutsideTable="onClickOutsideTable"
                :tables="tables"
                :groups="groups"
            />
            <SideMenu 
                :is-user-openned.sync="isUserOpenned"
                :person="person" 
            />
        </div>
        <Form 
            v-if="freeTablesLeft > 0"
            :groups="groups" 
            @submit="onSubmit" 
        />
    </div>
</template>

<script>
import Map from './components/Map.vue';
import SideMenu from './components/SideMenu.vue';
import people from '@/assets/data/people.json';
import legend from '@/assets/data/legend.json';
import tables from '@/assets/data/tables.json';
import Form from '@/components/Form/Form.vue';

export default {
    name: 'App',
    components: {
        Map,
        SideMenu,
        Form,
    },
    data() {
        return {
            person: {},
            isUserOpenned: false,
            groups: legend,
            people,
            tables,
            coordsFreeTables: [{
                x: 44,
                y: 13
            },
            {
                x: 90,
                y: 37
            }],
            freeTablesLeft: 2,
        }
    },
    methods: {
        onClickTable(id) {
            this.isUserOpenned = true;
            this.person = people.find((val) => val.tableId === id);
        },
        onClickOutsideTable() {
            this.isUserOpenned = false;
        },
        onSubmit({ user, userGroup }) {
            this.freeTablesLeft = this.freeTablesLeft - 1;
            const {x, y} = this.coordsFreeTables[this.freeTablesLeft];
            const userId = this.people.length + 1;
            const tableId = this.tables.length + 1;

            this.people.push({
                ...user,
                _id: userId,
                tableId

            });
            
            this.tables.push({
                '_id': tableId,
                'x': x,
                'y': y,
                'group_id': userGroup,
                'rotate': 80
            })
        }
    }
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
    background-color: #fafafa;
    padding: 24px;
    box-sizing: border-box;
}

html,
body,
#app {
    height: 100%;
}

* {
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

.office {
    display: grid;
    grid-template-columns: 1fr 320px;
    border-radius: 6px;
    border: 1px solid #ccd8e4;
    height: 100%;
    background: white;
    max-width: 1500px;
    margin: 0 auto;
}
</style>
