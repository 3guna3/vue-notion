<template>
  <div class="main-page">
    <div class="left-menu" @click.self="onEditNoteEnd()">
      <!-- ノートリスト -->
      <NoteItem
      v-for="note in noteList"
      :note="note"
      :layer="1"
      :key="note.id"
      @delete="onDeleteNote"
      @editStart="onEditNoteStart"
      @editEnd="onEditNoteEnd"
      @addChild="onAddChildNote"
      @addNoteAfter="onAddNoteAfter"
      />
      <!-- ノート追加ボタン -->
      <button class="transparent" @click="onClickButtonAdd">
        <i class="fas fa-plus-square"></i>ノートを追加
      </button>
    </div>
    <div class="right-view" @click.self="onEditNoteEnd()">
      右ビュー
    </div>
  </div>

</template>

<script>
import NoteItem from './parts/NoteItem.vue'

export default {
  data() {
    return {
      noteList: [],
    }
  },
  methods: {
    // ノート新規作成
    onAddNoteCommon: function(targetList, layer, index) {
      layer = layer || 1;
      const note = {
        id: new Date().getTime().toString(16),
        name: `新規ノート-${layer}-${targetList.length}`,
        mouseover: false,
        editing: false,
        children: [],
        layer: layer,
      };
      if (index == null) {
        targetList.push(note);
      } else {
        targetList.splice(index + 1, 0, note);
      }
    },
    // ノート追加
    onClickButtonAdd: function() {
      this.onAddNoteCommon(this.noteList);
    },
    // ノート削除
    onDeleteNote: function(parentNote, note) {
      const targetList = parentNote == null? this.noteList: parentNote.children;
      // indexOfで要素が何番目にあるか判定
      const index = targetList.indexOf(note);
      // spliceで該当の要素を削除
      targetList.splice(index, 1);
    },
    // ノート編集開始
    onEditNoteStart: function(editNote, parentNote) {
      const targetList = parentNote == null ? this.noteList: parentNote.children;
      for (let note of targetList) {
        note.editing = (note.id === editNote.id);
        // 子ノートを基準として再起的に呼び出し
        this.onEditNoteStart(editNote, note);
      }
    },
    // ノート編集完了
    onEditNoteEnd: function(parentNote) {
      const targetList = parentNote == null ? this.noteList: parentNote.children;
      for (let note of targetList) {
        note.editing = false;
        // 子ノートを基準として再起的に呼び出し
        this.onEditNoteEnd(note);
      }
    },
    // 子ノート追加
    onAddChildNote: function(note) {
      this.onAddNoteCommon(note.children, note.layer +1);
    },
    // 兄弟ノート追加
    onAddNoteAfter: function(parentNote, note) {
      const targetList = parentNote == null ? this.noteList: parentNote.children;
      const layer = parentNote == null ? 1 : note.layer;
      const index = targetList.indexOf(note);
      this.onAddNoteCommon(targetList, layer, index);
    }
  },
  components: {
    NoteItem,
  },
}
</script>

<style scoped lang="scss">
.main-page {
  display: flex;
  height: calc(100vh - 60px);
  .left-menu {
    width: 350px;
    background-color: #f7f6f3;
  }
  .right-view {
    flex-grow: 1;
    padding: 10px;
  }
}
</style>