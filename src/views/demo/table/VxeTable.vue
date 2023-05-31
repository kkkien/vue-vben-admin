<template>
  <PageWrapper
    title="VxeTable form"
    content="Only part of the operations are shown, please refer to the VxeTable official website case for detailed functions"
    contentFullHeight
    fixedHeight
  >
    <VxeBasicTable ref="tableRef" v-bind="gridOptions">
      <template #action="{ row }">
        <TableAction outside :actions="createActions(row)" />
      </template>
    </VxeBasicTable>
  </PageWrapper>
</template>
<script lang="ts" setup>
  import { reactive, ref } from 'vue';
  import { ActionItem, TableAction } from '/@/components/Table';
  import { PageWrapper } from '/@/components/Page';
  import { useMessage } from '/@/hooks/web/useMessage';
  import { vxeTableColumns, vxeTableFormSchema } from './tableData';
  import { BasicTableProps, VxeBasicTable, VxeGridInstance } from '/@/components/VxeTable';
  import { demoListApi } from '/@/api/demo/table';

  const { createMessage } = useMessage();

  const tableRef = ref<VxeGridInstance>();

  const gridOptions = reactive<BasicTableProps>({
    id: 'VxeTable',
    keepSource: true,
    editConfig: { trigger: 'click', mode: 'cell', showStatus: true },
    columns: vxeTableColumns,
    toolbarConfig: {
      buttons: [
        {
          content: 'Add on the first line',
          buttonRender: {
            name: 'AButton',
            props: {
              type: 'primary',
              preIcon: 'mdi:page-next-outline',
            },
            events: {
              click: () => {
                tableRef.value?.insert({ name: 'Newly added' });
                createMessage.success('Added successfully');
              },
            },
          },
        },
        {
          content: 'In the last line add',
          buttonRender: {
            name: 'AButton',
            props: {
              type: 'warning',
            },
            events: {
              click: () => {
                tableRef.value?.insertAt({ name: 'Newly added' }, -1);
              },
            },
          },
        },
      ],
    },
    formConfig: {
      enabled: true,
      items: vxeTableFormSchema,
    },
    height: 'auto',
    proxyConfig: {
      ajax: {
        query: async ({ page, form }) => {
          return demoListApi({
            pageNum: page.currentPage,
            pageSize: page.pageSize,
            ...form,
          });
        },
        queryAll: async ({ form }) => {
          return await demoListApi(form);
        },
      },
    },
  });

  // Action Button (Access Control)
  const createActions = (record) => {
    const actions: ActionItem[] = [
      {
        label: 'Deta',
        onClick: () => {
          console.log(record);
        },
      },
      {
        label: 'Edit',
        onClick: () => {},
      },
      {
        label: 'Del',
        color: 'error',
        popConfirm: {
          title: 'Are you sure to delete?',
          confirm: () => {
            tableRef.value?.remove(record);
          },
        },
      },
    ];

    return actions;
  };
</script>
