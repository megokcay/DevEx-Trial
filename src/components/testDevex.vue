<template>
    <div>
        <DxDataGrid :data-source="this.dataSource" :allow-column-reordering="true" :row-alternation-enabled="true"
            :show-borders="true">
            <DxHeaderFilter :visible="true" />
            <DxColumnChooser :enabled="true" mode="select" />
            <DxColumn data-field="name" />
            <DxColumn data-field="calories" />
            <DxColumn data-field="Discount" />
            <DxColumn data-field="fat" />
            <DxColumn data-field="carbs" />
            <DxColumn data-field="protein" />
            <DxColumn data-field="iron" />
            <DxPaging :page-size="10" />
        </DxDataGrid>
        <div class="popOutElement">
            <DxDataGrid class="popOutGrid" :data-source="this.dataSource" :allow-column-reordering="true"
                :row-alternation-enabled="true" :show-borders="true" ref="popOutGrid">
                <DxHeaderFilter :visible="true" />
                <DxColumnChooser :enabled="true" mode="select" />
                <DxColumn data-field="name" />
                <DxColumn data-field="calories" />
                <DxColumn data-field="Discount" />
                <DxColumn data-field="fat" />
                <DxColumn data-field="carbs" />
                <DxColumn data-field="protein" />
                <DxColumn data-field="iron" />
                <DxPaging :page-size="10" />
            </DxDataGrid>
        </div>
        <button type="button" v-on:click="popOutClicked">popUp</button>

    </div>
</template>
<script>

import {
    DxDataGrid,
    DxColumn,
    DxPaging,
    DxColumnChooser,
    DxHeaderFilter
} from 'devextreme-vue/data-grid';


export default {
    components: {
        DxDataGrid,
        DxColumn,
        DxPaging,
        DxColumnChooser,
        DxHeaderFilter
    },
    data() {
        return {
            dataSource: [{ name: 'Frozens Yogurt', calories: 159, fat: '6.0', carbs: 24, protein: '4.0', iron: '1%' },
            { name: 'Ice cream sandwich', calories: 237, fat: '9.0', carbs: 37, protein: '4.3', iron: '1%' },
            { name: 'Eclair', calories: 262, fat: '16.0', carbs: 23, protein: '6.0', iron: '7%' },
            { name: 'Cupcake', calories: 305, fat: '3.7', carbs: 67, protein: '4.3', iron: '8%' },
            { name: 'Gingerbread', calories: 356, fat: '16.0', carbs: 49, protein: '3.9', iron: '16%' },
            { name: 'Jelly bean', calories: 375, fat: '0.0', carbs: 94, protein: '0.0', iron: '0%' },
            { name: 'Lollipop', calories: 392, fat: '0.2', carbs: 98, protein: '0.0', iron: '2 %' },
            { name: 'Honeycomb', calories: 408, fat: '3.2', carbs: 87, protein: '6.5', iron: '45%' },
            { name: 'Donut', calories: 452, fat: '25.0', carbs: 51, protein: '4.9', iron: '22%' },
            { name: 'KitKat', calories: 518, fat: '26.0', carbs: 65, protein: '7', iron: '6 %' }],
            pageSizes: [10, 25, 50, 100],
            childWindows: []
        };
    },
    methods: {
        popOutClicked() {
            const self = this;
            const mainWindow = window;
            const newWindow = window.open("", "", "height=700,width=1000");
            mainWindow.childWindow = newWindow;
            self.childWindows.push(newWindow);
            if (newWindow) {
                newWindow.onload = () => {
                    if (newWindow) {
                        if (newWindow.ranOnce) {
                            return;
                        } else {
                            newWindow.ranOnce = true;
                        }
                        const popoutOuterElement = document.getElementsByClassName("popOutGrid")[0];
                        const div = document.createElement("DIV");
                        newWindow.document.write(`<html><head><title>popout</title></head><body></body></html>`);
                        newWindow.document.body.append(div);
                        div.style.width = "100%";
                        div.style.height = "100%";
                        div.style.overflow = "scroll";
                        div.style.display = "grid";
                        div.append(popoutOuterElement);
                        const currentPageSChildren = document.head.children;
                        for (let i = 0; i < currentPageSChildren.length; i++) {
                            let child = currentPageSChildren[i];
                            if (child.tagName == "LINK") {
                                let AChild = child;
                                const cssLink = newWindow.document.createElement("link");
                                cssLink.rel = "stylesheet";
                                cssLink.type = "text/css";
                                cssLink.href = AChild.href;
                                newWindow.document.head.appendChild(cssLink);
                            } else if (child.tagName == "STYLE") {
                                const style = newWindow.document.createElement("style");
                                style.type = "text/css";
                                style.innerHTML = child.innerHTML;
                                newWindow.document.head.appendChild(style);
                            }
                        }
                        self.overrideAppendChild();
                        newWindow.onbeforeunload = function () {
                            const popinDiv = mainWindow.document.getElementsByClassName("popOutElement")[0];
                            popinDiv.appendChild(newWindow.document.getElementsByClassName("popOutGrid")[0]);
                            self.childWindows = [];
                        };
                    }

                };
                if (newWindow.document && newWindow.document.readyState == "complete" /* ah chrome ah */) {
                    newWindow.onload();
                }
            }
        },
        overrideAppendChild() {
            const self = this;
            const childWindows = self.childWindows;
            const currDocument = window.document;
            (function (origAppendChild) {
                currDocument.body.appendChild = (node) => {
                    if (currDocument.hasFocus()) {
                        origAppendChild.apply(window.document.body, [node]);
                        return node;
                    }
                    for (let i = 0; i < childWindows.length; i++) {
                        if (childWindows[i].document.hasFocus()) {
                            childWindows[i].document.body.appendChild(node);
                            break;
                        }
                    }
                    return node;
                };


            })(currDocument.body.appendChild);
        }
    }
}


</script>
