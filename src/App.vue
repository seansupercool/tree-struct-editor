<template>
  <div class="tree-container">
    <Draggable class="mtl-tree" v-model="treeData" treeLine>
      <template #default="{ node, stat }">
        <div class="tree-node-wrapper">
          
          <!-- 展開/收合 -->
          <OpenIcon
            v-if="node.children && node.children.length"
            :open="stat.open"
            class="open-icon"
            @click="stat.open = !stat.open"
          />
          <div v-else class="icon-spacer"></div>

          <!-- 節點內容 -->
          <div class="node-content">

            <!-- 動態欄位 -->
            <div class="field-group">
              <template v-for="field in fieldList" :key="field">
                <input
                  v-if="node[field].visible"
                  v-model="node[field].value"
                  :placeholder="node[field].placeholder"
                  class="field-input"
                />
              </template>
            </div>

            <!-- 操作按鈕 -->
            <div class="action-buttons">

              <!-- 新增 -->
              <button
                @click="addChild(node)"
                class="btn btn-add"
                title="新增子節點"
              >
                新增
              </button>

              <!-- 刪除 -->
              <button
                @click="deleteNode(node)"
                class="btn btn-delete"
                title="刪除節點"
              >
                刪除
              </button>

            </div>

          </div>
        </div>
      </template>
    </Draggable>
  </div>
</template>

<script>
import { Draggable, OpenIcon } from "@he-tree/vue";
import "@he-tree/vue/style/default.css";
import "@he-tree/vue/style/material-design.css";

export default {
  components: { Draggable, OpenIcon },

  data() {
    return {
      fieldList: ["name", "nameCN", "dataType", "content"],

      treeData: [
        {
          text: "Projects",
          name: { value: "", placeholder: "欄位名稱", visible: true },
          nameCN: { value: "", placeholder: "欄位中文", visible: false },
          dataType: { value: "", placeholder: "資料型態", visible: true },
          content: { value: "", placeholder: "資料內容", visible: true },
          children: [
            {
              text: "Frontend",
              name: { value: "", placeholder: "欄位名稱", visible: true },
              nameCN: { value: "", placeholder: "欄位中文", visible: true },
              dataType: { value: "", placeholder: "資料型態", visible: true },
              content: { value: "", placeholder: "資料內容", visible: true },
              children: [
                {
                  text: "Vue",
                  name: { value: "", placeholder: "欄位名稱", visible: true },
                  nameCN: { value: "", placeholder: "欄位中文", visible: true },
                  dataType: { value: "", placeholder: "資料型態", visible: true },
                  content: { value: "", placeholder: "資料內容", visible: true },
                  children: [
                    {
                      text: "Nuxt",
                      name: { value: "", placeholder: "欄位名稱", visible: true },
                      nameCN: { value: "", placeholder: "欄位中文", visible: true },
                      dataType: { value: "", placeholder: "資料型態", visible: true },
                      content: { value: "", placeholder: "資料內容", visible: true },
                    },
                  ],
                },
              ],
            },
          ],
        },
      ],

      nodeCounter: 1,
    };
  },

  methods: {
    /** Vue3 正確新增方式：直接操作 node.children */
    addChild(node) {
      if (!node.children) node.children = [];

      node.children.push({
        text: `新節點 ${this.nodeCounter++}`,
        name: { value: "", placeholder: "欄位名稱", visible: true },
        nameCN: { value: "", placeholder: "欄位中文", visible: true },
        dataType: { value: "", placeholder: "資料型態", visible: true },
        content: { value: "", placeholder: "資料內容", visible: true },
        children: []
      });
      this.treeData = [...this.treeData];
    },

    /** Vue3 正確刪除方式：遞迴從 treeData 找到並刪除 */
    deleteNode(node) {
      this.removeNodeRecursive(this.treeData, node);
      this.treeData = [...this.treeData];
    },

    removeNodeRecursive(list, target) {
      for (let i = 0; i < list.length; i++) {
        const item = list[i];

        if (item === target) {
          list.splice(i, 1);
          return true;
        }

        if (item.children && item.children.length) {
          const removed = this.removeNodeRecursive(item.children, target);
          if (removed) return true;
        }
      }
      return false;
    },
  },
};
</script>

<style scoped>
/* 保留你的全部 CSS */
.tree-container {
  padding: 20px;
  background: #f5f7fa;
  min-height: 100vh;
}

.tree-node-wrapper {
  display: flex;
  align-items: flex-start;
}

.open-icon {
  margin-top: 8px;
  cursor: pointer;
}

.icon-spacer {
  width: 24px;
}

.node-content {
  display: flex;
  flex: 1;
  border: 1px solid #e1e8ed;
  border-radius: 8px;
  padding: 5px;
  margin-left: 8px;
}

.field-group {
  display: flex;
  gap: 10px;
  margin-right: 10px;
}

.field-input {
  padding: 8px;
}

.action-buttons {
  display: flex;
  gap: 10px;
}

.btn {
  padding: 6px 10px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.btn-add {
  background: #48bb78;
  color: white;
}

.btn-delete {
  background: #f56565;
  color: white;
}
</style>
