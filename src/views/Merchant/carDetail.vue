<template>
  <div>
    <template v-for="(tag, index) in tags">
      <a-tooltip v-if="tag.length > 20" :key="tag" :title="tag">
        <a-tag :key="tag" :closable="index !== 0" @close="() => handleClose(tag)">
          {{ `${tag.slice(0, 20)}...` }}
        </a-tag>
      </a-tooltip>
      <a-tag v-else :key="tag" :closable="index !== 0" @close="() => handleClose(tag)">
        {{ tag }}
      </a-tag>
    </template>
    <a-input
      v-if="inputVisible"
      ref="input"
      type="text"
      size="small"
      :style="{ width: '78px' }"
      :value="inputValue"
      @change="handleInputChange"
      @blur="handleInputConfirm"
      @keyup.enter="handleInputConfirm"
    />
    <a-tag v-else style="background: #fff; borderStyle: dashed;" @click="showInput">
      <a-icon type="plus" /> New Tag
    </a-tag>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tags: ['Unremovable', 'Tag 2', 'Tag 3Tag 3Tag 3Tag 3Tag 3Tag 3Tag 3'],
      inputVisible: false,
      inputValue: '',
    };
  },
  methods: {
    handleClose(removedTag) {
      const tags = this.tags.filter(tag => tag !== removedTag);
      console.log(tags);
      this.tags = tags;
    },

    showInput() {
      this.inputVisible = true;
      this.$nextTick(function() {
        this.$refs.input.focus();
      });
    },

    handleInputChange(e) {
      this.inputValue = e.target.value;
    },

    handleInputConfirm() {
      const inputValue = this.inputValue;
      let tags = this.tags;
      if (inputValue && tags.indexOf(inputValue) === -1) {
        tags = [...tags, inputValue];
      }
      console.log(tags);
      Object.assign(this, {
        tags,
        inputVisible: false,
        inputValue: '',
      });
    },
  },
};
</script>
