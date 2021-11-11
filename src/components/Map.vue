<template>
    <div class="map">
        <h3>Карта офиса</h3>

        <div
            v-if="!isLoading"
            class="map-root"
        >
            <MapSVG 
                @click="clickOutsideTable"
                ref="officeMap" 
            />
            <TableSVG 
                v-show="false" 
                ref="officeTable" 
            />

        </div>
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import MapSVG from '@/assets/images/map.svg';
import TableSVG from '@/assets/images/workPlace.svg';
import d3 from '@/assets/vendors/D3';

export default {
    props: {
        tables: {
            type: Array,
            default: () => ([])
        },
        groups: {
            type: Array,
            default: () => ([])
        },
    },
    data() {
        return {
            isLoading: false,
            map: null,
            table: null
        };
    },
    components: {
        MapSVG,
        TableSVG,
    },
    methods: {
        initializeOfficeMap() {
            this.map = d3.select(this.$refs.officeMap);
            this.officeGroup = this.map.select('g');
            this.table = d3.select(this.$refs.officeTable);

            if(!this.officeGroup) {
                console.error('Element group is missing');
                return false;
            }

            this.drawTables();
        },
        drawTables() {
            const tablesGroup = this
                .officeGroup.append('g')
                .classed('tables', true);

            this.tables.forEach((table) => {
                tablesGroup
                    .append('g')
                    .attr(
                        'transform', 
                            `translate(${table.x} ${table.y}) scale(0.5) rotate(${table.rotate || 0})`
                    )
                    .attr('id', table._id)
                    .attr('group_id', table.group_id)
                    .html(this.table.html())
                    .attr(
                        'fill',
                        this.groups.find((it) => it.group_id === table.group_id)?.color 
                        ?? 'transparent')

                    .classed('place', true)
                    .append('rect')
                    .style('visibility', 'hidden')
                    .attr('x', 0)
                    .attr('y', 0)
                    .attr('width', 14)
                    .attr('height', 14)
                    .on('click', (evt) => this.getPeople(evt, table._id));
                
            });
        },
        getPeople($event, id) {
            $event.stopPropagation();
            this.$emit('clickTable', id);
        },
        clickOutsideTable() {
            this.$emit('clickOutsideTable')
        }
    },
    watch: {
        tables() {
            this.drawTables();
        }
    },
    mounted() {
        this.initializeOfficeMap();
    }
};
</script>

<style scoped>
.map {
    height: 100%;
    width: 100%;
    padding: 24px;
    overflow: hidden;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.map-root {
    height: 100%;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

::v-deep svg {
    height: 100%;
    width: 100%;
}

::v-deep .table {
    cursor: pointer; 
}

svg g {
    pointer-events: all;
    width: min-content;
}
</style>
