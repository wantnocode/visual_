<template>
    <div>
        <Tabs value="name1">
            <TabPane label="实体与关系划分" name="name1">
                <i-table border :columns="columns1" :data="data1"></i-table>
                <p class="addentity">
                    <Button type="info" style="padding:5px;" @click="addentity()">添加实体按钮</Button>
                </p>
                <p class="entity_title">
                    <span>实体类型</span>
                    <span>vertex_id</span>
                    <span>实体属性</span>
                </p>
               <!-- <div> -->
                    <p v-for="(item,index) in entities" :title="item" :key="item">
                        <i-select  size="small" style="width:100px" placeholder="">
                            <i-option v-for="item in entityType" :value="item.key">{{ item.text }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px" placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px" placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px" placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
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
                    <i-select  size="small" style="width:100px" placeholder="">
                        <i-option v-for="item in relationType" :value="item.key">{{ item.text }}</i-option>
                    </i-select>
                    <i-select  size="small" style="width:100px"  placeholder="">
                        <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                    </i-select>
                    <i-select  size="small" style="width:100px"  placeholder="">
                        <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                    </i-select>
                    <i-select  size="small" style="width:100px"  placeholder="">
                        <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                    </i-select>
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
                    <p v-for="item in entities" :title="item" >
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                    </p>
                <p>
                    属性统一为:
                    <p>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                            <i-option v-for="item in columns1" :value="item.key">{{ item.key }}</i-option>
                        </i-select>
                        <i-select  size="small" style="width:100px"  placeholder="">
                        </i-select>
                    </p>
                </p>
            </TabPane>
            <TabPane label="添加实体/关系纬度" name="name3">
                 <div class="warp">
                    <div class="left_Graph">

                         <diagram ref='diag' :model-data='diagramData' @node-click="node_click" @model-changed='modelChanged' @changed-selection='changedSelection' style='width:100%; height:600px'></diagram>

                    </div>
                    <div class="right_table">
                        名称:<input type="text" v-model="schema_name">
                        <ul>
                            <li>{{key_arr.text}}</li>
                            <li>{{key_arr.key}}</li>
                            <li>{{key_arr.color}}</li>
                        </ul>
                    </div>
                </div>
            </TabPane>
        </Tabs>
    </div>
</template>

<script>
import diagram from '../components/goGraph'

export default {
    name: 'schema',
    components:{
    	diagram
    },	
    data () {
        return {
            
            model2: '',
            entitiyNum:3,
            relationNum:2,
            entities:[
            1,2,
            ],
            relations:[
            1
            ],
            entityType:[
                {
                    key:"person",
                    text:"人"
                },
                {
                    key:"kh",
                    text:"卡号"
                }
            ],
            relationType:[
                {
                    key:"jy",
                    text:"交易"
                },{
                    key:"yy",
                    text:"拥有"
                }
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
            console.log(this.key_arr)
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
		width: 60%;
		background: #fff;
		height: 600px;
		border: 1px solid #ccc;
	}
	.right_table{
		margin-left: 2%;
		width: 38%;
		border: 1px solid #ccc;
	}
    table td{
        border:1px solid #ccc;
        padding: 5px 10px;
    }
</style>
