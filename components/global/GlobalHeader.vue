<template lang="pug">
    div(class="bg-red-mid h-screen text-white")
        // Navigation section
        // ----------------------------------------------------------------------------------------
        nav(class="flex justify-center p-4")
            p(class="font-bold text-2xl") PomoTimer

        // Timer section
        // ----------------------------------------------------------------------------------------
        aside(class="flex flex-col gap-4 items-center justify-center glass-bg m-4 p-4 rounded-md")
            // Tabs
            // ------------------------------------------------------------------------------------
            div(class="flex items-center justify-center")
                p(
                    @click="setTime('pomodoro')"
                    :class="{'active-tab': activeTab === 'pomodoro'}"
                    class="cursor-pointer p-4"
                ) Pomodoro

                p(
                    @click="setTime('short')"
                    :class="{'active-tab': activeTab === 'short'}"
                    class="cursor-pointer p-4"
                ) Short break

                p(
                    @click="setTime('long')"
                    :class="{'active-tab': activeTab === 'long'}"
                    class="cursor-pointer p-4"
                ) Long break

            // Timer
            // ------------------------------------------------------------------------------------
            p(
                class="font-bold mb-4 text-center text-8xl"
            ) {{ minutesRewrite }}:{{ secondsRewrite }}

            button(
                @click="startStopTimer"
                class="bg-white capitalize rounded-md text-4xl text-black w-64 p-4"
            ) {{ hasStarted ? "STOP" : "START" }}

</template>

<script>
    export default {
        name: "GlobalHeader",
        data() {
            return {
                activeTab: "pomodoro",
                hasStarted: false,
                minutes: 0,
                seconds: 10,
                timerIntervalRef: null,
            }
        },
        computed: {
            minutesRewrite() {
                let output = this.minutes;

                if (this.minutes < 10) output = `0${this.minutes}`;

                return output;
            },
            secondsRewrite() {
                let output = this.seconds;

                if (this.seconds === 0) {
                    output = `${this.seconds}0`;
                    // ----------------------------------------------------------------------------
                } else if (this.seconds < 10 && this.seconds > 0) {
                    output = `0${this.seconds}`;
                }

                return output;
            }
        },
        methods: {
            decrementTime() {
                if (this.seconds === 0) {
                    this.seconds = 59;

                    if (this.minutes !== 0) {
                        this.minutes--;
                    } else {
                        this.resetTimer();
                    }
                } else {
                    this.seconds--;
                }
            },
            resetTimer() {
                // Timer is done, so we stop the timer
                // ------------------------------------------------------------------------
                clearInterval(this.timerIntervalRef);

                // Reinitialise variables
                // ------------------------------------------------------------------------
                this.hasStarted = false;
                this.minutes = 25;
                this.seconds = 0;
                this.activeTab = "pomodoro";
            },
            setTime(value) {
                this.activeTab = value;
                this.seconds = 0;

                if (value === "pomodoro") {
                    this.minutes = 25;
                } else if (value === "short") {
                    // ----------------------------------------------------------------------------
                    this.minutes = 5;
                } else if (value === "long") {
                    // ----------------------------------------------------------------------------
                    this.minutes = 10;
                }
            },
            startStopTimer() {
                // Stop the timer if its currently running
                // --------------------------------------------------------------------------------
                if (this.hasStarted) {
                    clearInterval(this.timerIntervalRef);
                } else {
                    this.timerIntervalRef = setInterval(this.decrementTime, 1000);
                }
            }
        }
    }
</script>

<style lang="sass" scoped>
    .active-tab
        @apply bg-red-mid box-border rounded-md

    .glass-bg
        background: rgba( 255, 255, 255, 0.25 )
        backdrop-filter: blur( 0px )
        -webkit-backdrop-filter: blur( 0px )
        border-radius: 10px
</style>
