<!--
/**
 * null
 * email: @@@
 * website: https://#
 * date: 2021.08.18
 *  
 */
-->

<template>
  <container-wrapper :designer="designer"
                     :widget="widget"
                     :parent-widget="parentWidget"
                     :parent-list="parentList"
                     :index-of-parent-list="indexOfParentList">

    <div :key="widget.id"
         class="dialog-container"
         :class="{'selected': selected}"
         @click.stop="selectWidget(widget)">

      <draggable :list="widget.widgetList"
                 item-key="id"
                 v-bind="{group:'dragGroup', ghostClass: 'ghost',animation: 200}"
                 handle=".drag-handler"
                 tag="transition-group"
                 :component-data="{name: 'fade'}"
                 @add="(evt) => onContainerDragAdd(evt, widget.widgetList)"
                 @update="onContainerDragUpdate"
                 :move="checkContainerMove">
        <template #item="{ element: subWidget, index: swIdx }">
          <div class="form-widget-list">
            <template v-if="'container' === subWidget.category">
              <component :is="subWidget.type + '-widget'"
                         :widget="subWidget"
                         :designer="designer"
                         :key="subWidget.id"
                         :parent-list="widget.widgetList"
                         :index-of-parent-list="swIdx"
                         :parent-widget="widget"></component>
            </template>
            <template v-else>
              <component :is="subWidget.type + '-widget'"
                         :field="subWidget"
                         :designer="designer"
                         :key="subWidget.id"
                         :parent-list="widget.widgetList"
                         :index-of-parent-list="swIdx"
                         :parent-widget="widget"
                         :design-state="true"></component>
            </template>
          </div>
        </template>
      </draggable>

    </div>

  </container-wrapper>
</template>

<script>
import i18n from '@/utils/i18n'
import containerMixin from '@/components/form-designer/form-widget/container-widget/containerMixin'
import ContainerWrapper from '@/components/form-designer/form-widget/container-widget/container-wrapper'
import FieldComponents from '@/components/form-designer/form-widget/field-widget/index'
import refMixinDesign from '@/components/form-designer/refMixinDesign'

export default {
  name: 'dialog-widget',
  componentName: 'ContainerWidget',
  mixins: [i18n, containerMixin, refMixinDesign],
  inject: ['refList'],
  components: {
    ContainerWrapper,
    ...FieldComponents,
  },
  props: {
    widget: Object,
    parentWidget: Object,
    parentList: Array,
    indexOfParentList: Number,
    designer: Object,
  },
  data() {
    return {}
  },
  computed: {
    selected() {
      return this.widget.id === this.designer.selectedId
    },

    customClass() {
      return this.widget.options.customClass || ''
    },
  },
  watch: {
    //
  },
  created() {
    this.initRefList()
  },
  mounted() {
    //
    console.log('widget', this.widget)
  },
  methods: {},
}
</script>

<style lang="scss" scoped>
.dialog-container {
  //padding: 5px;
  margin: 2px;
  outline: 2px dashed #e6e7e9;
  min-height: 80px;
  padding: 10px 0;

  .form-widget-list {
    min-height: 28px;
  }

  :deep(.el-tabs__content) {
    min-height: 28px;
  }
}

.tab-container.selected {
  outline: 2px solid $--color-primary !important;
}
</style>
