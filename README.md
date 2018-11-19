# jQuery Chosen Wrapper for Vue

https://harvesthq.github.io/chosen/

## Example
```js
new Vue({
  el: "#app",
  data: {
    item: null,
    list: [
      {id: 1, label: 'First'},
      {id: 2, label: 'Second'},
      {id: 3, label: 'Third'},
    ],
    list2: [
      {anotherKey: 1, anotherLabel: 'First'},
      {anotherKey: 2, anotherLabel: 'Second'},
      {anotherKey: 3, anotherLabel: 'Third'},
    ],
    list3: {
      1: "First",
      2: "Second",
      3: "Third"
    }
  }
})
```
```html
<vue-chosen v-model="item" :options="list"></vue-chosen>
<vue-chosen v-model="item" :options="list2" track-by="anotherKey" label="anotherLabel"></vue-chosen>
<vue-chosen v-model="item" :options="list3"></vue-chosen>
```

## License

The package is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).
