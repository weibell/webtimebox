<template>
    <div id="timebox">
        <div v-if="secondsUntilStart >= 0" class="pending">
            −{{formatTime(secondsUntilStart)}}
        </div>

        <div v-else-if="secondsSinceEnd >= 0" class="time-up">
            +{{formatTime(secondsSinceEnd)}}
        </div>

        <div v-else class="in-progress">
            {{formatTime(secondsUntilEnd)}}
        </div>

    </div>
</template>

<script>
    export default {
        name: "Timebox",

        props: {
            start: Number,
            end: Number
        },

        data() {
            return {
                now: new Date().getTime()
            }
        },

        computed: {

            secondsUntilStart() {
                return Math.floor((this.start - this.now) / 1000)
            },

            secondsSinceEnd() {
                return Math.floor((this.now - this.end) / 1000)
            },

            secondsUntilEnd() {
                return Math.floor((this.end - this.now) / 1000)
            }
        },

        created() {
            setInterval(() => {
                this.now = new Date().getTime()
            }, 1000)
        },

        methods: {
            formatTime(seconds) {
                const zeroPad = number => number <= 9 ? "0" + number : number

                let output = ""
                const hours = Math.floor(seconds / 3600)
                if (hours > 0) {
                    output += zeroPad(hours) + ":"
                    seconds -= hours * 3600
                }
                const minutes = Math.floor(seconds / 60)
                output += zeroPad(minutes) + ":"

                seconds -= minutes * 60
                output += zeroPad(seconds)

                return output
            }
        }
    }
</script>

<style scoped>

    #timebox {
        font-family: monospace;
        text-align: center;
        margin-top: calc(50vh - 30px);
        font-size: 50px;
    }

    .pending {
        color: gray;
    }

    .in-progress {
        color: darkorange;
    }

    .time-up {
        color: red;
    }
</style>