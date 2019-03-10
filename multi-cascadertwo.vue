<template>
  <div class="searchType" ref="mbMenu">
    <span class="select-option">{{labletitle}}</span>
    <div class="el-select select multiSelect">
      <div class="el-input" style="position: relative">
        <div>
          <i class="el-input__icon el-icon-caret-top iconbtnthis is-reverse" v-if="canshow" @click="closeOpenIcon" ref="closebtn"></i>
          <i class="el-input__icon el-icon-caret-top iconbtnthis"  v-else @click="closeOpenIcon" ref="closebtn"></i>
          <!--<i :class="isclose?classA:classB" @click="closeOpenIcon" ref="closebtn"></i>-->
          <input autocomplete="off" placeholder="全部"  readonly="readonly" ref='inputbtn'  :value="selecteddata" :title="selecteddata" icon="caret-top" type="text" rows="2" class="el-input__inner inputstyle" :style="{width:widthInput}" @click="showSelectlist">
        </div>
        <div>
          <div class="el-scrollbar__view el-select-dropdown__list selectcontain boxfor" style="max-height: 340px;overflow: auto;" ref='box' v-show="canshow">
            <div style="margin-left: 10px">
              <el-checkbox  style="color: #48576a;margin-top: 6px"  v-model="isall" @change="selectBtnAll(isall)">全部</el-checkbox>
            </div>
            <div>
              <div v-for="item in selectlist">
                <span class="el-select-group__title" v-if="item.lable">{{item.lable}}</span>
                <div class="selectitem" v-for="itemchild in item.child">
                  <div v-if="itemchild.child">
                    <div @click="showChildOption" style="cursor: pointer;line-height: 32px">
                      <span style="color: #48576a;padding-left: 24px" @click="showChildOption_1">{{itemchild.lable }}</span>
                      <i class="el-icon-arrow-right iconitem" @click="showChild(itemchild)" style="line-height: 32px"></i>
                    </div>
                  </div>
                  <div v-else>
                    <el-checkbox :label="itemchild.lable" style="color: #48576a;" @change="selectBtn(itemchild)"  v-model="itemchild.checked"></el-checkbox>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="el-scrollbar__view el-select-dropdown__list selectcontain1 boxfor1" ref='boxitem' v-show="canshow" style="max-height: 320px;overflow: auto;display: none" >
            <div style="margin-left: 10px">
              <el-checkbox v-model="isallchild" v-if="selectend.length>1" style="color: #48576a;margin-top: 6px" @change="selectBtnAllChild(isallchild)">全部</el-checkbox>
            </div>
            <div class="selectitem" v-for="item in selectend">
              <el-checkbox :label="item.lable" style="color: #48576a;" @change="selectBtn(item)" v-model="item.checked"></el-checkbox>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: "multi-cascadertwo",
    data: function () {
      return {
        classA:"el-input__icon el-icon-caret-top iconbtnthis",
        classB:"el-input__icon el-icon-caret-top iconbtnthis is-reverse",
        isallchild:false,
        isall:false,
        canshow:false,
        widthInput:this.widthinput,
        selectlist:this.selectlistdata,
        labletitle:this.labletitledata,
        selectend:[],
        selecteddata:[],
        sendselectdata:[],
      }
    },
    created() {
      // document.addEventListener('click',(e)=>{
      //   if(!this.$el.contains(e.target)){
      //     var el = e.target;
      //     $(el).find(".iconbtnthis").click();
      //   }
      // })
    },
    watch:{

    },
    methods:{
      isAllCheckedFor(){
        let selectlistdata=this.selectlist;
        this.isall=true;
        console.log(selectlistdata);
        for(let i=0;i<selectlistdata.length;i++){
          if(selectlistdata[i].checked!=undefined){
            if(!selectlistdata[i].checked){
              this.isall=false
            }
          }
          if(selectlistdata[i].child){
            if(selectlistdata[i].child.checked!=undefined){
              if(!selectlistdata[i].child.checked){
                this.isall=false
              }
            }
            for(let j=0;j<selectlistdata[i].child.length;j++){
              if(selectlistdata[i].child[j].checked!=undefined){
                if(!selectlistdata[i].child[j].checked){
                  this.isall=false
                }
              }
              if(selectlistdata[i].child[j].child){
                if(selectlistdata[i].child[j].checked!=undefined){
                  if(!selectlistdata[i].child[j].checked){
                    this.isall=false
                  }
                }
                for(let k=0;k<selectlistdata[i].child[j].child.length;k++){
                  if(selectlistdata[i].child[j].child[k].checked!=undefined){
                    if(!selectlistdata[i].child[j].child[k].checked){
                      this.isall=false
                    }
                  }
                }
              }
            }
          }
        }
      },
      selectBtnAllChild(data){
        if(data) {
          let selectlistdata = this.selectend;
          for(let i=0;i<selectlistdata.length;i++){
            selectlistdata[i].checked=true;
          }
          this.selectend=selectlistdata;
          this.selectBtn()
        }else{
          let selectlistdata = this.selectend;
          for(let i=0;i<selectlistdata.length;i++){
            selectlistdata[i].checked=false;
          }
          this.selectend=selectlistdata;
          this.selectBtn()
        }
      },
      selectBtnAll(data){
        if(data){
          this.isallchild=true;
          let selectlistdata=this.selectlist;
          for(let i=0;i<selectlistdata.length;i++){
            if(selectlistdata[i].checked!=undefined){
              selectlistdata[i].checked=true
            }
            if(selectlistdata[i].child){
              if(selectlistdata[i].child.checked!=undefined){
                selectlistdata[i].child.checked=true
              }
              for(let j=0;j<selectlistdata[i].child.length;j++){
                if(selectlistdata[i].child[j].checked!=undefined){
                  selectlistdata[i].child[j].checked=true
                }
                if(selectlistdata[i].child[j].child){
                  if(selectlistdata[i].child[j].checked!=undefined){
                    selectlistdata[i].child[j].checked=true
                  }
                  for(let k=0;k<selectlistdata[i].child[j].child.length;k++){
                    if(selectlistdata[i].child[j].child[k].checked!=undefined){
                      selectlistdata[i].child[j].child[k].checked=true;
                    }
                  }
                }
              }
            }
          }
          this.selectlist=selectlistdata;
          this.selectBtn()
        }else{
         this. clearSelectedData();
         this.selectBtn()
        }
      },
      clearSelectedData(){
        this.isall=false;
        this.isallchild=false;
        this.selecteddata=[];
        let selectlistdata=this.selectlist;
        for(let i=0;i<selectlistdata.length;i++){
          if(selectlistdata[i].checked){
            selectlistdata[i].checked=false
          }
          if(selectlistdata[i].child){
            if(selectlistdata[i].child.checked){
              selectlistdata[i].child.checked=false
            }
            for(let j=0;j<selectlistdata[i].child.length;j++){
              if(selectlistdata[i].child[j].checked){
                selectlistdata[i].child[j].checked=false
              }
              if(selectlistdata[i].child[j].child){
                if(selectlistdata[i].child[j].checked){
                  selectlistdata[i].child[j].checked=false
                }
                for(let k=0;k<selectlistdata[i].child[j].child.length;k++){
                  if(selectlistdata[i].child[j].child[k].checked){
                    selectlistdata[i].child[j].child[k].checked=false;
                  }
                }
              }
            }
          }
        }
        this.selectlist=selectlistdata;
      },
      showChildOption(e){
        var el = e.target;
        $(el).find("i").click();
      },
      showChildOption_1(e){
        var el = e.target;
        $(el).next("i").click();
      },
      closeOpenIcon(e){
        var el = e.target;
        $(el).next("input").click();
      },
      showSelectlist(e){
        var el = e.target;
        var Y = $(el).position().top+34;
        var X = $(el).position().left;
        var thiswidth = $(el).parents("div").width();
        this.$refs.box.style.left=X+"px";
        this.$refs.box.style.top=Y+"px";
        this.$refs.box.style.width=thiswidth+"px";
        // $(el).prev("i").toggleClass("is-reverse");
        // if($(el).prev("i").hasClass("is-reverse")){
        //   this.$refs.box.style.display="block";
        //   document.addEventListener('click', (e)=> {
        //     if (this.$refs.mbMenu && !this.$refs.mbMenu.contains(e.target)) {//点击除弹出层外的空白区域
        //       this.isclose=true;
        //       console.log(this.isclose)
        //       this.canshow=false;
        //     }
        //   }, false)
        // }else{
        //   this.$refs.box.style.display="none";
        //   this.$refs.boxitem.style.display="none";
        //   document.removeEventListener('click', ()=> {
        //     this.canshow=true;
        //   }, false)
        // }
        if(!this.canshow){
          // this.$refs.box.style.display="block";
          this.canshow=true;
          document.addEventListener('click', (e)=> {
            if (this.$refs.mbMenu && !this.$refs.mbMenu.contains(e.target)) {//点击除弹出层外的空白区域
              this.canshow=false;
            }
          }, false)
        }else{
          // this.$refs.box.style.display="none";
          // this.$refs.boxitem.style.display="none";
          this.canshow=false;
          document.removeEventListener('click', ()=> {
            // this.canshow=true;
          }, false)
        }
      },
      showChild(data){
        // this.isallchild=false;
        this.selectend=data.child;
        // if(isall){
        //   this.isallchild=true
        // }else{
        //   this.isallchild=false
        // }
        var el = event.target;
        var Y = $(el).position().top+28;
        var X = $(el).position().left+32;
        this.$refs.boxitem.style.left=X+"px";
        this.$refs.boxitem.style.top=Y+"px";
        if(this.$refs.boxitem.style.display=="block"){
          this.$refs.boxitem.style.display="none";
        }else{
          let isallthis=true;
          for(let i=0;i<data.child.length;i++){
            if(!data.child[i].checked){
              isallthis=false;
            }
          }
          this.isallchild=isallthis;
          setTimeout(()=>{
            this.$refs.boxitem.style.display="block";
          },100)
        }
      },
      selectBtn(data){
        // this.isallchild=false;
        // this.isall=false;
        this.selecteddata=[];
        this.sendselectdata=[];
        for(let i=0;i<this.selectlist.length;i++){
          for(let j=0;j<this.selectlist[i].child.length;j++) {
            if(this.selectlist[i].child[j].checked){
              this.selecteddata.push(this.selectlist[i].child[j].lable);
              this.sendselectdata.push({
                key:this.selectlist[i].child[j].id,
                value:this.selectlist[i].child[j].lable,
                pkey:this.selectlist[i].id,
                pvalue:this.selectlist[i].lable
              });
            }
            if(this.selectlist[i].child[j].child){
              for(let k=0;k<this.selectlist[i].child[j].child.length;k++) {
                if(this.selectlist[i].child[j].child[k].checked){
                  this.selecteddata.push(this.selectlist[i].child[j].child[k].lable);
                  this.sendselectdata.push({
                    key:this.selectlist[i].child[j].child[k].id,
                    value:this.selectlist[i].child[j].child[k].lable,
                    pkey:this.selectlist[i].child[j].id,
                    pvalue:this.selectlist[i].child[j].lable
                  });
                }
              }
            }
          }
        }
        let isallthis=true;
        for(let i=0;i<this.selectend.length;i++){
          if(!this.selectend[i].checked){
            isallthis=false;
          }
        }
        this.isallchild=isallthis;
        this.isAllCheckedFor();
        this.$emit('onclickchange',this.sendselectdata);
      }
    },
    props:{
      widthinput:{
        type: String,
        default(){
          return "120px"
        }
      },
      selectlistdata:{
        type: Array,
        default(){
          return []
        }
      },
      labletitledata:{
        type:String,
        default(){
          return ""
        }
      }
    },
    watch: {
      selectlistdata() {
        this.selectlist = this.selectlistdata
      }
    }
  }

</script>
<style scoped>
  .selectcontain{
    /*display: none;*/
    position: absolute;
    z-index: 100;
    border: 1px solid #d1dbe5;
    border-radius: 2px;
    box-shadow: 0 2px 4px rgba(0,0,0,.12), 0 0 6px rgba(0,0,0,.04);
    box-sizing: border-box;
    background-color: #fff;
    padding: 2px 2px 4px 2px;
  }
  .selectcontain1{
    display: none;
    position: absolute;
    z-index: 100;
    border: 1px solid #d1dbe5;
    border-radius: 2px;
    box-shadow: 0 2px 4px rgba(0,0,0,.12), 0 0 6px rgba(0,0,0,.04);
    box-sizing: border-box;
    background-color: #fff;
    padding: 2px 2px 4px 2px;
  }
  .selectitem{
    white-space:nowrap;
    overflow: hidden;
    padding: 8px 10px
  }
  .selectitem:hover{
    background-color: #e4e8f1;
  }
  .iconitem{
    color: #bfcbd9;
    float: right;

  }
  .inputstyle{
    height: 30px!important;
    overflow:hidden;
    text-overflow:ellipsis;
    white-space:nowrap
  }
</style>
