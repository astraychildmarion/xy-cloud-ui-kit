<template>
  <Dropdown :trigger="['click']">
    <template #overlay>
      <Menu @click="click">
        <template v-for="item in exportExcelOption" :key="item.value">
          <MenuItem>{{ item.title }}</MenuItem>
        </template>
      </Menu>
    </template>
    <Button class="xy-export-excel__button">
      Export excel
      <template #icon>
        <LoadingOutlined v-show="loading" />
        <CloudDownloadOutlined v-show="!loading" />
      </template>
    </Button>
  </Dropdown>
</template>
<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Dropdown, Button, Menu } from 'ant-design-vue';
import { CloudDownloadOutlined, LoadingOutlined } from '@ant-design/icons-vue';
import { ExportExcelDropdownData, ExportExcelMenuType } from './interface';

export default defineComponent({
  name: 'ExportButton',
  components: {
    Dropdown,
    Button,
    Menu,
    MenuItem: Menu.Item,
    CloudDownloadOutlined,
    LoadingOutlined,
  },
  emits: ['clickExport'],
  props: {
    loading: {
      required: true,
      default: false,
      type: Boolean as PropType<boolean>,
    },
    exportExcelOption: {
      type: Array as PropType<ExportExcelDropdownData[]>,
      default: () => [
        { title: 'All servers', value: '1' },
        { title: 'Current result', value: '2' },
      ],
    },
  },
  setup(props, { emit }) {
    const click = ({ key }: ExportExcelMenuType) => {
      emit('clickExport', key);
    };

    return {
      click,
    };
  },
});
</script>
<style lang="scss" scoped>
:deep(.ant-dropdown-menu-item) {
  &:hover {
    background-color: $dropdown-hover-bg;
  }
}
.xy-export-excel__button .ant-btn > .xy-export-excel__button .anticon + span,
.ant-btn > span + .anticon {
  margin-left: 0px;
}
.xy-export-excel--media-query {
  @media screen and (max-width: 1000px) {
    display: none;
  }
}
</style>
