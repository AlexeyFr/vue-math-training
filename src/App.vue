<template>
    <div class="training">
        <h1>Math training Level {{ level + 1 }}</h1>
        <hr>
        <div class="progress">
          <div class="progress-bar" :style="progresStyles"></div>
        </div>
        <div class="box">
            <transition name="flip" mode="out-in">

                <app-start-screen
                    v-if="state == 'start'"
                    @onStart="onStart">
                </app-start-screen>

                <app-question
                    v-else-if="state == 'question'"
                    @success="onQuestionSuccess"
                    @error="onQuestionError"
                    :settings="levels[level]">
                </app-question>

                <app-message
                    v-else-if="state == 'message'"
                    :type="message.type"
                    :text="message.text"
                    @onNext="onNext">
                </app-message>

                <app-result-screen
                    v-else-if="state == 'result'"
                    :stats="stats"
                    :buttonNextLevel="buttonNextLevel"
                    @repeat="onStart"
                    @nextLevel="onNextLevel"
                    @start="start">
                </app-result-screen>

                <div v-else>Unknown state</div>

            </transition>
        </div>
</div>
</template>

<script>
    export default {
        data () {
            return {
                state: 'start',
                stats: {
                    success: 0,
                    error: 0
                },
                message: {
                    type: '',
                    text: ''
                },
                level: 0,
                levels: [
                    {
                        questMax: 2,
                        from: 10,
                        to: 40,
                        range: 5,
                        variants: 2
                    },
                    {
                        questMax: 3,
                        from: 100,
                        to: 200,
                        range: 10,
                        variants: 4
                    },
                    {
                        questMax: 4,
                        from: 200,
                        to: 400,
                        range: 15,
                        variants: 6
                    },
                    {
                        questMax: 5,
                        from: 300,
                        to: 600,
                        range: 20,
                        variants: 8
                    },
                    {
                        questMax: 6,
                        from: 400,
                        to: 800,
                        range: 25,
                        variants: 10
                    }
                ]
            }
        },
        computed: {
            questDone(){
                return this.stats.success + this.stats.error;
            },
            progresStyles() {
                return {
                    width: (this.questDone / this.levels[this.level].questMax * 100) + '%'
                }
            },
            buttonNextLevel(){
                if (this.level == this.levels.length - 1) {
                    return false;
                }
                else {
                    return true;
                }
            }
        },
        methods: {
            onStart(){
                this.state = 'question';
                this.stats.success = 0;
                this.stats.error = 0;
            },
            onQuestionSuccess(){
                this.state = 'message';
                this.message.text = 'Good Job!';
                this.message.type = 'success';
                this.stats.success++;
            },
            onQuestionError(msg){
                this.state = 'message';
                this.message.text = msg;
                this.message.type = 'warning';
                this.stats.error++;
            },
            onNext(){
                if (this.questDone < this.levels[this.level].questMax) {
                    this.state = 'question';
                }
                else {
                    this.state = 'result';
                }
            },
            onNextLevel(){
                this.level++;
                this.onStart();
            },
            start(){
                this.level = 0;
                this.onStart();
            }
        }
    }

</script>

<style scoped>
    .training {
        max-width: 700px;
        margin: 20px auto;
    }
    .progress-bar {
        transition: width 0.5s ease-out;
    }
    .box {
        margin: 10px 0;
    }
    .flip-enter-active {
      animation: flipInX 0.3s linear;
    }
    .flip-leave-to {
      animation: flipOutX 0.3s linear;
    }
    @keyframes flipInX {
        from {
            transform: rotateX(90deg);
        }
        to {
            transform: rotateX(0deg);
        }
    }
    @keyframes flipOutX {
        from {
            transform: rotateX(0deg);
        }
        to {
            transform: rotateX(90deg);
        }
    }
</style>