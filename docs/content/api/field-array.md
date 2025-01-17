---
title: FieldArray
description: API reference for the Field Array component
menuTitle: '<FieldArray />'
order: 4
---

# FieldArray <DocBadge title="v4.5" />

<doc-tip title="Form Context" type="warn">

The `<FieldArray />` component requires being used inside a `Form` component or a `useForm` to be called at its parent tree.

</doc-tip>

The `<FieldArray />` component is used to manage repeatable array fields. It is a renderless component, meaning it doesn't render anything of its own.

```vue
<template>
  <Form @submit="onSubmit" :initial-values="initialValues">
    <FieldArray name="links" key-path="id" v-slot="{ fields, push, remove }">
      <div v-for="(field, idx) in fields" :key="field.key">
        <Field :name="`links[${idx}].url`" type="url" />

        <button type="button" @click="remove(idx)">Remove</button>
      </div>

      <button type="button" @click="push({ id: Date.now(), name: '', url: '' })">Add</button>
    </FieldArray>

    <button>Submit</button>
  </Form>
</template>

<script>
export default {
  data: () => ({
    // you can set initial values for those array fields
    initialValues: {
      links: [{ id: 1, url: 'https://github.com/logaretm' }],
    },
  }),
  methods: {
    onSubmit(values) {
      alert(JSON.stringify(values, null, 2));
    },
  },
};
</script>
```

## API Reference

### Props

| Prop        | Type     | Required/Default | Description                            |
| :---------- | :------- | :--------------- | :------------------------------------- |
| `arrayPath` | `string` | Yes              | The form array path you wish to manage |

### Slots

#### default

The default slot gives you access to the following props:

<code-title level="4">

`fields: FieldArrayEntry<TValue>`

</code-title>

This is a **read-only** version of your array items, wrapped inside a `FieldArrayEntry` object which has the following interface:

```ts
interface FieldEntry<TValue = unknown> {
  // The actual value of the item, this is what exists in the form values
  value: TValue;
  // a value you can use as a key for iteration, generated by `key-path` prop
  key: string | number;
  // true if this is the first array item
  isFirst: boolean;
  // true if this is the last array item
  isLast: boolean;
}
```

<code-title level="4">

`push(item: any)`

</code-title>

Adds an item to the end of the array.

<code-title level="4">

`prepend(item: any)`

</code-title>

Adds an item to the start of the array.

<code-title level="4">

`remove(idx: number)`

</code-title>

Removes the item at the specified index from the array if it exists.

<code-title level="4">

`swap(idxA: number, idxB: number)`

</code-title>

Swaps the items at the given indexes with each other. Both indexes must exist in the array or it won't have an effect.

<code-title level="4">

`insert(idx: number, item: any)`

</code-title>

Adds an item at the specified index. If the specified index will place the item out of bounds (i.e: larger than length) the operation will be ignored, you still can add items as the last item of the array.

<code-title level="4">

`update(idx: number, value: any)`

</code-title>

Updates the value at the specified index, note that it doesn't merge the values if they are objects. If the specified index is outside the array boundary the operation will be ignored.

<code-title level="4">

`replace(items: any[])`

</code-title>

Replaces the entire array of fields.
