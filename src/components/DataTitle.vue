<template>
    <div class="text-center">
        <h2 class="text-3xl font-bold">
            <!-- {{}} = Prop passed in for "text" per below under script-->
            {{ text }}
        </h2>
        <div class="text-2xl mt-4 mb-10">
            <!-- {{}} = Prop passed in for "timestamp" per below under script-->
            {{ timestamp }}
        </div>
    </div>
</template>

<script>
import moment from 'moment';

    export default {
        name: 'DataTitle',
        props: ['text', 'dataDate'],
        computed: {
            // 'timestamp' is a field
            timestamp: function() {
                return moment(this.dataDate).format('MMMM Do YYYY, h:mm:ss a')
            }
        },
    }
</script>