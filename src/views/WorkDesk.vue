<template>
  <div class="workDesk">
    <!--我的应用-->
    <section v-if="MyApp">
      <header>
        <h1>我的应用</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Application></Application>
    </section>
    <!--我的服务-->
    <section v-if="MyService">
      <header>
        <h1>我的服务</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Service></Service>
    </section>
    <!--我的日程-->
    <section class="wk_card" v-if="MySchedule">
      <header>
        <h1>我的日程</h1>
        <div class="btn">
          <span v-for="(item,index) in sch_date" @click="toggle_sch_date(index)" :class="{ active : index === sch_date_index }">{{item}}</span>
        </div>
        <a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Schedule ref="Schedule" :sch_date_index="sch_date_index"></Schedule>
    </section>
    <!--校内通知-->
    <section v-if="Notice">
      <header>
        <h1>校内通知</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Notice></Notice>
    </section>
    <!--我的待办-->
    <section v-if="MyTodo">
      <header>
        <h1>我的待办</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Todo></Todo>
    </section>
    <!--学校公文|历史公文-->
    <section v-if="File">
      <header>
        <h1>学校公文<a class="history-file">历史公文</a></h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <File></File>
    </section>
    <!--个人中心-->
    <section v-if="Personal">
      <header>
        <h1>个人中心</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Personal></Personal>
    </section>
    <!--工资查询-->
    <section v-if="Pay">
      <header>
        <h1>工资查询</h1><a><i class="fa fa-ellipsis-h"></i></a>
      </header>
      <Pay></Pay>
    </section>
    <!--服务排行-->
    <section v-if="Rankings">
      <header>
        <h1>服务排行</h1>
        <div class="btn">
          <span v-for="(item,index) in service_rank" @click="toggle_service_rank(item.type,index)" :class="{ active : index === service_rank_index }">{{item.name}}</span>
        </div>
      </header>
      <Rankings ref="Rankings"></Rankings>
    </section>
  </div>
</template>

<script >
// @ is an alias to /src
import Application from "@/components/workdesk/Application.vue";
import Service from "@/components/workdesk/Service.vue";
import Schedule from "@/components/workdesk/Schedule.vue";
import Notice from "@/components/workdesk/Notice.vue";
import Todo from "@/components/workdesk/Todo.vue";
import File from "@/components/workdesk/File.vue";
import Personal from "@/components/workdesk/Personal.vue";
import Pay from "@/components/workdesk/Pay.vue";
import Rankings from "@/components/workdesk/Rankings.vue";

export default {
  name: "WorkDesk",
    components: {
      Application,
      Service,
      Schedule,
      Notice,
      Todo,
      File,
      Personal,
      Pay,
      Rankings
    },
    data: function () {
        return {
          MyApp:true,
          MyService:false,
          MySchedule:false,
          Notice:false,
          MyTodo:false,
          File:false,
          Personal:false,
          Pay:false,
          Rankings:false,
          sch_date:['今天','明天','后天'],
          sch_date_index:0,
          service_rank:[{name:'本周',type:1},{name:'本月',type:2},{name:'本年',type:3}],
          service_rank_index:0
        }
    },
    created:function () {
        this.$ajax.post(this.$url.viewHome)
            .then((res)=>{
              for(let i=0;i<res.data.widgets.length;i++){
                switch (res.data.widgets[i].NAME) {
                  case '个人中心' :this.Personal=true;
                  case '我的服务' :this.MyService=true;
                  case '学校公文' :this.File=true;
                  case '通知公告' :this.Personal=true;
                  case '我的应用' :this.MyApp=true;
                  case '校内通知' :this.Notice=true;
                  case '工资查询' :this.Pay=true;
                  case '我的日程' :this.MySchedule=true;
                  case '我的待办' :this.MyTodo=true;
                  case '服务排行' :this.Rankings=true;
                }
              }
            })
    },
  methods:{
    /***
     * 切换我的日程
     * @param index 0、1、2分别代表今天明天后天
       */
    toggle_sch_date(index){
      this.sch_date_index = index;
      this.$refs.Schedule.getData(index);
    },
    /***
     * 切换服务排行
     * type 1、2、3分别代表本周、本月、本年
     */
    toggle_service_rank(type,index){
      this.service_rank_index = index;
      this.$refs.Rankings.getData(type);
    }
  }
};
</script>

<style scoped lang="less">
  @base: 75/2rem;
  .workDesk{
    background-color: rgb(232,232,232);
    margin-top: 70/@base;
  }
  section{
    margin-top: 6/@base;
    border-top: 2px solid rgb(19,144,211);
  }
  header{
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #fff;
    padding: 10/@base 20/@base;
    border-bottom: 1px dashed #bfbfbf;
    i{
      font-size: 22/@base;
      color: rgb(19,144,211);
    }
  }
  h1{
    line-height: 30/@base;
    font-size: 18/@base;
    font-weight: 700;
    color: rgb(19,144,211);
    margin: 0;
  }
  span{
    display: inline-block;
    background-color: rgb(191,191,191);
    font-size: 14/@base;
    color: #fff;
    padding: 4/@base 10/@base;
    border-radius: 20/@base;
    margin: 0 4/@base 0 0 ;
  }
  span.active{
    background-color: rgb(240,134,37);
  }
  a img{
    width: 30/@base;
  }
  a.history-file{
    display: inline-block;
    border-left: 2px solid rgb(19,144,211);
    padding-left: 10/@base;
    margin-left: 10/@base;
  }
</style>
