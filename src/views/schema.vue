<template>
    <div>
        <Tabs value="name1"  type="card">
            <TabPane label="实体与关系划分" name="name1">
                <i-table border :columns="columns1" :data="data1" ></i-table>
                <p class="addentity">
                    <Button type="info" style="padding:5px;" @click="addentity()">添加实体按钮</Button>
                </p>
                <p class="entity_title">
                    <span>实体类型</span>
                    <span>vertex_id</span>
                    <span>实体属性</span>
                </p>
               <!-- <div> -->
                    <p v-for="(item,index) in entityProperies">
                       <i-input size="small" placeholder="" style="width: 100px;" v-model="item.type"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;" v-model="item.vertex_id"></i-input>
                       <span  v-for="(o,i) in item.properies">
                                <i-input size="small" placeholder="" style="width: 100px;" :key="index + ',' + i "  v-model="o.val"></i-input>
                           
                       </span>
                       <!-- <i-input size="small" placeholder="" style="width: 100px;"></i-input> -->
                        <i-button style="padding: 2px 10px" @click="deleteEntity(index)">删除</i-button>
                    </p>



               <!-- </div> -->
                <p class="addedge">
                    <Button type="info" style="padding:5px;" @click="addedge()">添加关系按钮</Button>
                </p>
                <p class="entity_title">
                    <span>关系类型</span>
                    <span>src_id</span>
                    <span>dst_id</span>
                    <span>关系属性</span>

                </p>
                <p v-for="item in relations" :title="item" :key="item">
                   <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                    <i-button style="padding: 2px 10px" @click="deleteRelation(index)">删除</i-button>
                </p>
                <!-- <p class="next_edit">
                    <Button type="error" style="padding:5px;">下一步</Button>
                </p>
 -->
            </TabPane>
            <TabPane label="元素属性命名统一" name="name2">
               <p>
                   原实体:
               </p>

               <p class="entity_title">
                    <span>实体类型</span>
                    <span>vertex_id</span>
                    <span>实体属性</span>
                </p>
               <!-- <div> -->
                     <p v-for="(item,index) in entityProperies">
                       <i-input size="small" placeholder="" style="width: 100px;" v-model="item.type"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;" v-model="item.vertex_id"></i-input>
                       <span  v-for="(o,i) in item.properies">
                                <i-input size="small" placeholder="" style="width: 100px;"  :key="index + ',' + i "  v-model="o.val"></i-input>
                           
                       </span>
                       <!-- <i-input size="small" placeholder="" style="width: 100px;"></i-input> -->
                        <i-button style="padding: 2px 10px" @click="deleteEntity(index)">删除</i-button>
                    </p>
                <p>
                    属性统一为:
                    <p>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                       <i-input size="small" placeholder="" style="width: 100px;"></i-input>
                    </p>
                </p>
            </TabPane>
            <TabPane label="添加实体/关系纬度" name="name3">
                 <div class="warp">
                    <div class="left_Graph">

                         <diagram ref='diag' :model-data='diagramData' @link-click="link_click" @node-click="node_click" @model-changed='modelChanged' @changed-selection='changedSelection' style='width:100%; height:600px'></diagram>

                    </div>
                    <div class="right_table">
                        <ul class="latitude_title">
                           <li v-if="isEdge">link纬度信息</li>
                           <li v-else>node纬度信息</li>
                           <li>添加基础纬度</li>
                           <li>添加聚合纬度</li>
                        </ul>

                        <div class="node_latitude" v-for="item in latitudeList" v-show="item.type==latitude_val">
                            <p>entityName</p>
                             <i-input size="small" placeholder="" style="width: 200px;" v-model="item.entityName"></i-input>
                            <p>Description</p>
                            <i-input size="small" placeholder="" style="width: 200px;" v-model="item.description"></i-input>
                            <p>Vertex</p>
                            <i-input size="small" placeholder="" style="width: 200px;" v-model="item.vertex"></i-input>
                            <p>Properties</p>
                            <p>
                                <span style="display: inline-block;width: 100px;height: 30px;text-align: center;line-height: 30px;" v-for="item in propertyTitles">{{item}}</span>
                                
                            </p>
                            <p  v-for="i in item.properties">
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;"></span>
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;"></span>
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;"></span>

                                <span style="display: inline-block;border: 1px solid #ccc;">
                                    <select name="" id="" style="width: 96px;height: 30px;">
                                        <option value="" v-for="item in dataTypes" :key="item" :title="item">{{item}}</option>
                                    </select>
                                </span>

                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;">
                                    <select name="" id=""  style="width: 96px;height: 30px;">
                                        <option value="" v-for="item in aggregateFunctions" :key="item"> {{item}}</option>
                                    </select>
                                </span>
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;">
                                    <select name="" id=""  style="width: 96px;height: 30px;">
                                        <option value="" v-for="item in validateFunctions" :key="item"> {{item}}</option>
                                    </select>
                                </span>
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;"></span>
                                <span style="display: inline-block;border: 1px solid #ccc;width: 100px; height: 30px;"></span>
                            </p>
                            <p>GroupByProperties</p>
                            <p><i-input size="small" placeholder="" style="width: 200px;"></i-input></p>

                        </div>
                        <!-- <div class="edge_latitude" v-show="latitude_val=='edge'">
                            <p>edgeName</p>
                             <i-input size="small" placeholder="small size"></i-input>
                            <p>Description</p>
                            <p>source Node</p>
                            <p>destination Node</p>
                            <p>Directed</p>
                            <p>Properties</p>
                            <p>GroupByProperties</p>
                        </div> -->

                    </div>
                </div>
            </TabPane>
        </Tabs>
    </div>
