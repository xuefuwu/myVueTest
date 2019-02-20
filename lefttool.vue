<template>
    <div id="editorLeftTool" class="editor-left-tool">
        <div class="flt-container">
            <div class="flt-search"></div>
            <ul>
                <li class="flt-tool-list" v-for="(tool, index ) in searchToolItems" :key="index">
                    <div class="flt-list-tit" @click.stop="tool.show=!tool.show">
                        <icon :name="tool.show?'down-arrow':'up-arrow'" :size="8" class="flt-icon"></icon>
                        <span>{{tool.title}}</span>
                    </div>
                    <div class="flt-item-list" v-show="tool.show">
                        <div class="flt-item-box">
                            <template>
                                <ul>
                                    <li
                                        class="flt-list-item"
                                        v-for="(item, index) in tool.listData"
                                        :key="index"
                                    >
                                        <div
                                            draggable="true"
                                            @dragstart="selNode(item)"
                                            @dragend="nodeDragEnd"
                                        >{{item.title}}</div>
                                    </li>
                                </ul>
                            </template>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import { mapState, mapMutations } from "vuex";
import axios from "axios";
export default {
    name: "editorLeftTool",
    data() {
        return {
            selItem: undefined,
            searchCont: "",
            toolItems: [
                {
                    title: "事件",
                    listData: [
                        {
                            id: "StartNodeEvent",
                            title: "开始",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns="http://www.w3.org/2000/svg"\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   width="40"\n   height="40"\n   version="1.0">\n  <defs></defs>\n  <oryx:magnets>\n  \t<oryx:magnet oryx:cx="16" oryx:cy="16" oryx:default="yes" />\n  </oryx:magnets>\n  <g pointer-events="fill">\n    <circle id="bg_frame" cx="16" cy="16" r="15" stroke="#585858" fill="#ffffff" stroke-width="1"/>\n\t<text font-size="11" \n\t\tid="text_name" \n\t\tx="16" y="33" \n\t\toryx:align="top center" \n\t\tstroke="#373e48"\n\t></text>\n  </g>\n</svg>'
                        },
                        {
                            id: "EndNoneEvent",
                            title: "结束",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns="http://www.w3.org/2000/svg"\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   width="40"\n   height="40"\n   version="1.0">\n  <defs></defs>\n  <oryx:magnets>\n  \t<oryx:magnet oryx:cx="16" oryx:cy="16" oryx:default="yes" />\n  </oryx:magnets>\n  <g pointer-events="fill">\n    <circle id="bg_frame" cx="16" cy="16" r="14" stroke="#585858" fill="#ffffff" stroke-width="3"/>\n\t<text font-size="11" \n\t\tid="text_name" \n\t\tx="16" y="32" \n\t\toryx:align="top center" \n\t\tstroke="#373e48"\n\t></text>\n  </g>\n</svg>'
                        }
                    ],
                    show: true
                },
                {
                    title: "任务",
                    listData: [
                        {
                            id: "UserTask",
                            title: "普通任务",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns="http://www.w3.org/2000/svg"\n   xmlns:svg="http://www.w3.org/2000/svg"\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   xmlns:xlink="http://www.w3.org/1999/xlink"\n\n   width="102"\n   height="82"\n   version="1.0">\n  <defs></defs>\n  <oryx:magnets>\n  \t<oryx:magnet oryx:cx="1" oryx:cy="20" oryx:anchors="left" />\n  \t<oryx:magnet oryx:cx="1" oryx:cy="40" oryx:anchors="left" />\n  \t<oryx:magnet oryx:cx="1" oryx:cy="60" oryx:anchors="left" />\n  \t\n  \t<oryx:magnet oryx:cx="25" oryx:cy="79" oryx:anchors="bottom" />\n  \t<oryx:magnet oryx:cx="50" oryx:cy="79" oryx:anchors="bottom" />\n  \t<oryx:magnet oryx:cx="75" oryx:cy="79" oryx:anchors="bottom" />\n  \t\n  \t<oryx:magnet oryx:cx="99" oryx:cy="20" oryx:anchors="right" />\n  \t<oryx:magnet oryx:cx="99" oryx:cy="40" oryx:anchors="right" />\n  \t<oryx:magnet oryx:cx="99" oryx:cy="60" oryx:anchors="right" />\n  \t\n  \t<oryx:magnet oryx:cx="25" oryx:cy="1" oryx:anchors="top" />\n  \t<oryx:magnet oryx:cx="50" oryx:cy="1" oryx:anchors="top" />\n  \t<oryx:magnet oryx:cx="75" oryx:cy="1" oryx:anchors="top" />\n  \t\n  \t<oryx:magnet oryx:cx="50" oryx:cy="40" oryx:default="yes" />\n  </oryx:magnets>\n  <g pointer-events="fill" oryx:minimumSize="50 40">\n\t<rect id="text_frame" oryx:anchors="bottom top right left" x="1" y="1" width="94" height="79" rx="10" ry="10" stroke="none" stroke-width="0" fill="none" />\n\t<rect id="bg_frame" oryx:resize="vertical horizontal" x="0" y="0" width="100" height="80" rx="10" ry="10" stroke="#bbbbbb" stroke-width="1" fill="#f9f9f9" />\n\t\t<text \n\t\t\tfont-size="12" \n\t\t\tid="text_name" \n\t\t\tx="50" \n\t\t\ty="40" \n\t\t\toryx:align="middle center"\n\t\t\toryx:fittoelem="text_frame"\n\t\t\tstroke="#373e48">\n\t\t</text>\n\t\n\t<g id="userTask" transform="translate(3,3)">\n\t\t<path oryx:anchors="top left"\n       \t\tstyle="fill:#d1b575;stroke:none;"\n       \t\t d="m 1,17 16,0 0,-1.7778 -5.333332,-3.5555 0,-1.7778 c 1.244444,0 1.244444,-2.3111 1.244444,-2.3111 l 0,-3.0222 C 12.555557,0.8221 9.0000001,1.0001 9.0000001,1.0001 c 0,0 -3.5555556,-0.178 -3.9111111,3.5555 l 0,3.0222 c 0,0 0,2.3111 1.2444443,2.3111 l 0,1.7778 L 1,15.2222 1,17 17,17" \n         />\n\t\t\n\t</g>\n  \n\t<g id="parallel">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" d="M46 70 v8 M50 70 v8 M54 70 v8" stroke-width="2" />\n\t</g>\n\t\n\t<g id="sequential">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" stroke-width="2" d="M46,76h10M46,72h10 M46,68h10"/>\n\t</g>\n\t\n\n\t<g id="compensation">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" d="M 62 74 L 66 70 L 66 78 L 62 74 L 62 70 L 58 74 L 62 78 L 62 74" stroke-width="1" />\n\t</g>\n  </g>\n</svg>'
                        },
                        {
                            id: "ReviewTask",
                            title: "审核任务",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns="http://www.w3.org/2000/svg"\n   xmlns:svg="http://www.w3.org/2000/svg"\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   xmlns:xlink="http://www.w3.org/1999/xlink"\n\n   width="102"\n   height="82"\n   version="1.0">\n  <defs></defs>\n  <oryx:magnets>\n  \t<oryx:magnet oryx:cx="1" oryx:cy="20" oryx:anchors="left" />\n  \t<oryx:magnet oryx:cx="1" oryx:cy="40" oryx:anchors="left" />\n  \t<oryx:magnet oryx:cx="1" oryx:cy="60" oryx:anchors="left" />\n  \t\n  \t<oryx:magnet oryx:cx="25" oryx:cy="79" oryx:anchors="bottom" />\n  \t<oryx:magnet oryx:cx="50" oryx:cy="79" oryx:anchors="bottom" />\n  \t<oryx:magnet oryx:cx="75" oryx:cy="79" oryx:anchors="bottom" />\n  \t\n  \t<oryx:magnet oryx:cx="99" oryx:cy="20" oryx:anchors="right" />\n  \t<oryx:magnet oryx:cx="99" oryx:cy="40" oryx:anchors="right" />\n  \t<oryx:magnet oryx:cx="99" oryx:cy="60" oryx:anchors="right" />\n  \t\n  \t<oryx:magnet oryx:cx="25" oryx:cy="1" oryx:anchors="top" />\n  \t<oryx:magnet oryx:cx="50" oryx:cy="1" oryx:anchors="top" />\n  \t<oryx:magnet oryx:cx="75" oryx:cy="1" oryx:anchors="top" />\n  \t\n  \t<oryx:magnet oryx:cx="50" oryx:cy="40" oryx:default="yes" />\n  </oryx:magnets>\n  <g pointer-events="fill" oryx:minimumSize="50 40">\n\t<rect id="text_frame" oryx:anchors="bottom top right left" x="1" y="1" width="94" height="79" rx="10" ry="10" stroke="none" stroke-width="0" fill="none" />\n\t<rect id="bg_frame" oryx:resize="vertical horizontal" x="0" y="0" width="100" height="80" rx="10" ry="10" stroke="#bbbbbb" stroke-width="1" fill="#f9f9f9" />\n\t\t<text \n\t\t\tfont-size="12" \n\t\t\tid="text_name" \n\t\t\tx="50" \n\t\t\ty="40" \n\t\t\toryx:align="middle center"\n\t\t\toryx:fittoelem="text_frame"\n\t\t\tstroke="#373e48">\n\t\t</text>\n\t\n\t<g id="userTask" transform="translate(3,3)">\n\t\t<path oryx:anchors="top left"\n       \t\tstyle="fill:#d1b575;stroke:none;"\n       \t\t d="m 1,17 16,0 0,-1.7778 -5.333332,-3.5555 0,-1.7778 c 1.244444,0 1.244444,-2.3111 1.244444,-2.3111 l 0,-3.0222 C 12.555557,0.8221 9.0000001,1.0001 9.0000001,1.0001 c 0,0 -3.5555556,-0.178 -3.9111111,3.5555 l 0,3.0222 c 0,0 0,2.3111 1.2444443,2.3111 l 0,1.7778 L 1,15.2222 1,17 17,17" \n         />\n\t\t\n\t</g>\n  \n\t<g id="parallel">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" d="M46 70 v8 M50 70 v8 M54 70 v8" stroke-width="2" />\n\t</g>\n\t\n\t<g id="sequential">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" stroke-width="2" d="M46,76h10M46,72h10 M46,68h10"/>\n\t</g>\n\t\n\n\t<g id="compensation">\n\t\t<path oryx:anchors="bottom" fill="none" stroke="#bbbbbb" d="M 62 74 L 66 70 L 66 78 L 62 74 L 62 70 L 58 74 L 62 78 L 62 74" stroke-width="1" />\n\t</g>\n  </g>\n</svg>'
                        }
                    ],
                    show: true
                },
                {
                    title: "网关",
                    listData: [
                        {
                            id: "ExclusiveGateway",
                            title: "分支",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   xmlns:svg="http://www.w3.org/2000/svg"\n   xmlns="http://www.w3.org/2000/svg"\n   version="1.0"\n   width="40"\n   height="40">\n  <defs\n     id="defs4" />\n  <oryx:magnets>\n    <oryx:magnet\n       oryx:default="yes"\n       oryx:cy="16"\n       oryx:cx="16" />\n  </oryx:magnets>\t\t\t\t\t\n  <g>\n  \n    <path\n       d="M -4.5,16 L 16,-4.5 L 35.5,16 L 16,35.5z"\n       id="bg_frame"\n       fill="#ffffff"\n       stroke="#585858"\n       style="stroke-width:1" />\n    <g\n       id="cross">\n      <path\n      \tid="crosspath"\n      \tstroke="#585858"\n      \tfill="#585858"\n        d="M 8.75,7.55 L 12.75,7.55 L 23.15,24.45 L 19.25,24.45 z"\n        style="stroke-width:1" />\n      <path\n      \tid="crosspath2"\n      \tstroke="#585858"\n      \tfill="#585858"\n        d="M 8.75,24.45 L 19.25,7.55 L 23.15,7.55 L 12.75,24.45 z"\n        style="stroke-width:1" />\n    </g>\n\t\n\t<text id="text_name" x="26" y="26" oryx:align="left top"/>\n\t\n  </g>\n</svg>\n'
                        },
                        {
                            id: "ParallelGateway",
                            title: "合并",
                            view:
                                '<?xml version="1.0" encoding="UTF-8" standalone="no"?>\n<svg\n   xmlns:oryx="http://www.b3mn.org/oryx"\n   xmlns:svg="http://www.w3.org/2000/svg"\n   xmlns="http://www.w3.org/2000/svg"\n   version="1.0"\n   width="40"\n   height="40">\n   \n  <oryx:magnets>\n    <oryx:magnet\n       oryx:default="yes"\n       oryx:cy="16"\n       oryx:cx="16" />\n  </oryx:magnets>\n  <g>\n    <path\n       d="M -4.5,16 L 16,-4.5 L 35.5,16 L 16,35.5z"\n       id="bg_frame"\n       fill="#ffffff"\n       stroke="#585858"\n       style="stroke-width:1" />\n    <path\n       d="M 6.75,16 L 25.75,16 M 16,6.75 L 16,25.75"\n       id="path9"\n       stroke="#585858"\n       style="fill:none;stroke-width:3" />\n    \n\t<text id="text_name" x="26" y="26" oryx:align="left top"/>\n\t\n  </g>\n</svg>\n'
                        }
                    ],
                    show: true
                }
            ],
            userImages: []
        };
    },
    computed: {
        searchToolItems() {
            if (this.searchCont === "") {
                return this.toolItems;
            }
        }
    },
    methods: {
        ...mapMutations("editor", [
            "SEL_NODETYPE",
            "SET_DRAGGING",
            "SET_INDRAWAREA"
        ]),
        selNode(type) {
            this.SEL_NODETYPE(type);
            event.dataTransfer.setData("Text", "add");
        },
        nodeDragEnd() {
            if (this.selNodeType) {
                this.SEL_NODETYPE("");
            }
        },
        dropHandle(e) {
            //let reader = new
        },
        dragoverHandle() {},
        dragstart(imgSrc) {
            event.dataTransfer.setData("URL", imgSrc);
            event.dataTransfer.setData("Text", "add");
        },
        init() {
            axios({
                method: "get",
                url: "static/js/stencilset_bpmn.json"
            }).then(res => {
                if (res != undefined) {
                    console.log(res);
                    const stencils = res.data.stencils;
                    let quickMenuDefinition = [
                        "UserTask",
                        "EndNoneEvent",
                        "ExclusiveGateway",
                        "CatchTimerEvent",
                        "ThrowNoneEvent",
                        "TextAnnotion",
                        "SequenceFlow",
                        "Association"
                    ];
                    let ignoreForPaletteDefinition = [
                        "SequenceFlow",
                        "MessageFlow",
                        "Association",
                        "DataAssociation",
                        "DataStore",
                        "SendTask"
                    ];
                    let quickMenuItems = [];
                    let morphRoles = [];
                    for (var i = 0; i < res.data.rules.morphRoles.length; i++) {
                        let role = res.data.rules.morphRoles[i].role;
                        let roleItem = { role: role, morphOptions: [] };
                        morphRoles.push(roleItem);
                    }
                }
            });
        }
    },
    mounted() {
        this.init();
    }
};
</script>

<style lang="scss">
.editor-left-tool {
    width: 208px;
    position: absolute;
    top: 66px;
    bottom: 0;
    background: whitesmoke;
    overflow: auto;
    box-shadow: -1px 0px 5px #bbb inset;
    border-right: 1px solid #ddd;
    .flt-container {
        .flt-list-tit {
            color: #333;
            background: #eee;
            padding: 6px 0px 6px 14px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            line-height: 1.4em;
            font-size: 9pt;
            display: flex;
            align-items: center;
            cursor: pointer;
        }
        ul {
            margin-right: 3px;
        }
        .flt-icon {
            margin-right: 8px;
        }
        .flt-item-list {
            // display: flex;
            flex-wrap: wrap;
            background: #f5f5f5;
            .flt-item-box {
                // display: flex;
                padding: 1px;
                flex-wrap: wrap;
                align-items: center;
                cursor: move;
                .flt-list-item {
                    line-height: 30px;
                    overflow: hidden;
                    cursor: move;
                    background: none;
                    margin-left: 10px;
                }
            }
        }
    }
}
</style>
