 <template>
    <div class="hello">
        <div class="top_box">
            <span class="logo">校宝在线</span>
            <span class="headcp"><img src="../images/hs1.jpg" width="30px" height="30px"></span>
            <span class="other">|</span>
            <span class="mywork">我的任务</span>
        </div>
        <br>
        <br>
        <Tabs type="card"  class="tabs">
            <Tab-pane label="调研1">
                <div class="survey">
                    <div id="title">
                        <h3>新高考</h3>
                        <button v-bind:class="{state:isActive ,normal:normal}">{{state}}</button>
                    </div>
                    <div class="timeholder">
                        <p id="time">{{starttime}} 至 {{endtime}}</p>
                    </div>
                    <div class="explainholder">
                        <p id="explain">这里是选课说明，很长很长的选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明明选课说明选课说明选课说明选课说明选课说明选课说明选课说明选课说明 </p>
                    </div>
                    <br>
                    <hr>
                    <div class="quesHolder">
                        <ul v-for="(item,index) in lists" :key="index">
                            <li class="quesOne">
                                <p>Q{{item.subNum}}:{{item.title}}（最多选择{{item.max}}个选项）</p>
                            </li>
                            <Checkbox-group v-model="answers[index]" @on-change="checkAllGroupChange(item,index,item.choice,cItem)" v-for="(cItem,cIndex) in item.choice" :key="cIndex" :single="bool">
                                <Checkbox :label=cItem.content>{{cItem.content}}</Checkbox>
                            </Checkbox-group>
                        </ul>
                    </div>
                    <Spin>
                        <div v-bind:class="{'mine-spin-loading':spinShow} "></div>
                    </Spin>
                    <div>
                        <Button class="submit" type="primary" @click="submit(lists)">提交</Button>
                    </div>
                </div>
            </Tab-pane>
            <Tab-pane label="调研2">标签二的内容</Tab-pane>
            <Tab-pane label="调研3">标签三的内容</Tab-pane>
        </Tabs>

</div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data() {
        return {
            bool: '', //标记题目属性，是多选还是单选
            answers: [[], [], []], //存放用户选择的答案，每一题的答案放在一个子数组中
            starttime: '2018-08-11 12:00',
            endtime: '2018-08-22 12:00',
            state: '', //标记问卷状态，是进行中还是结束
            isActive: false, //标记状态类名，用来修改样式
            normal: true,
            spinShow: false,
            lists: [
                {
                    subNum: 1,
                    title: '第一个题目名称有点长有点长有点长?',
                    max: 3,
                    min: 1,
                    choice: [
                        { id: 1, content: '选项一', status: false },
                        { id: 2, content: '选项二', status: false },
                        { id: 3, content: '选项三', status: false },
                        { id: 4, content: '选项四', status: false }
                    ],
                    anTF: false //判断改题目是否有答案
                },
                {
                    subNum: 2,
                    title: '第二个题目名称第二个题目名称第二个题目名称第二个题目名称?',
                    max: 1,
                    min: 1,
                    choice: [
                        { id: 1, content: '选项一', status: false },
                        { id: 2, content: '选项二', status: false },
                        { id: 3, content: '选项三', status: false }
                    ],
                    anTF: false
                },
                {
                    subNum: 3,
                    title: '第三个题目名称第三个题目名称第三个题目名称第三个题目名称?',
                    max: 1,
                    min: 1,
                    choice: [
                        { id: 1, content: '选项一', status: false },
                        { id: 2, content: '选项二', status: false },
                        { id: 3, content: '选项三', status: false }
                    ],
                    anTF: false
                }
            ]
        };
    },
    methods: {
        checkAllGroupChange(item, index, allchoice, cItem) {
            var len = this.answers[index].length;
            console.log(len);
            if (item.max == 1) {
                this.bool = true; //修改题目属性
                if (len == item.max) {
                    //判断选项
                    this.lists[index].anTF = true;
                } else {
                    this.lists[index].anTF = false;
                }
            } else {
                this.bool = false;
                if (len <= item.max && len >= item.min) {
                    //判断选项个数
                    this.lists[index].anTF = true;
                } else {
                    this.answers[index].pop();
                    this.lists[index].anTF = false;
                }
            }
            console.log(this.lists[index].anTF);
        },

        submit(lists) {
            var a = 0;
            lists.forEach((item, index) => {
                if (item.anTF == false) {
                    a++;
                }
            });

            if (a != 0) {
                alert('你还有' + a + '题没有完成哦！');
            } else {
                // this.spinShow=true;
                // setTimeout(()=>{
                //     if(this.spinShow==true){
                //         this.spinShow==false;
                //     }
                // },2000);
                alert('提交成功！');
            }
        }
    },
    created: function() {
        var start_str = this.starttime.replace(/-/g, '/'); //开始时间字符串转换为日期
        var start_time = new Date(start_str);

        var end_str = this.endtime.replace(/-/g, '/'); //结束时间字符串转换为日期
        var end_time = new Date(end_str);

        var now = new Date(); //当前时间

        if (start_time <= now && now <= end_time) {
            this.isActive = true;
            this.state = '进行中';
        } else if (now <= start_time) {
            this.isActive = false;
            this.state = '未开始';
        } else {
            this.isActive = false;
            this.state = '已结束';
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import '../css/common.css';
@import '../css/detail.css';
</style>