</template>

<script>
import diagram from '../components/goGraph'
import axios from 'axios'
export default {
    name: 'schema',
    components:{
    	diagram
    },	
    data () {
        return {
            latitude_val:"entity",
            isEdge:false,  //边为true 点为false
            latitudeList:[
                {
                     "latitudeType":"Super",
                     "type":"entity",
                     "entityName":"",
                     "description": "人",
                      "vertex": "人",
                      "properties": {
                        "jymc": "gcsTest-BasicEntityPerson.jymc.String"
                      },
                      "aggregate": false
                },
                {
                     "latitudeType":"Super",
                     "type":"edge",
                     "entityName":"",
                     "description": "表示交易汇总.",  //描述
                      "source": "人",         //源
                      "destination": "人",     //目标
                      "directed": "directed",   //
                      "properties": {           //属性
                        "day1": "SuperEdgeJiaoYi-gcsTest.day1.Date",   //属性类型
                        "day2": "SuperEdgeJiaoYi-gcsTest.day2.Date",    //
                        "zje": "SuperEdgeJiaoYi-gcsTest.zje.Double"
                      },
                      "groupBy": [    //合并
                        "day1",
                        "day2"
                      ]
                },
            ],
            dataTypes:[], // 支持的数据类型
            aggregateFunctions:[],//聚合函数名称集合
            validateFunctions:[],// 验证函数名称集合
            model2: '',
            entitiyNum:3,
            relationNum:2,
            entities:[
                1,2,
            ],
            entityProperies:[
                {
                    type:"",
                    vertex_id:"",
                    properies:[{
                        val:""
                    },{
                        val:""
                    }],
                },
                {
                    type:"",
                    vertex_id:"",
                    properies:[{
                        val:""
                    },{
                        val:""
                    }],
                },
            ],
            linkProperies:[
                {
                    type:"",
                    vertex_id:"",
                    properies:[{
                        val:""
                    },{
                        val:""
                    }],
                }
            ],
            propertyTitles:[
                "从属性",
                "格式转换",
                "建属性",
                "属性类型",
                "聚合函数",
                "验证函数",
                "描述",
                "高级参数"
            ],

            relations:[
            1
            ],
            
            columns1: [
                    {
                        title: 'jykh',
                        key: 'jykh'
                    },
                    {
                        title: 'sfzh',
                        key: 'sfzh'
                    },
                    
                    {
                        title: 'jyje',
                        key: 'jyje'
                    },
                    {
                        title: 'dsjykh',
                        key: 'dsjykh'
                    },
                    {
                        title: 'dssfzh',
                        key: 'dssfzh'
                    },
                    
                ],
                data1: [
                    {
                        jykh: '23232321**112',
                        sfzh: '121312321342354534',
                        jyje: '10000',
                        dsjykh: '23232321**113',
                        dssfzh: '121312321342354534'
                    },
                     {
                        jykh: '23232321**112',
                        sfzh: '121312321342354534',
                        jyje: '10000',
                        dsjykh: '23232321**113',
                        dssfzh: '121312321342354534'
                    },
                     {
                        jykh: '23232321**112',
                        sfzh: '121312321342354534',
                        jyje: '10000',
                        dsjykh: '23232321**113',
                        dssfzh: '121312321342354534'
                    },
            ],
            diagramData: { // passed to <diagram> as its modelData
                nodeDataArray: [
                    { key: 1, text: '人', color: 'lightblue' ,"loc":new go.Point(20,20)},
                  
                    
                ],
                linkDataArray: [
                    { from: 1, to: 1},
                    
                ]
            },
            currentNode: null,
            savedModelText: '',
            counter: 1, // used by addNode
            counter2: 4, // used by modifyStuff
            schema_name:"",
            key_arr:[]
        }
    },
    mounted () {
        var self = this;

        // 所有数据类型
          axios.get('/graphs/schemas/dataTypes')
          .then(function (res) {
            self.dataTypes = res
            // console.log(self.dataTypes)
            // console.log(typeof self.dataTypes)
          })
          .catch(function (error) {
            // handle error
            console.log(error);
          })
          .then(function () {
            // always executed
          });
        // 所有聚合函数名称
         axios.get('/graphs/schemas/aggregateFunctions')
          .then(function (res) {
            self.aggregateFunctions = res
            // console.log(self.dataTypes)
            
          })
          .catch(function (error) {
            // handle error
            console.log(error);
          })
          .then(function () {
            // always executed
          });
          //// 所有验证函数名称
         axios.get('/graphs/schemas/validateFunctions')
          .then(function (res) {
            self.validateFunctions = res
            // console.log(self.dataTypes)
            
          })
          .catch(function (error) {
            // handle error
            console.log(error);
          })
          .then(function () {
            // always executed
          });
    },
    computed: {
        currentNodeText: {
            get: function () {
                let node = this.currentNode
                if (node instanceof window.go.Node) {
                    return node.data.text
                } else {
                    return ''
                }
            },
            set: function (val) {
                let node = this.currentNode
                if (node instanceof window.go.Node) {
                    let model = this.model()
                    model.startTransaction()
                    model.setDataProperty(node.data, 'text', val)
                    model.commitTransaction('edited text')
                }
            }
        }
    },
    methods: {
        
        model: function () {
            return this.$refs.diag.model()
        },
        node_click(e,node){
        	// node.data  选中值
            // this.model().setDataProperty(node.data, 'text', "123")
            this.key_arr = node.data
            this.isEdge = false
            console.log(this.key_arr)
        },
        link_click(e,node){
            console.log(node.data)
            this.isEdge = true
        },
        // tell the GoJS Diagram to update based on the arbitrarily modified model data
        updateDiagramFromData: function () {
            this.$refs.diag.updateDiagramFromData()
        },
        // this event listener is declared on the <diagram>
        modelChanged: function (e) {
            if (e.isTransactionFinished) { // show the model data in the page's TextArea
                this.savedModelText = e.model.toJson()
            }
        },
        changedSelection: function (e) {

            let node = e.diagram.selection.first()
            if (node instanceof window.go.Node) {
                this.currentNode = node
                this.currentNodeText = node.data.text
            } else {
                this.currentNode = null
                this.currentNodeText = ''
            }
        },
       addNode: function () {
            let model = this.model()
            model.startTransaction()
            model.setDataProperty(model.findNodeDataForKey(4), 'color', 'purple')
            let data = { text: 'NEW ' + this.counter++, color: 'yellow' }
            model.addNodeData(data)
            model.addLinkData({ from: 3, to: model.getKeyForNodeData(data) })
            model.commitTransaction('added Node and Link')
            // also manipulate the Diagram by changing its Diagram.selection collection
            let diagram = this.$refs.diag.diagram
            diagram.select(diagram.findNodeForData(data))
        },
        // Here we modify VUE's view model directly, and
        // then ask the GoJS Diagram to update everything from the data.
        // This is less efficient than calling the appropriate GoJS Model methods.
        // NOTE: Undo will not be able to restore all of the state properly!!
        modifyStuff: function () {
            let data = this.diagramData
            data.nodeDataArray[0].color = 'red'
            // Note here that because we do not have the GoJS Model,
            // we cannot find out what values would be unique keys, for reference by the link data.
            data.nodeDataArray.push({ key: ++this.counter2, text: this.counter2.toString(), color: 'orange' })
            data.linkDataArray.push({ from: 2, to: this.counter2 })
            this.updateDiagramFromData()
        },
        addentity(){
            this.entities.push(this.entitiyNum)
            this.entitiyNum++;
        },
        addedge(){
            this.relations.push(this.relationNum)
            this.relationNum++
        },
        deleteEntity(index){
            console.log(index)
            this.entities.splice(index,1)
        },
        deleteRelation(index){
            this.relations.splice(index,1)
        }
    }
}
</script>

<style scoped>
    .latitude_title{
        display: flex;
    }
    .latitude_title li{
        width: 30%;
        margin-left: 3%;
        text-align: center;
        line-height: 32px;
        height: 32px;
        background: #000;
        color: #fff;
    }
    .latitude_title li:hover{
        cursor: pointer;
        background: #fff;
        border:1px solid #000;
        color: #000;
    }
    .latitude_title li:first-child{
        margin-left: 0;
    }
    .addentity,.addedge,.next_edit{
        text-align: end;
    }
    .entity_title span{
        display: inline-block;
        width: 100px;
        
        line-height: 22px;
        border: .5px solid #ccc;
        text-align: center;
    }
    *{
        margin: 0;
        padding: 0
    }
    p{
        margin-top: 3px;
    }
    .table_content{
        height: 260px;
    }
	.warp{
		display: flex;

	}
	.left_Graph{
		width: 35%;
		background: #fff;
		height: 600px;
		border: 1px solid #ccc;
	}
	.right_table{
		margin-left: 2%;
		width: 63%;
		border: 1px solid #ccc;
	}
    table td{
        border:1px solid #ccc;
        padding: 5px 10px;
    }
</style>
