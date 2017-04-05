<template>

  <div
    v-on:click="focusNewTag()"
    :class="[
      'vue-input-tag-wrapper ',
      readOnly ? 'read-only' : ''
    ]"
  >
    <span 
      v-for="(tag, index) in attachedTags" 
      :key="index"
      class="input-tag"
    >
      <span>{{ tag }}</span>
      <a 
        v-if="!readOnly" 
        v-on:click.prevent.stop="remove(index)" 
        class="remove"
      ></a>
    </span>
    <input 
      v-if="!readOnly" 
      v-model="newTag" 
      v-on:keydown.delete.stop="removeLastTag()" 
      v-on:keydown.enter.prevent.stop="addNew(newTag)" 
      :placeholder="query" 
      type="text" 
      class="new-tag"
      style="width: auto;"
    />
  </div>
</template>

<script>
/*eslint-disable*/
  const validators = {
    email : new RegExp(/^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/),
    url : new RegExp(/^(https?|ftp|rmtp|mms):\/\/(([A-Z0-9][A-Z0-9_-]*)(\.[A-Z0-9][A-Z0-9_-]*)+)(:(\d+))?\/?/i),
    text : new RegExp(/^[a-zA-Z]+$/),
    digits : new RegExp(/^[\d() \.\:\-\+#]+$/),
    isodate : new RegExp(/^\d{4}[\/\-](0?[1-9]|1[012])[\/\-](0?[1-9]|[12][0-9]|3[01])$/)
  }
  /*eslint-enable*/
  export default {
    name: 'InputTag',

    props: {
      attachedTags: {
        type: Array,
        default: () => [],
      },
      allTags: {
        type: Array,
        default: () => [],
      },
      placeholder: {
        type: String,
        default: '',
      },
      readOnly: {
        type: Boolean,
        default: false,
      },
      validate: {
        type: String,
        default: '',
      },
    },
    data() {
      return {
        newTag: '',
      };
    },
    methods: {
      focusNewTag() {
        if (this.readOnly) { return; }
        this.$el.querySelector('.new-tag').focus();
      },
      addNew(tag) {
        if (tag && !this.attachedTags.includes(tag) && this.validateIfNeeded(tag)) {
          this.$emit('addTag', tag); 
        }
        this.newTag = '';
      },
      validateIfNeeded(tagValue) {
        if (this.validate === '' || this.validate === undefined) {
          return true;
        } else if (Object.keys(validators).indexOf(this.validate) > -1) {
          return validators[this.validate].test(tagValue);
        }
        return true;
      },
      remove(index) {
        this.$emit('removeTag', this.attachedTags[index])
      },
      removeLastTag() {
        if (this.newTag) { return; }
        this.$emit('removeTag', this.attachedTags.pop())
      },
      tagChange() {
        if (this.onChange) {
          // avoid passing the observer
          this.onChange(JSON.parse(JSON.stringify(this.attachedTags)));
        }
      }
    },
    computed: {
      query () {
        if (this.newTag === '') {
          return this.placeholder
        }
        return this.newTag 
      },
      tagSearch () {
        if (this.newTag === '') {
          return []
        }

        let regex = new RegExp('^.*' + this.newTag + '.*$', 'i')
        let list = []
        for (let i = 0; i < this.allTags.length; i++) {
          if (!regex.test(this.allTags[i].title)) {
            continue
          }
          list.push(this.allTags[i].title)
        }
        return list
      }
    } 
  };
</script>
<style scoped>

  .vue-input-tag-wrapper {
    background-color: #fff;
    overflow: hidden;
    padding-left: 4px;
    padding-top: 4px;
    cursor: text;
    text-align: right;
    -webkit-appearance: textfield;
  }

  .vue-input-tag-wrapper .input-tag {
    background-color: #cde69c;
    border-radius: 2px;
    border: 1px solid #a5d24a;
    color: #638421;
    display: inline-block;
    font-size: 13px;
    font-weight: 400;
    margin-bottom: 4px;
    margin-right: 4px;
    padding: 3px;
  }

  .vue-input-tag-wrapper .input-tag .remove {
    cursor: pointer;
    font-weight: bold;
    color: #638421;
  }

  .vue-input-tag-wrapper .input-tag .remove:hover {
    text-decoration: none;
  }

  .vue-input-tag-wrapper .input-tag .remove::before {
    content: " x";
  }

  .vue-input-tag-wrapper .new-tag {
    background: transparent;
    border: 0;
    color: #777;
    font-size: 13px;
    font-weight: 400;
    margin-bottom: 6px;
    margin-top: 1px;
    outline: none;
    padding: 4px;
    padding-left: 0;
    width: 80px;
  }

  .vue-input-tag-wrapper.read-only {
    cursor: default;
  }

</style>
