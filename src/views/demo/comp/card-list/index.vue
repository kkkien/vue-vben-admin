<template>
  <PageWrapper title="Card list example" content="Basic package">
    <CardList :params="params" :api="demoListApi" @get-method="getMethod" @delete="handleDel">
      <template #header>
        <Button type="primary" color="error"> Button 1 </Button>
        <Button type="primary" color="success"> Button 2 </Button>
      </template>
    </CardList>
  </PageWrapper>
</template>
<script lang="ts" setup>
  import { CardList } from '/@/components/CardList';
  import { Button } from '/@/components/Button';
  import { PageWrapper } from '/@/components/Page';
  import { demoListApi } from '/@/api/demo/table';
  import { useMessage } from '/@/hooks/web/useMessage';

  const { notification } = useMessage();
  // With parameters when requesting api
  const params = {};

  let reload = () => {};
  // Get the internal fetch method;
  function getMethod(m: any) {
    reload = m;
  }
  //delete button event
  function handleDel(id) {
    console.log(id);
    notification.success({ message: `Successfully deleted${id}` });
    reload();
  }
</script>
