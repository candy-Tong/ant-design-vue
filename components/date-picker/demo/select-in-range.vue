<docs>
---
order: 5
title:
  zh-CN: 选择不超过七天的范围
  en-US: Select range dates in 7 days
---

## zh-CN

这里举例如何用 `onCalendarChange` 和 `disabledDate` 来限制动态的日期区间选择。

## en-US

A example shows how to select a dynamic range by using `onCalendarChange` and `disabledDate`.

</docs>
<template>
  <a-range-picker
    :value="hackValue || value"
    :disabled-date="disabledDate"
    @change="onChange"
    @openChange="onOpenChange"
    @calendarChange="onCalendarChange"
  />
</template>
<script lang="ts">
import { Dayjs } from 'dayjs';
import { defineComponent, ref } from 'vue';
export default defineComponent({
  setup() {
    const dates = ref<Dayjs[]>([]);
    const value = ref<Dayjs[]>();
    const hackValue = ref<Dayjs[]>();

    const disabledDate = (current: Dayjs) => {
      if (!dates.value || dates.value.length === 0) {
        return false;
      }
      const tooLate = dates.value[0] && current.diff(dates.value[0], 'days') > 7;
      const tooEarly = dates.value[1] && dates.value[1].diff(current, 'days') > 7;
      return tooEarly || tooLate;
    };

    const onOpenChange = (open: boolean) => {
      if (open) {
        dates.value = [];
        hackValue.value = [];
      } else {
        hackValue.value = undefined;
      }
    };

    const onChange = (val: Dayjs[]) => {
      value.value = val;
    };

    const onCalendarChange = (val: Dayjs[]) => {
      dates.value = val;
    };

    return {
      dates,
      value,
      hackValue,
      disabledDate,
      onOpenChange,
      onChange,
      onCalendarChange,
    };
  },
});
</script>

