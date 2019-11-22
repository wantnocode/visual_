<template>
    <div></div>
</template>

<script>
let $ = window.go.GraphObject.make
export default {
    name: '',
    props: ['modelData','layout'],
    data () {
        return {
            diagram: null
        }
    },
    mounted () {
        console.log(window.go)
        let self = this
        let myDiagram =
            $(window.go.Diagram, this.$el,
                {
                    initialContentAlignment: window.go.Spot.Center,
                    // layout: $(window.go.TreeLayout, {angle: 90, arrangement: window.go.TreeLayout.ArrangementHorizontal}),
                    'undoManager.isEnabled': true,
                    // Model ChangedEvents get passed up to component users
                    'ModelChanged': function (e) {
                        self.$emit('model-changed', e)
                    },
                    'ChangedSelection': function (e) {
                        self.$emit('changed-selection', e)
                    }
                })
        myDiagram.nodeTemplate =
            $(window.go.Node, 'Auto',{
                click:function(e,node){
                    self.$emit("node-click",e,node)
                }
            },
            new go.Binding("location","loc").makeTwoWay(),
            $(window.go.Shape,"Ellipse",{
                        fill: 'white',
                        strokeWidth: 0,
                        portId: '',
                        // fromLinkable: true,
                        // toLinkable: true,
                        cursor: 'pointer',
                        width:60,
                        height:60
            },
            new window.go.Binding('fill', 'color')),
            $(window.go.TextBlock,
                    {margin: 8, editable: true},
            new window.go.Binding('text')
            )
        )
        myDiagram.linkTemplate =
            $(window.go.Link,{
                click:function(e,node){
                    self.$emit("link-click",e,node)
                }
            },
            $(go.Shape),
            $(go.Shape,{toArrow:"standard"})
               
            ),
            $(window.go.TextBlock,{
                // text:"1"
            })
        this.diagram = myDiagram
        this.updateModel(this.modelData)
    },
    watch: {
        modelData: function (val) {
            this.updateModel(val)
        }
    },
    computed: {},
    methods: {
        model: function () {
            return this.diagram.model
        },
        updateModel: function (val) {
            // No GoJS transaction permitted when replacing Diagram.model.
            if (val instanceof window.go.Model) {
                this.diagram.model = val
            } else {
                let m = new window.go.GraphLinksModel()
                if (val) {
                    for (let p in val) {
                        m[p] = val[p]
                    }
                }
                this.diagram.model = m
            }
        },
        updateDiagramFromData: function () {
            this.diagram.startTransaction()
            // This is very general but very inefficient.
            // It would be better to modify the diagramData data by calling
            // Model.setDataProperty or Model.addNodeData, et al.
            this.diagram.updateAllRelationshipsFromData()
            this.diagram.updateAllTargetBindings()
            this.diagram.commitTransaction('updated')
        }
    }
}
</script>

<style>
</style>