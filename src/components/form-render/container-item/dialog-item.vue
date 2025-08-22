<template>
  <container-item-wrapper :widget="widget">

    <div :key="widget.id"
         class="dialog-container">
      <el-dialog v-model="widget.visible"
                 :title="widget.options.title"
                 :width="widget.options.dialogWidth"
                 :fullscreen="widget.options.fullscreen"
                 :center="widget.options.center"
                 :close-on-click-modal="widget.options.closeOnClickModal"
                 :show-close="widget.options.showClose"
                 :before-close="handleOnDialogBeforeClose"
                 :custom-class="widget.options.customClass"
                 :close-on-press-escape="widget.options.closeOnPressEscape"
                 @opened="handleOnDialogOpened"
                 :ref="widget.id"
                 :class="[customClass]">

        <template v-for="(subWidget, swIdx) in widget.widgetList">
          <template v-if="'container' === subWidget.category">
            <component :is="getComponentByContainer(subWidget)"
                       :widget="subWidget"
                       :key="swIdx"
                       :parent-list="widget.widgetList"
                       :index-of-parent-list="swIdx"
                       :parent-widget="widget">
              <!-- 递归传递插槽！！！ -->
              <template v-for="slot in Object.keys($slots)"
                        v-slot:[slot]="scope">
                <slot :name="slot"
                      v-bind="scope" />
              </template>
            </component>
          </template>
          <template v-else>
            <component :is="subWidget.type + '-widget'"
                       :field="subWidget"
                       :key="swIdx"
                       :parent-list="widget.widgetList"
                       :index-of-parent-list="swIdx"
                       :parent-widget="widget">
              <!-- 递归传递插槽！！！ -->
              <template v-for="slot in Object.keys($slots)"
                        v-slot:[slot]="scope">
                <slot :name="slot"
                      v-bind="scope" />
              </template>
            </component>
          </template>
        </template>

        <template #footer>
          <span class="dialog-footer">
            <el-button v-show="!widget.options.cancelButtonHidden"
                       @click="emitDialogCancelButtonClick">{{ widget.options.cancelButtonLabel || i18nt('designer.hint.confirm') }}</el-button>
            <el-button v-show="!widget.options.okButtonHidden"
                       type="primary"
                       @click="emitDialogOkButtonClick">
              {{ widget.options.okButtonLabel || i18nt('designer.hint.cancel') }}
            </el-button>
          </span>
        </template>

      </el-dialog>
    </div>

  </container-item-wrapper>
</template>

<script>
import emitter from '@/utils/emitter'
import i18n from '../../../utils/i18n'
import refMixin from '../../../components/form-render/refMixin'
import ContainerItemWrapper from './container-item-wrapper'
import containerItemMixin from './containerItemMixin'
import FieldComponents from '@/components/form-designer/form-widget/field-widget/index'

export default {
  name: 'vf-dialog-item',
  componentName: 'ContainerItem',
  mixins: [emitter, i18n, refMixin, containerItemMixin],
  components: {
    ContainerItemWrapper,
    ...FieldComponents,
  },
  props: {
    widget: Object,
  },
  inject: ['refList', 'sfRefList', 'globalModel'],
  data() {
    return {
      //
    }
  },
  computed: {},
  created() {
    this.initRefList()
  },
  mounted() {
    //
  },
  beforeUnmount() {
    this.unregisterFromRefList()
  },
  methods: {},
}
</script>

<style lang="scss" scoped>
</style>
