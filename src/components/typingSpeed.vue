<template>
    <div class="content">
        <div class="message">
            <p><i class="el-icon-timer"></i>时间:{{minute}}:{{second}}</p>
            <p><i class="el-icon-stopwatch"></i>速度:{{speed}}wpm</p>
            <p><i class="el-icon-pie-chart"></i>正确率:{{rightRate}}%</p>
        </div>
        <div class="answers" id="sid">
            <div class="rows" v-for="(item,index) in contents" :key="item.index">
                <div class="row-item">
                        <span v-for="(itemm,indexx) in item.question" :key="indexx">
                            <span v-if="item.error.includes(indexx)" style="color: red">{{itemm}}</span>
                            <span v-else style="color: black">{{itemm}}</span>
                        </span>
                </div>
                <el-input v-model="item.answer" class="input" @input="beginTime(index)" ref="name"/>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "typingSpeed",
        data(){
            return{
                answer: '',
                content: "行走在人生旅途，我们触摸的是多棱的生活，无须为艰难哀叹，为挫败悲语；不必为虚无动情，为沉沦失声。命运总有幸运与不幸，勿被顺境捆着了脚步，勿被厄运颓废了精神。其实，站得越高，你的阴影越长；越是暗夜，就越接近光明。只要不停地校正前行的方向，我们就不会迷失自己。",
                contents:[],
                rightRate: "0",
                minute: "00",
                second: "00",
                speed: 0,
                oldAnswer: "",
                allWord: 0,
                begin: false,
                currentLine:0
            }
        },
        methods: {
            //开始计时
            beginTime(index) {
                this.makeAnswer()
                if (this.begin == false) {
                    this.updateTime(0)
                }
                this.begin = true
                this.getRate()
                this.currentLine = index
                if(this.contents[this.currentLine].answer.length >= this.contents[this.currentLine].question.length && this.currentLine<this.contents.length-1){
                    this.$refs.name[this.currentLine+1].$refs.input.focus()
                    this.currentLine++
                }
            },
            //拼接答案
            makeAnswer(){
                let temp = ""
                for (let i in this.contents){
                    temp+=this.contents[i].answer
                }
                this.answer = temp
            },
            //计时器
            updateTime(msec) {
                if(this.answer==this.content){
                    return null;
                }
                let minute = parseInt(msec / 60 % 60)
                let second = parseInt(msec % 60)
                this.minute = minute > 9 ? minute : '0' + minute
                this.second = second > 9 ? second : '0' + second
                var that = this
                setTimeout(function () {
                    that.updateTime(msec + 1)
                }, 1000)
                if (msec == 0) {
                    this.speed = parseInt(this.allWord * 60 / 1)
                } else {
                    this.speed = parseInt(this.allWord * 60 / msec)
                }

            },
            //处理content，进行拆分
            make(){
                let div = document.getElementById('sid')
                let width = div.style.width || div.clientWidth || div.offsetWidth || div.scrollWidth
                let num = parseInt((width-30)/25.6)
                let i = 0
                while(i<this.content.length){
                    let temp = {
                        question: this.content.substring(i,i+num),
                        answer:"",
                        error:[],
                    }
                    i=i+num
                    this.contents.push(temp)
                }
            },
            //求正确率
            getRate() {
                if(this.answer.length > this.oldAnswer.length){
                    this.allWord = this.allWord + this.answer.length - this.oldAnswer.length
                }
                this.rightRate = parseInt((this.answer.length - this.compare(this.answer, this.content).errnum) / this.allWord * 100)
                this.oldAnswer = this.answer
                let temp = this.compare(this.answer, this.content).error
                let k = 0
                for(let i in this.contents){
                    this.contents[i].error=[]
                    for(let j in temp){
                        if(temp[j]<this.contents[i].question.length+k && temp[j]>=k){
                            this.contents[i].error.push(temp[j]-k)
                        }
                    }
                    k+=this.contents[i].question.length
                }
            },
            //对比xy字符串
            compare (x,y) {
                let s = 0, z = 0;
                let err = []
                let tmperr=0
                if(x.length > y.length){
                    tmperr = x.length - y.length
                    s = y.length
                    x = x.substring(0,s)
                }else if(x.length < y.length){
                    s = x.length
                    y = y.substring(0,s)
                }else{
                    s = y.length
                }
                x = x.split('');
                y = y.split('');
                for(let i = 0; i < s; i++){
                    if(x[i] == y[i]){
                        z++;
                    }else{
                        err.push(i)
                    }
                }
                let temp = {
                    errnum:s-z+tmperr,
                    error:err
                }
                return temp;
            }
        },
        mounted() {
            this.make()
        },
    }
</script>

<style scoped>
    .content {
        margin: 30px 30px 20px 30px;
        text-align: center;
        height: 80vh;
        padding: 5px 150px;
    }

    .message {
        background-color: #dedad9;
        height: 10%;
        font-size: 1.5em;
        font-weight: bolder;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 80px;
        box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1)
    }

    .message i {
        margin: 0 8px;
    }

    .answers {
        height: 90%;
    }

    .rows {
        height: 17%;
        margin: 0.5% 0;
    }

    .row-item {
        height: 50%;
        background-color: #f2eee2;
        font-size: 1.6em;
        display: flex;
        align-items: center;
        text-align: center;
        justify-items: center;
        padding: 0 15px;
    }

    .input {
        font-size: 1.6em;
    }

</style>
