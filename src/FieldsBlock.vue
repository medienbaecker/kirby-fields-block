<template>
  <div class="k-block-fields-preview" @mousedown="preventSelect" :data-collapsed="isCollapsed">
    <k-fields-block-title
      :content="content"
      :fieldset="fieldset"
      v-if="fieldset.label === null || fieldset.label"
      @dblclick="toggle"
      @toggle="toggle"
    />
    <k-form
      ref="form"
      :autofocus="true"
      :fields="fieldset.tabs.content.fields"
      :value="$helper.clone(content)"
      @input="$emit('update', $event)"
      v-if="!isCollapsed"
    />
  </div>
</template>

<script>
export default {
  data () {
    return {
      isCollapsed: JSON.parse(sessionStorage.getItem(`kirby.fieldsBlock.${this.$attrs.endpoints.field}.${this.$attrs.id}`))
    }
  },
  created () {
    console.log(this.collapse);
  },
  methods: {
    toggle () {
      this.isCollapsed = !this.isCollapsed
      sessionStorage.setItem(`kirby.fieldsBlock.${this.$attrs.endpoints.field}.${this.$attrs.id}`, this.isCollapsed)
    },
    preventSelect (event) {
      if (event.detail > 1) {
        if(!event.target.closest('.k-input')) {
          event.preventDefault()
        }
      }
    }
  }
}
</script>

<style>

.k-blocks:has(.k-block-fields-preview) {
  background: none;
  box-shadow: none;
}

.k-block-container:has(.k-block-fields-preview) {
  border-bottom: none;
  border-radius: var(--rounded-md);
}

.k-block-container:has(.k-block-fields-preview):not(:first-of-type) {
  margin-top: 0.5rem;
}
.k-block-container:has(.k-block-fields-preview):not(:last-of-type) {
  margin-bottom: 0.5rem;
}

.k-block-fields-preview {
  margin: -0.75rem;
  overflow: hidden;
  border-radius: var(--rounded-md);
  box-shadow: var(--shadow);
}

.k-block-fields-preview .k-block-title {
  padding: 0.75rem;
  background: #f7f7f7;
}

.k-block-fields-preview:not([data-collapsed="true"]) .k-block-title {
  border-bottom: 1px solid rgba(0,0,0,.1);
}

.k-block-fields-preview .k-fields-block-toggle {
  margin-left: auto;
  cursor: pointer;
}

.k-block-fields-preview[data-collapsed="true"] .k-fields-block-toggle {
  transform: scaleY(-1);
}

.k-block-fields-preview .k-form {
  padding: 1.25rem 1.5rem 1.5rem 1.5rem;
}

/**
 * Reset `.k-layout-column .k-empty` overrides
 */

.k-block-fields-preview .k-empty {
  position: static !important;
  opacity: 1 !important;
  align-items: stretch !important;
  justify-content: flex-start !important;
  color: #777 !important;
  border: 1px dashed #ccc !important;
  border-radius: 1px !important;
}

.k-block-fields-preview .k-empty[data-layout="cards"] {
  justify-content: center !important;
}

.k-block-fields-preview .k-empty[data-layout="list"] .k-icon {
  border-right: 1px solid rgba(0,0,0,.05) !important;
}
</style>