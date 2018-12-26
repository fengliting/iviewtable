<template>
    <div style="width:80%;margin:50px auto">
        <Button @click="additem">添加</Button>
        <Table 
        class="margin-top-20"
        :loading="loading"
        :columns="columns" 
        :data="formList"
        size="small" 
        ref="table"  
        border 
        ></Table>
        <footer style="text-align:right;margin-top:10px">
            <Button type="primary" @click="submit">确定</Button>
            <Button @click="cancel">取消</Button>
        </footer>
    </div>
</template>

<script>
export default {
  name: 'App',
  data() {
      return {
          loading:false,
          datalist:[
              {
                  number:122333,
                  carrier:'aaa',
                  datatime:'2018-12-13 03:40:12'
              }
          ],
          Types:[
              { value: 'hhh', name: 'hhh' },
              { value: 'aaa', name: 'aaa' },
              { value: 'cccc', name: 'cccc' },
              { value: 'tytyty', name: 'tytyty' },
          ],
      }
  },
  created() {
      this.initdatalist()
  },
  computed:{
      formList(){
          return this.datalist || []
      },
      columns() {
          const _this = this
          return [
          {
              title: 'Number',
              key: 'number',
              render(h, { row,index }) {
                  if(row.$Edit){
                      return h('Input',{
                          props: {
                              size:'small',
                              value:row.number
                          },
                          on: {
                              'input':(event) => {
                                //   console.log(event)
                                  row.number = event
                                  _this.datalist[index] = row
                              }
                          },
                      })
                  }else{
                      return <div>{row.number}</div>
                  }
              }
          }, {
              title: 'Carrier',
              key: 'carrier',
              render(h, { row,index }) {
                  if(row.$Edit){
                      return h('Select',{
                          props:{  
                              value: row.carrier,
                          },  
                          on: {  
                              'on-change':(value) => {  
                                  row.carrier = value
                                  _this.datalist[index] = row
                              }  
                          },  
                      },
                          _this.Types.map((json)=>{  
                              // console.log(json)
                              return h('Option', {  
                                  props: {
                                      value: json.value
                                
                                  }  
                              },json.name);  
                          })
                      )
                  }else{
                      return <div>{row.carrier}</div>
                  }
              }
          }, {
              title: '时间',
              key: 'datatime',
              width:220,
              render(h, { row,index }) {
                  if(row.$Edit){
                      return h('DatePicker',{
                          props: {
                              type:'datetime',
                              placeholder:"Select date",
                              value:_this.datalist[index].datatime
                          },
                          on: {
                              'on-change':function(date){
                                  _this.datalist[index].datatime = date
                              },
                          },
                      })
                  }else{
                      return <div>{row.datatime}</div>
                  }
              }
          },{
              title: '操作',
              width:140,
              render(h, { row, index }) {
                  return h('div',[
                      h('Button', {
                          props:{
                              type:"primary",
                              size:'small'
                          },
                          style: {
                              marginRight: '5px'
                          },
                          on: {
                              click: () => {
                                  _this.changeEdit(row)
                              }
                          }
                      },'编辑'),
                      h('Button', {
                          props:{
                              type:"error",
                              size:'small'
                          },
                          on: {
                              click: () => {
                                  _this.comremove(index)
                              }
                          }
                      },'删除')
                  ])
              }
          },]
      },
  },
  methods: {
      initdatalist(){
          this.datalist.map(item=>{
              item.$Edit = false;
          })
      },
      additem(){
          let itemdata = {
              number:null,
              carrier:null,
              datatime:null,
              $Edit:true,
          }
          this.datalist.push(itemdata)
      },
      changeEdit(row){
          this.$set(row, '$Edit', true)
      },
      comremove(index) {
          this.datalist.splice(index, 1);
      },
      submit(){
          console.log(this.datalist)
      },
      cancel(){},
      
  },
}
</script>
