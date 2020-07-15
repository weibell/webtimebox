<template>
    <div id="timebox">
        <div v-if="secondsUntilStart > 0" class="pending">
            {{displayedTime}}
        </div>

        <div v-else-if="secondsSinceEnd > 0" class="time-up">
            {{displayedTime}}
        </div>

        <div v-else class="in-progress">
            {{displayedTime}}
        </div>

    </div>
</template>

<script>
    import Piecon from 'piecon'

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
            },

            fractionFinished() {
                return (this.now - this.start) / (this.end - this.start)
            },

            displayedTime() {
                if (this.secondsUntilStart > 0) {
                    return "−" + this.formatTime(this.secondsUntilStart)
                } else if (this.secondsSinceEnd > 0) {
                    return "+" + this.formatTime(this.secondsSinceEnd)
                } else {
                    return this.formatTime(this.secondsUntilEnd)
                }
            }
        },

        created() {
            Piecon.setOptions({
                color: '#f00',
                background: '#f0f0f0',
                shadow: '#f0f0f0'
            });

            this.update()
            setInterval(this.update, 1000)
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
            },
            update() {
                this.now = new Date().getTime()
                document.title = this.displayedTime
                Piecon.setProgress(this.fractionFinished * 100)
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