<template>
    <div class="qTemplate--photo">
        <div class="question">
            <h2 class="photo__question">{{question.text}}</h2>
            <div class="answers">
                <cmp-answer
                        v-for="(i,index) in shuffledAnswers"
                        :answerText="i.toString()"
                        :answerNumber="index"
                        :key="index"
                        @choose="picked(index)"
                />
            </div>
        </div>
        <div class="photo">
            <img :src="question.photo.url" alt="question.text" class="photo__img--right"/>
        </div>
    </div>
</template>

<script>
  import cmpAnswer from '@/components/Answer.vue';
  export default {
    name: 'QuestionTemplate_RightPhoto',
    props:{
      question:{
        type: Object,
        required: true
      }
    },
    data(){
      return {
        shuffledAnswers: [],
        properId: 0
      };
    },
    mounted(){
      const properAnswer = this.question.answears[0];
      this.shuffledAnswers = this.shuffle(Object.values(this.question.answears));
      this.properId = this.shuffledAnswers.findIndex(v => v === properAnswer);
    },
    methods:{
      shuffle(a) {
        for (let i = a.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1));
          [a[i], a[j]] = [a[j], a[i]];
        }
        return a;
      },
      picked(index){
        const realId = Object.values(this.question.answears)
        .findIndex(v => v === this.shuffledAnswers[index]);

        this.$emit('choose', realId);
      }
    },
    components:{
      cmpAnswer
    }
  };
</script>

<style lang="scss" scoped>
    @import '@/assets/global.scss';

    .qTemplate--photo{
        width:90%;
        margin:0 auto;
        padding: 60px 0;
        display:flex;
        flex-wrap:wrap;
    }

    .photo__question{
        font-size: 1.6em;
    }

    .photo__img--right{
        max-width:100%;
    }

    div.photo{
        order:1;
    }
    div.question{
        order:2;
    }

    @include for-size(tablets){
        .qTemplate--photo{
            flex-wrap: nowrap;
            div.question{
                order:0;
            }
            > div{
                width:50%;
                &:first-of-type{
                    margin-right:30px;
                }
            }
        }
    }
</style>